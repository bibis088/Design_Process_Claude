---
name: figma-generate-design
description: "Crée ou met à jour des écrans complets dans Figma depuis une spec UX ou du code — en réutilisant les composants et tokens du design system (jamais de valeurs en dur). Charger figma-use en premier. Use when user says 'crée l écran dans figma', 'génère le screen', 'pousse l écran', 'build screen figma', 'crée les frames depuis la spec', or 'génère le design'."
argument-hint: "[feature-slug]/[screen-id]"
agent: ui-designer
---

## Rôle
Construire ou mettre à jour des écrans Figma complets depuis les specs UX en réutilisant systématiquement les composants et tokens du DS — jamais de valeurs hardcodées.

**OBLIGATOIRE : charger `figma-use` avant tout appel `use_figma`.**

> Source : https://github.com/figma/mcp-server-guide/blob/main/skills/figma-generate-design/SKILL.md
> Adapté pour mobile iOS/Android — pas de web app, pas de generate_figma_design

---

## Limites de ce skill

| Tâche | Skill à utiliser |
|-------|-----------------|
| Créer un écran depuis une spec UX | **ce skill** |
| Créer/éditer des composants réutilisables | `figma-use` directement |
| Générer du code SwiftUI/Compose depuis Figma | `figma-implement-design` |
| Mapper composants Figma ↔ code | `figma-code-connect` |

---

## Prérequis

- [ ] `figma-use` chargé — obligatoire avant tout `use_figma`
- [ ] Fichier Figma DS connecté avec composants publiés
- [ ] `setup-figma-tokens` exécuté — tokens disponibles
- [ ] Spec UX de l'écran disponible (`design/[feature-slug]/ux/screens/S-XX.md`)
- [ ] Navigation map `SCREENS-MAP.md` disponible

---

## Étape 1 — Comprendre l'écran avant de toucher Figma

Depuis `design/[feature-slug]/ux/screens/S-XX.md`, extraire :
1. Les sections majeures de l'écran (ex: Header, Content list, Empty state, Bottom nav)
2. Pour chaque section : les composants impliqués (depuis les specs, pas des suppositions UI)
3. L'OS de base défini dans le cadrage

---

## Étape 2 — Découvrir le DS (composants + variables + styles)

### 2a — Découvrir les composants (priorité : écrans existants)

```javascript
// Inspecter les instances d'un écran existant (méthode préférée)
const frame = figma.currentPage.findOne(n => n.name.includes("US_"));
const uniqueSets = new Map();
frame.findAll(n => n.type === "INSTANCE").forEach(inst => {
  const mc = inst.mainComponent;
  const cs = mc?.parent?.type === "COMPONENT_SET" ? mc.parent : null;
  const key = cs ? cs.key : mc?.key;
  const name = cs ? cs.name : mc?.name;
  if (key && !uniqueSets.has(key)) {
    uniqueSets.set(key, { name, key, isSet: !!cs, sampleVariant: mc.name });
  }
});
return [...uniqueSets.values()];
```

Si pas d'écran existant → `search_design_system` avec termes multiples et synonymes.

### 2b — Découvrir les variables

> ⚠️ `figma.variables.getLocalVariableCollectionsAsync()` retourne uniquement les variables **locales**.
> Si vide → les variables existent peut-être dans une library distante. Toujours aussi lancer `search_design_system` avec `includeVariables: true`.

```javascript
// Inspecter les variables bindées d'un écran existant
const frame = figma.currentPage.findOne(n => n.name.includes("US_"));
const varMap = new Map();
frame.findAll(() => true).forEach(node => {
  const bv = node.boundVariables;
  if (!bv) return;
  for (const [prop, binding] of Object.entries(bv)) {
    const bindings = Array.isArray(binding) ? binding : [binding];
    for (const b of bindings) {
      if (b?.id && !varMap.has(b.id)) varMap.set(b.id, { id: b.id });
    }
  }
});
return [...varMap.values()];
```

Pour les variables library (remote) → importer avec `figma.variables.importVariableByKeyAsync(key)`.

### 2c — Découvrir les styles (text + effect)

```javascript
const frame = figma.currentPage.findOne(n => n.name.includes("US_"));
const styles = { text: new Map(), effect: new Map() };
frame.findAll(() => true).forEach(node => {
  if ('textStyleId' in node && node.textStyleId) {
    const s = figma.getStyleById(node.textStyleId);
    if (s) styles.text.set(s.id, { name: s.name, key: s.key });
  }
  if ('effectStyleId' in node && node.effectStyleId) {
    const s = figma.getStyleById(node.effectStyleId);
    if (s) styles.effect.set(s.id, { name: s.name, key: s.key });
  }
});
return { textStyles: [...styles.text.values()], effectStyles: [...styles.effect.values()] };
```

---

## Étape 3 — Créer le frame wrapper en premier

**Ne pas construire les sections comme enfants top-level de la page et les reparenter ensuite — `appendChild()` cross-appel échoue silencieusement.**

