---
name: write-token
description: "Crée et documente un token de design sémantique avec implémentation iOS et Android. Exécuté par le design-system-manager."
argument-hint: "[type]/[token-slug]"
agent: design-system-manager
---

## Rôle
Créer et documenter un token de design — sémantique, light/dark mode, avec implémentation iOS (SwiftUI) et Android (Compose).

## Agents consommateurs
Design System Manager  · UI Designer 

## Prérequis
Avant d'utiliser ce skill, vérifier que les éléments suivants sont disponibles :
- [ ] La demande de token a été soumise par l'agent consommateur
- [ ] La palette de primitives existante a été consultée (`./design-system/tokens/colors.md`)
- [ ] Le Design System Manager a validé que le token n'existe pas déjà
- [ ] L'usage du token est clairement défini (pas de token "générique")

## Processus de génération

### Étape 1 — Vérifier si le token existe déjà
Consulter `./design-system/tokens/` avant toute création. Si un token existant couvre le besoin :
- Documenter pourquoi le token existant est suffisant
- Informer l'agent demandeur avec la référence correcte

### Étape 2 — Nommer le token

**Convention de nommage obligatoire :**
```
[catégorie].[sous-catégorie].[usage]
```

| Catégorie | Sous-catégories | Exemples |
|-----------|----------------|---------|
| `color` | `content`, `background`, `interactive`, `border`, `feedback` | `color.content.primary` |
| `spacing` | — | `spacing.md`, `spacing.lg` |
| `radius` | — | `radius.sm`, `radius.md` |
| `shadow` | — | `shadow.card`, `shadow.modal` |
| `motion` | `duration`, `easing` | `motion.duration.fast` |
| `type` | — | `type.title1`, `type.body` |

**Règles de nommage :**
- Toujours sémantique — jamais `color.blue` ou `color.500`
- Toujours en anglais, en kebab-case pour les valeurs
- Le nom doit décrire l'usage, pas la valeur

### Étape 3 — Documenter le token

Suivre ce format selon le type de token :

#### Token de couleur

```markdown
## Token — [color.catégorie.usage]

### Définition
| Propriété | Valeur |
|-----------|--------|
| Nom | `color.[catégorie].[usage]` |
| Type | Couleur sémantique |
| Usage | [Description précise du contexte d'utilisation] |
| Primitive light | `palette.[ramp].[stop]` |
| Primitive dark | `palette.[ramp].[stop]` |

### Valeurs
| Mode | Primitive | Valeur hex |
|------|-----------|-----------|
| Light | `palette.[ramp].[stop]` | #[valeur] |
| Dark | `palette.[ramp].[stop]` | #[valeur] |

### Exemples d'usage
- ✅ [Contexte d'usage correct]
- ✅ [Autre contexte correct]
- ❌ [Contexte incorrect — utiliser X à la place]

### Implémentation iOS (SwiftUI)
```swift
extension Color {
    static let [camelCaseName] = Color("[nom.du.token]")
}
// Usage : .foregroundColor(.contentPrimary)
```

### Implémentation Android (Compose)
```kotlin
val [CamelCaseName] = MaterialTheme.colorScheme.[mappingMaterial]
// Usage : color = MaterialTheme.colorScheme.[mappingMaterial]
```
```

#### Token d'espacement

```markdown
## Token — [spacing.taille]

### Définition
| Propriété | Valeur |
|-----------|--------|
| Nom | `spacing.[taille]` |
| Type | Espacement |
| Valeur | [N]pt (iOS) / [N]dp (Android) |
| Usage | [Description du contexte d'utilisation] |

### Exemples d'usage
- ✅ [Contexte correct]
- ❌ [Contexte incorrect]

### Implémentation iOS (SwiftUI)
```swift
extension CGFloat {
    static let spacing[Taille]: CGFloat = [valeur]
}
```

### Implémentation Android (Compose)
```kotlin
val Spacing[Taille] = [valeur].dp
```
```

#### Token de typographie

```markdown
## Token — [type.nom]

### Définition
| Propriété | iOS | Android |
|-----------|-----|---------|
| Nom | `type.[nom]` | `type.[nom]` |
| Taille | [N]pt | [N]sp |
| Poids | [Bold / SemiBold / Regular] | [Bold / Medium / Regular] |
| Line height | [N]pt | [N]sp |
| Usage | [Contexte] | [Contexte] |

### Implémentation iOS (SwiftUI)
```swift
extension Font {
    static let [camelCaseName] = Font.system(size: [N], weight: .[weight])
}
```

### Implémentation Android (Compose)
```kotlin
val [CamelCaseName] = TextStyle(
    fontSize = [N].sp,
    fontWeight = FontWeight.[Weight],
    lineHeight = [N].sp
)
```
```

### Étape 4 — Mettre à jour le changelog

Après validation du Design System Manager, ajouter une entrée dans le changelog :

```markdown
## [x.x.x] — [YYYY-MM-DD]
### Ajouté
- Token `[nom.du.token]` — [usage en une phrase]
```


## Gestion des erreurs
> ❌ Prérequis manquant — vérifier les fichiers sources avant de relancer.

## Règles de qualité

- Jamais de valeur primitive en dur dans le code — toujours le token sémantique
- Chaque token a **obligatoirement** une valeur light ET dark mode
- Un token sans usage documenté ne doit pas être créé
- Si deux tokens ont le même usage, les fusionner en un seul
- La demande de token doit venir d'un besoin réel — pas par anticipation

## Chemin de fichier
```
design-system/tokens/[type].md
```


## Phase de validation — niveau approfondi

Avant de valider ce livrable et de passer à la suite, réponds à ces questions dans l'ordre :

1. Le token a-t-il un nom sémantique — décrit-il l'usage et non la valeur ? (oui / renommer en ...)
2. Les valeurs light ET dark mode sont-elles toutes les deux définies ? (oui / non + mode manquant)
3. Le token référence-t-il une primitive existante de la palette — jamais une valeur hex en dur ? (oui / non)
4. L'implémentation iOS ET Android est-elle fournie et conforme aux conventions du design system ? (oui / non)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-token "$ARGUMENTS" terminé
📁 design-system/tokens/[type].md

Prochaines étapes :
→ /write-changelog $ARGUMENTS
→ /write-component-ds $ARGUMENTS (si token pour nouveau composant)
```
