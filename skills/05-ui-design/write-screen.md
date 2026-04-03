---
name: write-screen
description: "Spec et implémentation UI d'un écran — décrit le contenu et comportements puis implémente dans Figma. Use when user says 'spec écran', 'écran figma', 'screen spec', 'screen ui', 'implémente l écran', or 'write-screen'."
argument-hint: "[feature-slug]/[screen-id]"
agent: ui-designer
---

## Rôle
Documenter puis implémenter chaque écran dans Figma — spec textuelle puis frame UI.
Couvre les écrans principaux (happy path) **et** les états dégradés (loading · empty · error) à partir des composants DS et du UI principal existant.

> Ce skill utilise `figma-generate-design` pour construire les frames section par section — jamais en un seul appel `use_figma`.


## Prérequis
- [ ] `US-###` source de l'écran disponible et Approved
- [ ] Gate 6 validée — direction artistique définie (Product Designer)
- [ ] Frames créées via `setup-figma-frames`
- [ ] Composants DS disponibles (`write-component`)
- [ ] `design/[feature-slug]/ux/screens/S-XX-[nom].md` disponible
- [ ] `FLUX-###` source identifié — contexte du flux pour l'écran
- [ ] `specs/[epic-slug]/figma-urls.md` disponible — URL page `Work In Progress UI`

## Étape 1 — Spec textuelle de l'écran
### Étape 1 — Identifier l'écran
1. Attribuer l'ID selon la convention : `S-01`, `S-02`... (remis à zéro par feature)
2. Nommer l'écran de façon descriptive : `S-01-login`, `S-02-dashboard`
3. Identifier le type : Root / Child / Modal / Overlay
4. Lister les user stories que cet écran couvre

### Étape 2 — Documenter les 4 états obligatoires
Chaque écran doit avoir ses 4 états documentés avant d'être considéré complet :
- **Loading** : que voit l'utilisateur pendant le chargement ?
- **Empty** : que voit l'utilisateur si aucune donnée n'est disponible ?
- **Error** : que voit l'utilisateur en cas d'erreur ?
- **Success** : l'état nominal avec données

### Étape 3 — Rédiger la spec
Suivre strictement ce format :

```markdown
# [S-XX] Nom de l'écran — [Plateforme]

## Métadonnées
- Feature : [FEAT-###]
- User Stories couvertes : [US-###, US-###]
- Flux UX source : [FLUX-###]
- Plateforme : [iOS / Android / iOS + Android]
- Statut : [To Do / In Progress / In Review / Approved / Dev Ready]

## Références Figma
- Frame : [URL directe — ex: https://figma.com/file/xxx?node-id=yyy]
- Page Figma : [Work In Progress UI / Validated UI / Ready for dev]
- Annotations accessibilité : [To Do / In Progress / Done]

## Informations générales
- Route / deeplink : `app://[chemin]`
- Barre de navigation : [Visible / Cachée / Transparente]
- Status bar : [Light / Dark / Automatique]
- Scroll : [Oui — vertical / Non / Horizontal]
- Point d'entrée depuis : [Écran(s) ou trigger(s)]
- Points de sortie vers : [Écran(s) ou action(s)]

