---
name: write-ds-documentation
description: Génère la documentation web du design system pour un composant — contexte d'utilisation, cas principaux, cas dégradés, utilisations interdites, visualisation des tokens appliqués (spacing, couleurs, typo). Section code réservée pour la phase de développement. Exécuté par le design-system-manager.
argument-hint: [component-slug]
disable-model-invocation: false
context: fork
agent: design-system-manager
---

## Rôle
Produire la page de documentation web d'un composant du design system — exploitable directement comme page HTML standalone ou intégrable dans un site de documentation (Storybook, Zeroheight, Notion, etc.).

## Agents consommateurs
- Design System Manager (pilote — produit la documentation)
- UI Designer (consommateur — référence lors de l'implémentation)
- Dev iOS / Android (consommateur — section code alimentée en phase Dev)

## Prérequis
- [ ] Composant Figma créé et publié dans la bibliothèque DS
- [ ] Spec composant disponible (`write-component-ds`)
- [ ] Tokens appliqués sur le composant (`get_variable_defs`)
- [ ] MCP Figma connecté pour les screenshots

## Processus de génération

### Étape 1 — Lire les sources

```
get_variable_defs [URL composant Figma]
→ Extraire : tokens couleurs, spacing, typo, radius appliqués

get_screenshot [URL composant Figma]
→ Captures de chaque variant et état
```

Lire aussi `design-system/components/[component-slug].md` pour les specs textuelles.

### Étape 2 — Générer la page de documentation

```markdown
# [NomComposant]

> [Une phrase — ce que fait ce composant]

---

## Contexte d'utilisation

[Quand utiliser ce composant — situation, objectif utilisateur, fréquence]

**Utiliser quand :**
- [Cas d'usage principal]
- [Cas d'usage secondaire]

**Ne pas utiliser quand :** *(voir section Utilisations interdites)*

---

## Variantes

### [Variante 1 — ex: Primary]
[Screenshot du composant]
[Description courte de quand utiliser cette variante]

### [Variante 2 — ex: Secondary]
[Screenshot]
[Description]

### [Variante 3 — ex: Destructive]
[Screenshot]
[Description — attention aux cas d'usage]

---

## États

| État | Description | Déclencheur |
|------|-------------|------------|
| Default | État au repos | — |
| Hover / Pressed | Feedback au toucher | Tap de l'utilisateur |
| Disabled | Non interactif | Condition métier non remplie |
| Loading | Action en cours | Après tap, en attente serveur |
| Error | État d'erreur | Erreur de validation ou réseau |

---

## Cas principaux

### Cas 1 — [Nom du cas]
[Screenshot]
**Contexte :** [Situation typique]
**Comportement :** [Ce qui se passe]
**Règle :** [Contrainte à respecter]

### Cas 2 — [Nom du cas]
[Même structure]

---

## Cas dégradés

### État vide / sans données
[Screenshot état empty si applicable]
[Description de ce qui s'affiche quand il n'y a pas de contenu]

### Texte long / troncature
[Screenshot avec texte long]
[Comment le composant gère un contenu plus long que prévu]

### Écran réduit / accessibilité texte agrandi
[Screenshot à 200% zoom]
[Comportement avec Dynamic Type (iOS) / Large Text (Android)]

### Dark mode
[Screenshot dark mode]
[Tokens appliqués en dark mode]

---

## Utilisations interdites

| ❌ À ne pas faire | ✅ À faire à la place |
|-------------------|----------------------|
| [Utilisation incorrecte 1] | [Alternative correcte] |
| [Utilisation incorrecte 2] | [Alternative correcte] |

[Screenshots des cas interdits si possible]

---

## Tokens appliqués

### Couleurs
| Propriété | Token | Valeur Light | Valeur Dark |
|-----------|-------|-------------|------------|
| Fond | `color/interactive/primary` | `#007AFF` | `#0A84FF` |
| Texte | `color/content/onPrimary` | `#FFFFFF` | `#FFFFFF` |
| Bordure | — | — | — |

### Typographie
| Propriété | Token | iOS | Android |
|-----------|-------|-----|---------|
| Label | `iOS/type/callout` / `Android/type/labelLarge` | 16pt SemiBold | 14sp Medium |

### Spacing
| Propriété | Token | Valeur |
|-----------|-------|--------|
| Padding horizontal | `spacing/md` | 16pt/dp |
| Padding vertical | `spacing/sm` | 8pt/dp |
| Gap icône-label | `spacing/xs` | 4pt/dp |

### Radius
| Propriété | Token | Valeur |
|-----------|-------|--------|
| Border radius | `radius/lg` | 12pt/dp |

### Zones tactiles
| Plateforme | Taille min. | Implémentation |
|-----------|------------|----------------|
| iOS | 44×44pt | Padding invisible si visuel plus petit |
| Android | 48×48dp | TouchTarget ou padding |

---

## Accessibilité

| Critère RAAM | Implémentation |
|-------------|----------------|
| Label accessible | `accessibilityLabel` = "[Libellé du bouton]" |
| Rôle | `Button` (iOS) / `role="button"` (Android) |
| État disabled | `accessibilityTraits: .notEnabled` / `enabled=false` |
| Contraste | ≥ 3:1 fond/texte (vérifié — ratio : [valeur]) |

---

## Code

> ⏳ Cette section sera alimentée lors de la phase de développement.

### iOS (SwiftUI)
```swift
// À venir
```

### Android (Jetpack Compose)
```kotlin
// À venir
```

---

## Changelog

| Version | Date | Changement |
|---------|------|-----------|
| 1.0.0 | [YYYY-MM-DD] | Création initiale |
```

### Étape 3 — Générer le fichier HTML standalone

Si une page web standalone est requise, générer un fichier HTML avec :
- Tailwind en CDN pour le style
- Sections repliables pour les cas dégradés et tokens
- Navigation latérale si plusieurs composants sur la même page
- Section Code avec onglets iOS / Android (contenu vide, à compléter)

```
design-system/documentation/[component-slug].html
```

## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. Les tokens affichés correspondent-ils exactement aux valeurs extraites via `get_variable_defs` — aucune valeur inventée ? (oui / non + tokens à corriger)
2. Les cas dégradés couvrent-ils texte long, dark mode et texte agrandi ? (oui / non + cas manquants)
3. La section "Utilisations interdites" contient-elle au moins 2 exemples concrets ? (oui / non)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-ds-documentation "$ARGUMENTS" terminé
📁 design-system/documentation/[component-slug].md
📁 design-system/documentation/[component-slug].html (si page web)

Prochaines étapes :
→ /write-ds-documentation [composant-suivant]
→ Section Code à compléter lors de la phase Dev
→ /write-client-deliverable functional-doc [epic-slug]
```
