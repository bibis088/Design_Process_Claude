---
name: audit-figma-existing
description: Revue complète d'un fichier Figma existant avant intégration du process — écrans, design system, composants, tokens, sémantique, nomenclature, auto layout, accessibilité, dark mode, Code Connect. Produit un rapport scoré avec plan de correction priorisé. Exécuté par le design-system-manager.
argument-hint: [figma-url]
disable-model-invocation: false
context: fork
agent: design-system-manager
---

## Rôle
Auditer de façon exhaustive un fichier Figma existant avant d'y intégrer le process — identifier les écarts avec `rules/figma.md`, scorer chaque dimension, et produire un plan de correction P1/P2/P3.

## Agents consommateurs
- Design System Manager (pilote — conduit l'audit)
- UX Designer (contributeur — valide la partie écrans/flows)
- UI Designer (contributeur — valide la partie composants)

## Prérequis
- [ ] URL du fichier Figma existant fournie par le Product Designer
- [ ] MCP Figma connecté (lecture suffisante — Dev Seat accepté)
- [ ] `rules/figma.md` consulté comme référentiel de conformité
- [ ] Validation humaine pour démarrer l'audit

## Gestion des erreurs

Si le fichier est inaccessible :
> ❌ Fichier Figma inaccessible — vérifier les permissions de partage et l'URL.

Si le fichier est trop volumineux pour `get_design_context` :
> ⚠️ Fichier volumineux — utiliser `get_metadata` page par page, puis `get_design_context` sur les frames échantillonnées.

---

## Processus d'audit

### Étape 0 — Initialisation

```
whoami → vérifier seat type et permissions
get_metadata [URL] → vue d'ensemble : pages, nombre de frames, layers
```

Produire l'inventaire initial :
```markdown
## Inventaire initial
- Pages : [N] — [liste des noms]
- Frames : [N total]
- Composants locaux : [N]
- Styles de texte : [N]
- Variables / styles de couleur : [N]
- Seat type : [Full / Dev]
```

---

### Étape 1 — Audit structure des fichiers et pages

**Référence :** `rules/figma.md` — Section "Structure des fichiers Figma"

```
get_metadata [URL] → liste des pages
```

```markdown
## 1. Structure des fichiers et pages

### Pages existantes
| Page trouvée | Page attendue (rules/figma.md) | Conforme | Écart |
|-------------|-------------------------------|---------|-------|
| [Nom page] | `🎨 Foundations` | ✅/❌ | [description] |
| [Nom page] | `🧩 Components` | ✅/❌ | [description] |
| [Nom page] | `📐 Patterns` | ✅/❌ | [description] |
| [Nom page] | `📖 Documentation` | ✅/❌ | [description] |
| [Nom page] | `🗺️ User Flows` | ✅/❌ | [description] |
| [Nom page] | `📱 Screens` | ✅/❌ | [description] |
| [Nom page] | `🚀 Handoff` | ✅/❌ | [description] |
| [Nom page] | `📦 Store Assets` | ✅/❌ | [description] |

### Score structure : [N/8] pages conformes
### Observations
- [Observation 1]
```

---

### Étape 2 — Audit nomenclature frames et layers

**Référence :** `rules/figma.md` — Section "Nomenclature des frames" et "Nomenclature des layers"

```
get_metadata [URL page Screens] → liste des frames et layers
```

Vérifier la convention `[US-###] [NomUS] / [Plateforme] / [État]` :

```markdown
## 2. Nomenclature frames et layers

### Frames — échantillon analysé (20 frames)
| Frame trouvée | Conforme à la convention | Problème |
|--------------|------------------------|---------|
| [Nom] | ✅/❌ | [ex: plateforme manquante] |

### Layers — problèmes détectés
| Problème | Fréquence | Exemple | Impact |
|---------|-----------|---------|--------|
| Noms auto Figma (`Rectangle 1`, `Group 4`) | [N occurrences] | [Frame] | Haut |
| Calques non nommés | [N] | [Frame] | Haut |
| Majuscules aléatoires | [N] | [Frame] | Moyen |
| Noms trop génériques | [N] | [Frame] | Moyen |

### Score nomenclature : [N]% de frames conformes
```

---

### Étape 3 — Audit tokens et sémantique

**Référence :** `rules/figma.md` — Section "Tokens", `design-system/tokens/`

```
get_variable_defs [URL sélection représentative]
→ extraire toutes les variables et styles définis
```

```markdown
## 3. Tokens et sémantique

### Variables de couleur
| Constat | Nombre | Exemples | Gravité |
|---------|--------|---------|---------|
| Tokens sémantiques définis | [N] | `color/content/primary` | — |
| Primitives exposées directement | [N] | `blue/500` utilisé dans composants | 🔴 Critique |
| Valeurs hex en dur (pas de variable) | [N] | `#007AFF` hardcodé | 🔴 Critique |
| Tokens sans mode dark | [N] | [liste] | 🟡 Majeur |
| Tokens sans usage documenté | [N] | [liste] | 🟢 Mineur |

### Typographie
| Constat | Nombre | Gravité |
|---------|--------|---------|
| Styles de texte définis | [N] | — |
| Tailles en dur (pas de style) | [N] | 🔴 Critique |
| Styles iOS et Android séparés | ✅/❌ | 🟡 Majeur si absent |
| Styles nommés selon convention `iOS/type/body` | ✅/❌ | 🟢 Mineur |

### Spacing
| Constat | Nombre | Gravité |
|---------|--------|---------|
| Variables de spacing définies | [N] | — |
| Valeurs en dur dans auto layout | [N occurrences] | 🔴 Critique |
| Base unit 8pt/dp respectée | ✅/❌ | 🟡 Majeur |

### Radius et shadows
| Constat | Nombre | Gravité |
|---------|--------|---------|
| Variables de radius | [N] | — |
| Radius en dur | [N] | 🟡 Majeur |
| Styles d'ombres définis | [N] | — |

### Score tokens : [N]% de tokens sémantiques vs valeurs en dur
```

---

### Étape 4 — Audit composants et bibliothèque

**Référence :** `rules/figma.md` — Section "Nomenclature des composants"

```
get_metadata [URL page Components]
search_design_system "[terme générique]" → inventaire bibliothèque
```

```markdown
## 4. Composants et bibliothèque

### Inventaire composants
| Catégorie | Nombre | Publiés en biblio | Avec variants |
|-----------|--------|------------------|--------------|
| Boutons | [N] | ✅/❌ | ✅/❌ |
| Inputs | [N] | ✅/❌ | ✅/❌ |
| Cards | [N] | ✅/❌ | ✅/❌ |
| Navigation | [N] | ✅/❌ | ✅/❌ |
| Feedback (toasts, alerts) | [N] | ✅/❌ | ✅/❌ |

### Qualité des composants — échantillon (10 composants)
| Composant | Auto layout | Tokens vs dur | Variants | Nomenclature | États couverts |
|-----------|-------------|--------------|---------|--------------|---------------|
| [Nom] | ✅/❌ | ✅/❌ | ✅/❌ | ✅/❌ | ✅/❌ |

### Composants manquants (vs standards du secteur)
- [ ] [Composant manquant 1]
- [ ] [Composant manquant 2]

### Score composants : [N/10] critères conformes sur l'échantillon
```

---

### Étape 5 — Audit auto layout et grilles

```
get_metadata [URL frames principales] → vérifier la présence d'auto layout
get_screenshot [URL frames] → vérifier visuellement les grilles
```

```markdown
## 5. Auto Layout et grilles

### Auto layout
| Constat | Nombre | Impact |
|---------|--------|--------|
| Composants avec auto layout | [N/N] | — |
| Composants avec positionnement fixe | [N] | 🔴 Critique — fragile |
| Frames principales avec auto layout | [N/N] | — |

### Grilles
| Constat | Valeur trouvée | Valeur attendue (rules/figma.md) | Conforme |
|---------|---------------|--------------------------------|---------|
| Colonnes iOS | [N] | 4 | ✅/❌ |
| Gouttière iOS | [N]pt | 16pt | ✅/❌ |
| Marge iOS | [N]pt | 16pt | ✅/❌ |
| Colonnes Android | [N] | 4 | ✅/❌ |
| Safe areas définies | ✅/❌ | Obligatoire | ✅/❌ |

### Score auto layout / grilles : [N]% conforme
```

---

### Étape 6 — Audit accessibilité

**Référence :** `rules/accessibility.md`

```
get_metadata [URL écrans principaux] → vérifier labels et annotations
get_screenshot [URL] → vérifier visuellement les contrastes
```

```markdown
## 6. Accessibilité

### Annotations RAAM dans Figma
| Critère RAAM | Présent | Qualité | Priorité |
|-------------|---------|---------|---------|
| Labels accessibles sur éléments interactifs | ✅/❌ | [Bonne/Partielle/Absente] | A — Critique |
| Ordre de focus numéroté | ✅/❌ | [Bonne/Partielle/Absente] | A — Critique |
| Zones tactiles min. 44pt/48dp | ✅/❌ | [Bonne/Partielle/Absente] | A — Critique |
| Overlay zones tactiles | ✅/❌ | [Bonne/Partielle/Absente] | A — Important |
| Ratios de contraste indiqués | ✅/❌ | [Bonne/Partielle/Absente] | AA — Important |
| Alt text images | ✅/❌ | [Bonne/Partielle/Absente] | A — Critique |

### Score accessibilité : [N/6] critères présents
```

---

### Étape 7 — Audit dark mode

```
get_variable_defs [URL] → vérifier présence de modes light/dark
```

```markdown
## 7. Dark mode

| Constat | Statut | Note |
|---------|--------|------|
| Variables de couleur avec mode Dark | ✅/❌ | [N] variables sans mode dark |
| Composants testés en dark mode | ✅/❌ | [N] composants non vérifiés |
| Images et illustrations adaptées | ✅/❌ | — |
| Styles de texte adaptés | ✅/❌ | — |

### Score dark mode : [N/4] critères conformes
```

---

### Étape 8 — Audit Code Connect

```
get_code_connect_map → mapping existant Figma → code
```

```markdown
## 8. Code Connect

| Constat | Nombre | Note |
|---------|--------|------|
| Composants avec mapping Code Connect | [N/N] | — |
| Composants sans mapping | [N] | Handoff dégradé |
| Frameworks connectés | [SwiftUI / Compose / Autre] | — |

### Score Code Connect : [N]% composants connectés
```

---

### Étape 9 — Rapport global et plan de correction

```markdown
# Rapport Audit Figma — [project-slug] — [YYYY-MM-DD]

## Score global

| Dimension | Score | Niveau |
|-----------|-------|--------|
| 1. Structure pages | [N/8] | 🔴/🟡/🟢 |
| 2. Nomenclature | [N]% | 🔴/🟡/🟢 |
| 3. Tokens / sémantique | [N]% | 🔴/🟡/🟢 |
| 4. Composants | [N/10] | 🔴/🟡/🟢 |
| 5. Auto layout / grilles | [N]% | 🔴/🟡/🟢 |
| 6. Accessibilité | [N/6] | 🔴/🟡/🟢 |
| 7. Dark mode | [N/4] | 🔴/🟡/🟢 |
| 8. Code Connect | [N]% | 🔴/🟡/🟢 |

**Score global : [N/100]**

| Score | Niveau de maturité |
|-------|-------------------|
| 80-100 | 🟢 Bon — ajustements mineurs |
| 50-79 | 🟡 Moyen — refactorisation partielle |
| 0-49 | 🔴 Faible — refonte nécessaire |

## Ce qu'on peut réutiliser tel quel
- [Élément réutilisable 1]
- [Élément réutilisable 2]

## Plan de correction

### 🔴 P1 — Bloquant (avant tout travail sur ce fichier)
| Action | Skill à utiliser | Effort estimé | Impact |
|--------|-----------------|--------------|--------|
| Migrer [N] valeurs hex en dur vers tokens sémantiques | `/setup-figma-tokens` | [J/H] | Critique |
| Renommer [N] frames selon convention | `/figma-use-wrapper` | [J/H] | Critique |
| Ajouter mode dark sur [N] variables | `/setup-figma-tokens` | [J/H] | Critique |

### 🟡 P2 — Important (sprint suivant)
| Action | Skill à utiliser | Effort estimé |
|--------|-----------------|--------------|
| Activer auto layout sur [N] composants | `/figma-use-wrapper` | [J/H] |
| Ajouter annotations RAAM sur écrans principaux | `/write-accessibility-annotations` | [J/H] |
| Publier [N] composants dans la bibliothèque | `/figma-use-wrapper` | [J/H] |

### 🟢 P3 — Nice-to-have (backlog)
| Action | Skill à utiliser | Effort estimé |
|--------|-----------------|--------------|
| Configurer Code Connect | `/figma-code-connect` | [J/H] |
| Créer page Documentation | `/setup-figma-project` | [J/H] |
```

## Phase de validation — niveau approfondi

Avant de valider ce livrable et de passer à la suite, réponds à ces questions dans l'ordre :

1. Les 8 dimensions ont-elles toutes été auditées — aucune sautée faute d'accès ? (oui / non + dimensions manquantes)
2. Le score global reflète-t-il honnêtement l'état du fichier — pas d'optimisme excessif ? (oui / non)
3. Les actions P1 bloquantes ont-elles chacune un skill de correction identifié et un effort estimé ? (oui / non + actions à compléter)
4. Le Product Designer a-t-il été consulté sur les éléments à conserver vs à refaire ? (oui / attendre confirmation)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ audit-figma-existing "$ARGUMENTS" terminé
📁 specs/[project-slug]/audit/figma-audit.md

Score global : [N/100] — Niveau : [Bon / Moyen / Faible]

⚠️ Validation Humaine requise
"Le plan de correction P1 est-il validé avant de démarrer les corrections ?"

Prochaines étapes selon le score :
→ Score > 80 : /write-cadrage (le fichier peut être utilisé avec ajustements mineurs)
→ Score 50-79 : Traiter les P1, puis /write-cadrage
→ Score < 50 : /setup-figma-project (restructuration complète recommandée)
→ Dans tous les cas : /audit-existing-project pour les specs et le code
```