```javascript
// Trouver un espace libre à droite des frames existantes
let maxX = 0;
for (const child of figma.currentPage.children) {
  maxX = Math.max(maxX, child.x + child.width);
}

const wrapper = figma.createFrame();
// iOS : 393pt / Android : 412dp
wrapper.name = "US_###_[nom]_ios_default";  // snake_case obligatoire
wrapper.layoutMode = "VERTICAL";
wrapper.primaryAxisAlignItems = "MIN";
wrapper.counterAxisAlignItems = "CENTER";
wrapper.resize(393, 852);  // taille OS de base
wrapper.layoutSizingHorizontal = "FIXED";
wrapper.layoutSizingVertical = "FIXED";
wrapper.x = maxX + 200;
wrapper.y = 0;

return { success: true, wrapperId: wrapper.id };
```

---

## Étape 4 — Construire chaque section dans le wrapper

**1 section par appel `use_figma`. Valider avec `get_screenshot` après chaque section.**

```javascript
// Récupérer le wrapper depuis l'appel précédent
const wrapper = await figma.getNodeByIdAsync("WRAPPER_ID");

// Importer un composant DS
const buttonSet = await figma.importComponentSetByKeyAsync("BUTTON_SET_KEY");
const primaryButton = buttonSet.children.find(c =>
  c.type === "COMPONENT" && c.name.includes("variant=primary")
) || buttonSet.defaultVariant;

// Importer une variable DS
const bgVar = await figma.variables.importVariableByKeyAsync("BG_COLOR_VAR_KEY");

// Créer la section
const section = figma.createFrame();
section.name = "header";  // snake_case
section.layoutMode = "HORIZONTAL";
const bgPaint = figma.variables.setBoundVariableForPaint(
  { type: 'SOLID', color: { r: 0, g: 0, b: 0 } }, 'color', bgVar
);
section.fills = [bgPaint];

// Créer instance et overrider le texte via les propriétés du composant
const btnInstance = primaryButton.createInstance();
section.appendChild(btnInstance);
btnInstance.setProperties({ "Label#2:0": "Se connecter" });  // clé depuis la map composants

wrapper.appendChild(section);
section.layoutSizingHorizontal = "FILL";  // APRÈS appendChild

return { success: true, createdNodeIds: [section.id, btnInstance.id] };
```

### Règle de nommage mobile
- Frame/section : `snake_case` — `header`, `content_list`, `empty_state`
- Frame wrapper : convention complète `US_###_[nom]_[os]_[état]`
- Aucun nom auto Figma (Frame 1, Group 7, Rectangle...)

### Ce qu'on construit manuellement vs ce qu'on importe
| Construire manuellement | Importer du DS |
|------------------------|----------------|
| Frame wrapper | Composants : buttons, inputs, cards, nav... |
| Frames de section | Variables : couleurs, spacing, radius |
| Layout containers | Text styles : ios/type/*, android/type/* |
| | Effect styles : shadows, blur |

**Jamais de valeur hex ou de pixel en dur quand un token DS existe.**

---

## Étape 5 — Valider l'écran complet

```
get_screenshot [node_id wrapper]
→ Vérifier par section (pas uniquement la vue complète)
→ Points à contrôler :
  - Texte tronqué ou coupé (line-height)
  - Éléments qui se chevauchent
  - Texte placeholder non remplacé ("Title", "Button", "Label")
  - Mauvaise variante composant (ex: Neutral au lieu de Primary)
  - Couleurs non bindées (hex en dur)
  - Zones tactiles iOS ≥ 44pt / Android ≥ 48dp
```

---

## Étape 6 — Mise à jour d'un écran existant

```javascript
// Swapper une variante de composant
const btn = await figma.getNodeByIdAsync("EXISTING_BUTTON_ID");
if (btn?.type === "INSTANCE") {
  const buttonSet = await figma.importComponentSetByKeyAsync("BUTTON_SET_KEY");
  const newVariant = buttonSet.children.find(c =>
    c.name.includes("variant=primary") && c.name.includes("state=default")
  ) || buttonSet.defaultVariant;
  btn.swapComponent(newVariant);
}
return { success: true, mutatedNodeIds: [btn.id] };
```

---

## Phase de validation

1. Aucune valeur hex ou pixel en dur — 100% tokens DS ? (oui / non + éléments à corriger)
2. Toutes les instances sont des composants DS importés — aucun élément dessiné manuellement ? (oui / non)
3. Nomenclature snake_case sur tous les layers et la frame wrapper ? (oui / non)
4. Zones tactiles ≥ 44pt iOS / 48dp Android ? (oui / non — RAAM 13.1)
5. Screenshot validé — 0 texte tronqué, 0 overlap, 0 placeholder non remplacé ? (oui / non)

## Résumé de fin d'exécution

```
✅ figma-generate-design "$ARGUMENTS" terminé
🎨 Figma → Work In Progress UI — frame US_###_[nom]_[os]_default créée
   Sections : [N] sections construites
   Composants DS importés : [N]
   Tokens bindés : [N] (0 valeur en dur)

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]

Prochaines étapes :
→ /figma-generate-design $ARGUMENTS (états dégradés : loading, empty, error)
→ /write-component $ARGUMENTS/[composant-manquant] (si composant absent du DS)
```
