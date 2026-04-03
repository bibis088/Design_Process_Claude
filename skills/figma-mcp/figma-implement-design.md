---
name: figma-implement-design
description: "Traduit un design Figma en code SwiftUI ou Jetpack Compose avec fidélité 1:1 — get_design_context, mapping tokens, validation screenshot. Use when user says 'implémente le design', 'génère le code depuis figma', 'implémente le composant', 'code depuis figma', or 'traduis le design en code'."
argument-hint: "[figma-url] [platform:ios|android]"
agent: ui-designer
---

## Rôle
Traduire un design Figma en code natif mobile (SwiftUI ou Jetpack Compose) avec fidélité visuelle 1:1. S'appuie sur `get_design_context` pour extraire les specs exactes.

> Source : https://github.com/figma/mcp-server-guide/blob/main/skills/figma-implement-design/SKILL.md
> Adapté pour mobile iOS (SwiftUI) et Android (Jetpack Compose) — pas de web/React/Tailwind

---

## Limites de ce skill

| Tâche | Skill à utiliser |
|-------|-----------------|
| Générer code depuis Figma | **ce skill** |
| Créer/modifier des nœuds dans Figma | `figma-use` |
| Créer des écrans Figma depuis specs | `figma-generate-design` |
| Mapper composants Figma ↔ code | `figma-code-connect` |

---

## Prérequis

- [ ] URL Figma fournie (format : `https://figma.com/design/:fileKey/:fileName?node-id=1-2`)
- [ ] Plateforme cible précisée : iOS (SwiftUI) ou Android (Jetpack Compose)
- [ ] MCP Figma connecté
- [ ] Design system tokens disponibles dans `design-system/design-tokens.md`

---

## Étape 1 — Extraire le fileKey et nodeId depuis l'URL

**Format URL :** `https://figma.com/design/:fileKey/:fileName?node-id=1-2`

- **fileKey** : segment après `/design/`
- **nodeId** : valeur du paramètre `node-id` (format `1-2` ou `1:2`)

---

## Étape 2 — Récupérer le contexte de design

```
get_design_context(fileKey=":fileKey", nodeId="1-2")
```

Extrait :
- Layout (Auto Layout, constraints, sizing)
- Typographie (font, size, weight, line-height)
- Couleurs et tokens DS
- Structure des composants et variants
- Spacing et padding

**Si réponse trop grande :**
1. `get_metadata(fileKey=":fileKey", nodeId="1-2")` → structure globale
2. Identifier les nœuds enfants clés
3. `get_design_context` sur chaque enfant individuellement

---

## Étape 3 — Screenshot de référence

```
get_screenshot(fileKey=":fileKey", nodeId="1-2")
```

Garder accessible tout au long de l'implémentation — c'est la source de vérité visuelle.

---

## Étape 4 — Télécharger les assets

Assets (images, icônes, SVG) retournés par le MCP via URLs `localhost` :
- Utiliser directement — ne pas modifier les URLs
- Ne pas importer de bibliothèques d'icônes tierces
- Ne pas créer de placeholders si un asset `localhost` est fourni

---

## Étape 5 — Traduire vers SwiftUI ou Jetpack Compose

### Principes fondamentaux

- Traiter la sortie Figma MCP comme une **représentation du design** — pas comme du code final
- Réutiliser les composants existants du projet (jamais dupliquer)
- Utiliser les tokens DS de `design-tokens.md` — jamais de valeur hex ou px en dur
- Respecter les conventions du projet existant

### Mapping Figma → iOS (SwiftUI)

| Figma | SwiftUI |
|-------|---------|
| Frame + Auto Layout Horizontal | `HStack` |
| Frame + Auto Layout Vertical | `VStack` |
| Frame sans Auto Layout | `ZStack` |
| Text | `Text` avec `.font(.custom(...))` ou style DS |
| Couleur token | `Color("token-name")` ou asset catalog |
| Spacing token | `Spacer()` ou `.padding(.spacing)` depuis DS |
| Radius token | `.cornerRadius(DS.radius.md)` |
| Shadow | `.shadow(color:, radius:, x:, y:)` |
| Component instance | Vue Swift correspondante |
| Boolean property | `@State var showIcon: Bool` |
| Text property | `var label: String` |

**Zones tactiles iOS :** minimum 44×44pt — utiliser `.frame(minWidth: 44, minHeight: 44)` si l'élément visuel est plus petit.

### Mapping Figma → Android (Jetpack Compose)

| Figma | Compose |
|-------|---------|
| Frame + Auto Layout Horizontal | `Row` |
| Frame + Auto Layout Vertical | `Column` |
| Frame sans Auto Layout | `Box` |
| Text | `Text` avec `style = MaterialTheme.typography.X` |
| Couleur token | `MaterialTheme.colorScheme.X` |
| Spacing token | `Modifier.padding(Dp)` depuis DS |
| Radius token | `RoundedCornerShape(Dp)` |
| Shadow | `Modifier.shadow(elevation = Dp)` |
| Component instance | Composable correspondante |
| Boolean property | `val showIcon: Boolean = false` |
| Text property | `val label: String` |

**Zones tactiles Android :** minimum 48×48dp — utiliser `Modifier.defaultMinSize(minWidth = 48.dp, minHeight = 48.dp)`.

### Ce qu'on ne hardcode jamais
- Couleurs hex → token DS
- Tailles en px → pt (iOS) ou dp (Android) depuis les tokens
- Fonts → styles typographiques DS
- Ombres → tokens effect

---

## Étape 6 — Validation finale contre Figma

**Checklist visuelle :**
- [ ] Layout conforme (spacing, alignement, sizing)
- [ ] Typographie conforme (font, size, weight, line-height)
- [ ] Couleurs exactes (tokens appliqués correctement)
- [ ] États interactifs fonctionnels (pressed, disabled)
- [ ] Zones tactiles ≥ 44pt iOS / 48dp Android
- [ ] Dark mode supporté via tokens sémantiques
- [ ] Accessibilité (accessibilityLabel, contentDescription)
- [ ] Dynamic Type (iOS) / Large Text (Android) supporté

---

## Règles de qualité du code

- Composants dans le répertoire DS du projet
- Nommage selon les conventions du projet existant
- Aucun style inline sauf valeurs dynamiques
- Types Swift/Kotlin pour les props
- Commentaires JSDoc/KDoc sur les composants exportés
- Déviations documentées avec justification en commentaire

---

## Gestion des erreurs

| Problème | Solution |
|---------|---------|
| Réponse Figma tronquée | `get_metadata` puis `get_design_context` par enfant |
| Design ne correspond pas | Comparer side-by-side avec screenshot étape 3 |
| Token DS ≠ valeur Figma | Préférer le token DS, ajuster spacing minimal si nécessaire |

## Résumé de fin d'exécution

```
✅ figma-implement-design "$ARGUMENTS" terminé
📱 Plateforme : [iOS (SwiftUI) / Android (Jetpack Compose)]
📁 [chemin/composant.swift ou .kt]

Tokens utilisés : [N] (0 valeur en dur)
Zones tactiles : ✅ conformes
Dark mode : ✅ supporté via tokens

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]

Prochaines étapes :
→ Validation visuelle Product Designer
→ /figma-code-connect $ARGUMENTS (mapper composant ↔ code)
```
