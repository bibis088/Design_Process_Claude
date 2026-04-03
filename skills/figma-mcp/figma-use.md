---
name: figma-use
description: "PRÉREQUIS OBLIGATOIRE — charger ce skill AVANT tout appel use_figma. Contient les règles critiques Plugin API (couleurs 0-1, fonts, FILL/HUG, page context, return IDs). Use when user says 'use figma', 'modifie figma', 'crée dans figma', 'écris dans figma', or before any use_figma call."
argument-hint: ""
agent: ui-designer
---

## Rôle
Skill fondateur — à charger **obligatoirement** avant tout appel `use_figma`. Contient les règles critiques qui s'appliquent à TOUS les scripts Plugin API. Manquer ce skill cause des bugs silencieux difficiles à déboguer.

> Source : https://github.com/figma/mcp-server-guide/blob/main/skills/figma-use/SKILL.md
> Adapté pour le process AI Assisted Design — mobile iOS/Android

---

## Règles critiques — à mémoriser avant tout script

### 1. Return et output
- **`return`** est le seul canal de sortie — jamais `figma.closePlugin()`
- `console.log()` n'est pas retourné — utiliser `return`
- **Toujours retourner les node IDs** créés ou mutés : `return { createdNodeIds: [...], mutatedNodeIds: [...] }`
- Code plain JavaScript avec `await` top-level — ne pas wrapper dans `(async () => {})()`

### 2. Couleurs — CRITIQUE
- **Plage 0–1** (pas 0–255) : rouge = `{r: 1, g: 0, b: 0}`
- Fills/strokes sont des **tableaux read-only** — cloner, modifier, réassigner

### 3. Fonts
- **Charger la font AVANT toute opération texte** : `await figma.loadFontAsync({family, style})`
- Pour "Inter" : style `"Semi Bold"` (avec espace), pas `"SemiBold"`

### 4. Auto Layout — ordre obligatoire
- `layoutSizingHorizontal/Vertical = 'FILL'` doit être défini **APRÈS** `parent.appendChild(child)`
- Définir avant l'append lève une erreur

### 5. Pages — CRITIQUE
- **`figma.currentPage` se réinitialise à la première page** entre chaque appel `use_figma`
- Utiliser `await figma.setCurrentPageAsync(page)` pour switcher (le setter sync lève une erreur)
- Si workflow multi-appels sur une page non-default : rappeler `setCurrentPageAsync` au début de chaque script

### 6. Atomicité
- Un script qui échoue **n'exécute rien** — le fichier reste intact
- En cas d'erreur : STOP → lire l'erreur → corriger → retry (ne pas retry immédiatement)

### 7. Variables
- `setBoundVariableForPaint` retourne une **NOUVELLE** paint — capturer et réassigner
- `createVariable` accepte l'objet collection (pas juste l'ID)
- Définir `variable.scopes` explicitement — ne pas laisser `ALL_SCOPES` par défaut

### 8. Positionnement
- Nouveaux nœuds top-level placés **loin de (0,0)** — scanner les enfants de page pour trouver un espace libre

### 9. Await — toujours
- Chaque Promise doit être `await`ée — `loadFontAsync`, `setCurrentPageAsync`, `importComponentByKeyAsync`, etc.

---

## Pre-flight checklist — vérifier avant chaque script

```
- [ ] Code utilise `return` (pas `figma.closePlugin()`)
- [ ] Code NON wrappé dans async IIFE
- [ ] `return` inclut les node IDs créés/mutés
- [ ] AUCUN `figma.notify()` (lève "not implemented")
- [ ] AUCUN `console.log()` comme output
- [ ] Couleurs en plage 0–1 (pas 0–255)
- [ ] Fills/strokes réassignés (pas mutés en place)
- [ ] Page switches via `await figma.setCurrentPageAsync()`
- [ ] `layoutSizingVertical/Horizontal = 'FILL'` défini APRÈS `appendChild()`
- [ ] `loadFontAsync()` appelé AVANT opérations texte
- [ ] `resize()` appelé AVANT les sizing modes
- [ ] Nœuds top-level positionnés loin de (0,0)
- [ ] Tous les async calls sont `await`és
```

---

## Récupération d'erreurs

| Message d'erreur | Cause probable | Correction |
|-----------------|---------------|-----------|
| `"not implemented"` | `figma.notify()` utilisé | Supprimer — utiliser `return` |
| `"node must be an auto-layout frame"` | `FILL`/`HUG` défini avant `appendChild` | Déplacer après `appendChild` |
| `"Setting figma.currentPage is not supported"` | Setter sync utilisé | `await figma.setCurrentPageAsync(page)` |
| Couleur hors plage | 0–255 au lieu de 0–1 | Diviser par 255 |
| `"Cannot read properties of null"` | Nœud introuvable (mauvais ID ou mauvaise page) | Vérifier le contexte de page et l'ID |
| `"The node with id X does not exist"` | Instance parent détachée silencieusement | Re-traverser depuis un frame parent stable |

---

## Workflow incrémental (bonne pratique)

```
Étape 1 : Inspecter — découvrir pages, composants, variables, conventions
Étape 2 : Créer tokens/variables → valider avec get_metadata
Étape 3 : Créer composants → valider avec get_metadata + get_screenshot
Étape 4 : Assembler layouts depuis instances → valider avec get_screenshot
Étape 5 : Vérification finale
```

**Règles :**
- 1 section/opération par appel `use_figma`
- Retourner les node IDs à chaque appel
- `get_screenshot` sur chaque section (pas seulement la page complète) — les problèmes de troncature et overlap sont invisibles à faible résolution
- Corriger avant de continuer — ne pas construire sur une base cassée

---

## Découverte des conventions avant création

**Toujours inspecter le fichier avant de créer quoi que ce soit.**

```javascript
// Lister pages et nœuds top-level
const pages = figma.root.children.map(p => `${p.name} id=${p.id} children=${p.children.length}`);
return pages.join('\n');
```

```javascript
// Lister collections de variables locales
const collections = await figma.variables.getLocalVariableCollectionsAsync();
return collections.map(c => ({ name: c.name, modes: c.modes.map(m => m.name), varCount: c.variableIds.length }));
```

```javascript
// Inspecter variables d'un écran existant (remote inclus)
const frame = figma.currentPage.findOne(n => n.name === "Existing Screen");
const varMap = new Map();
frame.findAll(() => true).forEach(node => {
  const bv = node.boundVariables;
  if (!bv) return;
  for (const [prop, binding] of Object.entries(bv)) {
    const bindings = Array.isArray(binding) ? binding : [binding];
    for (const b of bindings) {
      if (b?.id && !varMap.has(b.id)) varMap.set(b.id, b.id);
    }
  }
});
return [...varMap.values()];
```

---

## Référence docs Figma officielle
- `component-patterns.md` → import by key, findVariants, setProperties, text overrides
- `variable-patterns.md` → create/bind variables, import library variables, scopes
- `text-style-patterns.md` → create/apply text styles, type ramps
- `effect-style-patterns.md` → shadows, blurs
- `gotchas.md` → layout pitfalls (HUG/FILL, sizing order), paint/color issues

Source : https://github.com/figma/mcp-server-guide/tree/main/skills/figma-use/references
