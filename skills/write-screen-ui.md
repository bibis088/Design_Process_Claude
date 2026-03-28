---
name: write-screen-ui
description: "Implémente un écran complet en code natif SwiftUI + Compose depuis une spec UX validée. Exécuté par le ui-designer."
argument-hint: "[feature-slug]/[screen-id]-[screen-slug]"
disable-model-invocation: false
context: fork
agent: ui-designer
---

## Rôle
Implémenter un écran complet en code natif iOS (SwiftUI) et Android (Compose) depuis une spec UX validée.

## Agents consommateurs
- UI Designer (pilote)

## Distinction avec write-component-ui
| write-screen-ui | write-component-ui |
|-----------------|-------------------|
| Écran complet avec UiState | Composant isolé réutilisable |
| Orchestre plusieurs composants | Composant atomique |
| Gère la navigation | Ne gère pas la navigation |

## Prérequis
- [ ] Spec d'écran UX disponible dans `./design/[NomFeature]/ux/`
- [ ] Navigation map (`SCREENS-MAP.md`) à jour
- [ ] Tous les composants utilisés disponibles dans `./design-system/components/`
- [ ] Tokens nécessaires vérifiés dans `./design-system/tokens/`

## Processus de génération

### Étape 1 — Définir le UiState
Modéliser tous les états de l'écran avant d'écrire le moindre composant visuel.

#### SwiftUI
```swift
// MARK: - UiState
// Écran : [S-XX-nom]
// Spec UX : design/[feature]/ux/S-XX-[nom].md
// US couvertes : [US-###, US-###]

enum [NomEcran]State {
    case loading
    case empty
    case error(message: String, retryAction: () -> Void)
    case success([NomEcran]Content)
}

struct [NomEcran]Content {
    // Données de l'état success
}
```

#### Compose
```kotlin
// Écran : [S-XX-nom]
// Spec UX : design/[feature]/ux/S-XX-[nom].md
// US couvertes : [US-###, US-###]

sealed class [NomEcran]UiState {
    object Loading : [NomEcran]UiState()
    object Empty : [NomEcran]UiState()
    data class Error(val message: String) : [NomEcran]UiState()
    data class Success(val content: [NomEcran]Content) : [NomEcran]UiState()
}

data class [NomEcran]Content(
    // Données de l'état success
)
```

### Étape 2 — Implémenter l'écran

#### Structure SwiftUI obligatoire
```swift
struct [NomEcran]View: View {
    @StateObject var viewModel: [NomEcran]ViewModel

    var body: some View {
        switch viewModel.state {
        case .loading:
            [NomEcran]LoadingView()
        case .empty:
            [NomEcran]EmptyView(onAction: viewModel.handleEmptyCTA)
        case .error(let message, let retry):
            [NomEcran]ErrorView(message: message, onRetry: retry)
        case .success(let content):
            [NomEcran]ContentView(content: content)
        }
    }
}

// Sous-vues privées par état
private struct [NomEcran]LoadingView: View { ... }
private struct [NomEcran]EmptyView: View { ... }
private struct [NomEcran]ErrorView: View { ... }
private struct [NomEcran]ContentView: View { ... }

// Previews
#Preview("[NomEcran] — loading") { ... }
#Preview("[NomEcran] — empty") { ... }
#Preview("[NomEcran] — error") { ... }
#Preview("[NomEcran] — success") { ... }
#Preview("[NomEcran] — dark") { ... }
```

#### Structure Compose obligatoire
```kotlin
@Composable
fun [NomEcran]Screen(
    uiState: [NomEcran]UiState,
    onAction: ([NomEcran]Action) -> Unit,
    modifier: Modifier = Modifier
) {
    when (uiState) {
        is [NomEcran]UiState.Loading -> [NomEcran]Loading(modifier)
        is [NomEcran]UiState.Empty -> [NomEcran]Empty(onAction, modifier)
        is [NomEcran]UiState.Error -> [NomEcran]Error(uiState.message, onAction, modifier)
        is [NomEcran]UiState.Success -> [NomEcran]Content(uiState.content, onAction, modifier)
    }
}

@Preview(name = "Loading", showBackground = true)
@Preview(name = "Loading dark", uiMode = UI_MODE_NIGHT_YES)
@Composable private fun [NomEcran]LoadingPreview() { ... }
// Répéter pour Empty, Error, Success
```

## DoD — Écran UI

### Critères design
- [ ] Les 4 états implémentés : loading, empty, error, success
- [ ] Hiérarchie visuelle conforme à la spec UX
- [ ] Dark mode vérifié sur l'ensemble de l'écran
- [ ] Comportement au scroll conforme à la spec UX
- [ ] Transitions conformes aux specs UX et guidelines motion

### Critères code
- [ ] `UiState` couvrant tous les états
- [ ] Preview pour chaque état en light ET dark
- [ ] Tous les composants référencent des tokens sémantiques
- [ ] Accessibilité complète : ordre de focus, labels, zones tactiles
- [ ] `US-###` et `FLUX-###` référencés en en-tête du fichier
- [ ] Fichier déposé dans `./design/[NomFeature]/ui/`


## Gestion des erreurs

Si les prérequis ne sont pas remplis :
> ❌ Prérequis manquant — vérifier les fichiers sources avant de relancer.

Si un fichier source est introuvable via chemin relatif, utiliser Glob avec le pattern `**/design/$ARGUMENTS/**` pour localiser les fichiers depuis leur chemin absolu.

## Chemin de fichier
```
design/[feature-slug]/ui/[NomEcran]View.swift    ← iOS
design/[feature-slug]/ui/[NomEcran]Screen.kt      ← Android
```


## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. Le modèle UiState couvre-t-il tous les états de l'écran — loading, empty, error, success ? (oui / non)
2. Les previews couvrent-ils tous les états en light ET dark mode sur iOS et Android ? (oui / non)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-screen-ui "$ARGUMENTS" terminé
📁 design/$ARGUMENTS/ui/[NomEcran].swift + .kt

Prochaines étapes :
→ /review-dod S-XX
→ /write-changelog $ARGUMENTS (si nouveau composant DS)
```
