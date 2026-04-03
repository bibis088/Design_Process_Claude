---
name: write-component-ui
description: "Implémente un composant UI natif SwiftUI + Compose depuis une spec UX validée. Exécuté par le ui-designer."
argument-hint: "[feature-slug]/[component-slug]"
agent: ui-designer
---

## Rôle
Implémenter un composant UI natif iOS (SwiftUI) et Android (Compose) depuis une spec UX validée, en respectant les conventions du design system.

## Agents consommateurs
UI Designer 

## Prérequis
Avant d'utiliser ce skill, vérifier que les éléments suivants sont disponibles :
- [ ] Spec composant UX disponible dans `./design/[NomFeature]/ux/`
- [ ] Tokens nécessaires vérifiés dans `./design-system/tokens/`
- [ ] Composant vérifié dans `./design-system/components/` — ne pas recréer l'existant
- [ ] Si token ou composant manquant → demande soumise au Design System Manager avant de continuer
- [ ] `US-###` sources identifiées — composants requis listés

## Processus de génération

### Étape 1 — Lire la spec UX
Depuis `./design/[NomFeature]/ux/`, extraire :
- Les états à implémenter (default, pressed, disabled, loading, error)
- Les variantes requises (primary, secondary, destructive)
- Les interactions et leurs comportements
- Les annotations d'accessibilité (labels, zones tactiles, ordre de focus)
- L'intention de style (rôle visuel, hiérarchie, contexte)

### Étape 2 — Identifier les tokens à utiliser
Depuis `./design-system/tokens/` :
- Couleurs → tokens `color.[catégorie].[usage]` uniquement
- Typographie → tokens `type.[nom]` uniquement
- Espacements → tokens `spacing.[taille]` uniquement
- Radius → tokens `radius.[taille]` uniquement
- Jamais de valeur primitive ou hardcodée

### Étape 3 — Implémenter le composant

#### Structure SwiftUI obligatoire

```swift
// MARK: - [NomComposant]
// US associée : [US-###]
// Spec UX : design/[feature]/ux/[fichier].md

struct [NomComposant]: View {

    // MARK: - Paramètres
    /// [Description du paramètre]
    let [parametre]: [Type]
    /// [Description du paramètre optionnel]
    var [parametreOptionnel]: [Type] = [valeurParDefaut]

    // MARK: - Body
    var body: some View {
        // Implémentation
    }

    // MARK: - Sous-composants privés (si nécessaire)
}

// MARK: - Preview
#Preview("[NomComposant] — tous états") {
    VStack(spacing: 16) {
        [NomComposant](/* état default */)
        [NomComposant](/* état loading */)
        [NomComposant](/* état disabled */)
        [NomComposant](/* état error */)
    }
    .padding()
}

#Preview("[NomComposant] — dark mode") {
    [NomComposant](/* état default */)
        .preferredColorScheme(.dark)
}
```

#### Structure Compose obligatoire

```kotlin
/**
 * [NomComposant]
 * US associée : [US-###]
 * Spec UX : design/[feature]/ux/[fichier].md
 *
 * @param [parametre] [Description]
 * @param [parametreOptionnel] [Description] — default: [valeur]
 */
@Composable
fun [NomComposant](
    [parametre]: [Type],
    [parametreOptionnel]: [Type] = [valeurParDefaut],
    modifier: Modifier = Modifier
) {
    // Implémentation
}

// Previews obligatoires
@Preview(name = "[NomComposant] — light", showBackground = true)
@Preview(name = "[NomComposant] — dark", uiMode = UI_MODE_NIGHT_YES, showBackground = true)
@Composable
private fun [NomComposant]Preview() {
    AppTheme {
        Column(modifier = Modifier.padding(16.dp)) {
            [NomComposant](/* état default */)
            Spacer(modifier = Modifier.height(8.dp))
            [NomComposant](/* état loading */)
            Spacer(modifier = Modifier.height(8.dp))
            [NomComposant](/* état disabled */)
        }
    }
}
```

### Étape 4 — Checklist accessibilité

Avant de finaliser, vérifier :
- [ ] `accessibilityLabel` défini sur tous les éléments interactifs (SwiftUI)
- [ ] `accessibilityHint` ajouté si l'action n'est pas évidente depuis le label
- [ ] `accessibilityValue` mis à jour dynamiquement si l'état change (ex: toggle)
- [ ] `.disabled(true)` avec mise à jour du label en conséquence
- [ ] `semantics { contentDescription = "..." }` sur tous les éléments interactifs (Compose)
- [ ] Zones tactiles min. respectées : 44x44pt (iOS) / 48x48dp (Android)

## DoD — Composant UI

### Critères design
- [ ] Tous les états implémentés : default, pressed, disabled, loading, error
- [ ] Toutes les variantes couvertes : primary, secondary, destructive (si applicable)
- [ ] Dark mode supporté via tokens sémantiques uniquement
- [ ] Aucune valeur primitive en dur dans le code
- [ ] Cohérence vérifiée avec `./design-system/components/`

### Critères code
- [ ] Paramètres documentés avec commentaires inline
- [ ] Preview iOS ET Android couvrant tous les états (light + dark)
- [ ] Accessibilité complète (labels, hints, zones tactiles)
- [ ] Sous-composants extraits si réutilisables
- [ ] Soumis au Design System Manager si nouveau token ou composant introduit


## Gestion des erreurs
> ❌ Prérequis manquant — vérifier les fichiers sources avant de relancer.

## Chemin de fichier
```
design/[feature-slug]/ui/[nom-composant].swift   ← iOS
design/[feature-slug]/ui/[NomComposant].kt        ← Android
```


## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. Aucune valeur primitive n'est-elle utilisée en dur dans le code — uniquement des tokens sémantiques ? (oui / non + occurrences à corriger)
2. Les previews couvrent-ils tous les états en light ET dark mode ? (oui / non)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-component-ui "$ARGUMENTS" terminé
📁 design/$ARGUMENTS/ui/[NomComposant].swift + .kt

Prochaines étapes :
→ /review-dod [NomComposant]
→ /write-component-ds $ARGUMENTS (si ajout au design system)

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```