## État — Loading
[Description de ce que voit l'utilisateur pendant le chargement]
- Composant : [Skeleton / Spinner / Shimmer]
- Durée max avant timeout : [ex: 10s]
- Comportement si timeout : [ex: Affiche ERR-01]

## État — Empty
[Description de l'état vide]
- Illustration : [Description de l'illustration ou icône]
- Message principal : [Texte affiché]
- Message secondaire : [Texte explicatif optionnel]
- CTA : [Libellé du bouton d'action → destination]

## État — Error
[Description de l'état d'erreur]
- Type d'erreur : [Réseau / Serveur / Validation / Permission]
- Message affiché : [Texte d'erreur — doit expliquer quoi faire, pas juste ce qui s'est passé]
- Action de récupération : [Retry / Retour / Contacter le support]

## État — Success (nominal)

### Hiérarchie visuelle (de haut en bas)

1. **[Composant 1 — ex: Header]**
   - Contenu : [Description précise]
   - Comportement au scroll : [Sticky / Disparaît / Collapse]
   - Actions possibles : [tap → destination / comportement]

2. **[Composant 2 — ex: Liste]**
   - Contenu : [Description précise]
   - Nombre d'items : [Fixe / Variable — max recommandé]
   - Comportement au scroll : [Normal / Infini / Pagination]
   - Actions possibles :
     - tap sur item → [destination]
     - swipe gauche → [action destructive]

3. **[Composant 3 — ex: CTA fixe]**
   - Contenu : [Libellé]
   - Position : [Fixe en bas / Flottant]
   - Actions possibles : [tap → destination]

### Typographie (rôles sémantiques)
| Élément | Rôle sémantique | Hiérarchie | Usage |
|---------|----------------|------------|-------|
| [Titre] | Heading 1 | Priorité haute | [Contexte] |
| [Sous-titre] | Body | Priorité moyenne | [Contexte] |

## Transitions
| Vers | Type | Déclencheur |
|------|------|------------|
| [S-XX-nom] | push / modal / fade | [Action déclenchante] |
| [S-XX-nom] | pop / dismiss | [Action déclenchante] |

## Accessibilité
- [ ] Labels VoiceOver (iOS) / Content Description (Android) sur tous les éléments interactifs
- [ ] Ordre de focus logique documenté (de haut en bas, gauche à droite)
- [ ] Zones tactiles minimum : 44x44pt (iOS) / 48x48dp (Android)
- [ ] Aucune information transmise uniquement par la couleur
- [ ] Comportement en texte agrandi (Dynamic Type / Large Font) décrit
- [ ] Gestes alternatifs disponibles pour tout swipe ou geste complexe

### Détail des labels d'accessibilité
| Élément | Label VoiceOver / TalkBack | Hint (si nécessaire) |
|---------|---------------------------|---------------------|
| [Élément interactif] | [Label précis] | [Indication d'action si non évidente] |

## Comportements spécifiques plateforme
| Comportement | iOS | Android |
|-------------|-----|---------|
| [Navigation retour] | Swipe back natif | Bouton back système |
| [Autre divergence] | [Comportement iOS] | [Comportement Android] |

## User Stories couvertes
| US | Critères d'acceptance couverts |
|----|-------------------------------|
| [US-###] | [CA-01, CA-02, CA-03] |
```


## Gestion des erreurs
> ❌ Prérequis manquant — vérifier les fichiers sources avant de relancer.

## Règles de qualité

- Les 4 états sont **obligatoires** — un écran sans état Empty ou Error n'est pas complet
- Les messages d'erreur doivent expliquer **quoi faire**, pas juste ce qui s'est passé
- Chaque élément interactif a un label d'accessibilité — aucune exception
- Les divergences iOS / Android sont documentées explicitement
- Un écran ne peut pas être en statut `Approved` sans que les 4 états soient documentés

## Chemin de fichier
```
design/[feature-slug]/screens/S-XX-[nom].md
```


## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. Les 4 états sont-ils tous documentés — loading, empty, error, success ? (oui / manque : ...)
2. Chaque élément interactif a-t-il un label d'accessibilité défini ? (oui / non + éléments manquants)
3. Les comportements divergents iOS / Android sont-ils tous notés explicitement ? (oui / non)
4. L'URL du frame Figma est-elle renseignée dans la section "Références Figma" ? (oui / non — ajouter l'URL avant de continuer)
5. Le frame Figma contient-il en description la référence `S-XX` / `US-###` / `FLUX-###` ? (oui / non — à compléter dans Figma)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-screen-spec "$ARGUMENTS" terminé
📁 design/$ARGUMENTS/screens/S-XX-[nom].md

Prochaines étapes :
→ /review-dod S-XX
→ /write-component-spec $ARGUMENTS (si nouveau composant)

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```

---

## Étape 2 — Implémentation UI dans Figma
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
> ❌ Prérequis manquant — vérifier les fichiers sources avant de relancer.

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

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```

---

## Résumé de fin d'exécution

```
✅ write-screen "$ARGUMENTS" terminé
📁 design/$ARGUMENTS/ux/screens/S-XX-[nom].md
🎨 Figma → frame US_###_[nom]_[os]_[état]

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]

Prochaines étapes :
→ /write-screen $ARGUMENTS/[écran-suivant]
→ Direction artistique (Gate 6) quand tous les écrans principaux sont prêts
```
