---
name: write-component
description: "Cycle complet d'un composant Figma — création avec variants et tokens, vérification conformité HIG/Material 3, documentation DS, puis specs Documentation UX/UI (Anatomy + API + Color + Structure + Screen Reader) directement dans Figma. Use when user says 'crée un composant', 'nouveau composant', 'component', 'write-component', or 'composant figma'."
argument-hint: "[feature-slug]/[component-slug]"
agent: ui-designer
---

## Rôle
Cycle complet pour chaque composant : **créer → vérifier → documenter → specifier (Documentation UX/UI)**.

Les 4 étapes s'enchaînent automatiquement pour chaque composant.


## Prérequis
- [ ] `figma-use` chargé — OBLIGATOIRE avant tout `use_figma`
- [ ] MCP Figma connecté · `figma-read-design` exécuté
- [ ] Tokens DS disponibles dans `design-system/tokens/`
- [ ] Gate 5 en cours — `US-###` et specs UX disponibles
- [ ] `design/[feature-slug]/ux/` specs disponibles — composants créés depuis les specs UX

## Étape 1 — Création du composant Figma
### Étape 1 — Lire la spec UX du composant
Depuis `design/[feature-slug]/ux/component-[slug].md`, extraire :
- Les états : default, hover, pressed, disabled, loading, error
- Les variantes : primary, secondary, destructive
- Les dimensions et contraintes
- Les annotations d'accessibilité

### Étape 2 — Créer le composant de base via `use_figma`

Sur la page `Sandbox` du fichier Projet (ou `🧩 components` du DS si composant global) :

**Structure de layers obligatoire :**
```
[Catégorie] / [Nom] / [Variante] / [État]
    ├── Background (rectangle avec token couleur)
    ├── Content
    │   ├── Icon (si applicable)
    │   ├── Label (texte avec style typographique DS)
    │   └── [autres éléments]
    └── Focus Ring (invisible par défaut — visible sur focus)
```

**Auto Layout obligatoire :**
- Direction : selon le composant (horizontal pour bouton, vertical pour card)
- Padding : valeurs depuis tokens `spacing/*`
- Gap : valeur depuis tokens `spacing/*`
- Hug contents sur les deux axes (sauf si fill container requis)

**Tokens à appliquer :**
- Couleur fond : `color/interactive/[variante]`
- Couleur texte : `color/content/on[Variante]`
- Border radius : `radius/[taille]`
- Ombre : `shadow/[niveau]` si applicable

### Étape 3 — Créer les variants

Via `use_figma`, créer un component set avec les propriétés :

| Propriété | Valeurs |
|-----------|---------|
| `variant` | primary, secondary, destructive |
| `state` | default, hover, pressed, disabled, loading, error |
| `platform` | ios, android (si divergence visuelle) |
| `size` | sm, md, lg (si applicable) |
| `has_icon` | true, false (si applicable) |

Pour chaque combinaison applicable, configurer :
- Les tokens de couleur selon l'état et la variante
- L'opacité pour l'état Disabled (0.4)
- L'indicateur de loading (spinner) pour l'état Loading
- La bordure d'erreur pour l'état Error

### Étape 4 — Vérifier la conformité plateforme

**iOS (HIG) :**
- Zone tactile ≥ 44x44pt — agrandir si nécessaire avec padding invisible
- Texte : style typographique iOS (`iOS/type/[nom]`)
- Coins arrondis : `radius/lg` (12pt) pour les boutons principaux
- Feedback : state change visible (couleur/opacité)

**Android (Material 3) :**
- Zone tactile ≥ 48x48dp
- Texte : style typographique Android (`Android/type/[nom]`)
- Ripple effect : annoter le layer pour l'implémentation dev
- Élévation : `shadow/[niveau]` pour les composants élevés

### Étape 5 — Configurer les propriétés de contenu

Via `use_figma`, exposer les propriétés éditables :
- `Label` : propriété texte (modifiable sans entrer dans le composant)
- `Icon` : swap set d'icônes (si applicable)
- `Show Icon` : boolean (si applicable)

### Étape 6 — Publier dans la bibliothèque

Si le composant est global (réutilisable dans tout le projet) :
1. Déplacer vers la page `🧩 components` du Design System
2. Soumettre au Design System Manager pour validation
3. Publier dans la bibliothèque Figma uniquement après validation DSM

Si le composant est local à la feature :
1. Garder dans la page `Sandbox` du fichier Projet
2. Ne pas publier dans la bibliothèque DS sans validation DSM

## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. Tous les états et variantes définis dans la spec UX sont-ils créés dans le component set ? (oui / non + états manquants)
2. L'auto layout est-il configuré avec des valeurs issues des tokens `spacing/*` — aucune valeur en dur ? (oui / non + valeurs à corriger)
3. La zone tactile est-elle ≥ 44x44pt (iOS) / 48x48dp (Android) sur tous les variants ? (oui / non + variants non conformes)

### ✅ Critère de sortie accessibilité (RAAM niveau A — obligatoire)

Ces critères bloquent le passage au composant ou écran suivant :

4. Chaque élément interactif du composant a-t-il un `accessibilityLabel` défini dans les annotations du layer Figma ? (oui / non + éléments manquants)
5. Le composant transmet-il l'information par autre chose que la couleur seule — icône, texte ou forme accompagne toujours la couleur ? (oui / non — RAAM 2.1)
6. Le ratio de contraste du composant est-il ≥ 3:1 pour les éléments interactifs (RAAM 2.4) ? (oui / non — indiquer le ratio mesuré)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.
> Les critères 4, 5, 6 sont des critères RAAM niveau A — ils bloquent si non conformes.

## Résumé de fin d'exécution

```
✅ create-figma-component "$ARGUMENTS" terminé
🧩 Figma → composant créé, guidelines et accessibilité validés

Prochaines étapes :
→ /check-guidelines-compliance $ARGUMENTS
→ Soumettre au Design System Manager si composant global
→ /create-figma-component $ARGUMENTS (composant suivant)

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```

---

## Étape 2 — Vérification conformité HIG / Material 3 + anti-patterns
### Étape 1 — Lire les propriétés via `use_figma`
Extraire depuis le composant ou l'écran :
- Dimensions de tous les éléments interactifs
- Valeurs de padding et spacing
- Styles typographiques appliqués
- Couleurs et tokens utilisés
- Border radius
- Structure de l'auto layout

### Étape 2 — Audit iOS (HIG)

```markdown
## Audit HIG — iOS

| Critère | Valeur attendue | Valeur trouvée | Statut | Action |
|---------|----------------|----------------|--------|--------|
| Zones tactiles | ≥ 44x44pt | [valeur] | ✅/❌ | [correction] |
| Navigation push | Bottom Sheet ou Push | [type utilisé] | ✅/❌ | |
| Tab bar items | 3-5 max | [nombre] | ✅/❌ | |
| Safe area insets | Respectés | [oui/non] | ✅/❌ | |
| Typographie | Styles iOS/* | [styles trouvés] | ✅/❌ | |
| Border radius boutons | radius/lg (12pt) | [valeur] | ✅/❌ | |
| Dynamic Type | Supporté | [oui/non] | ✅/❌ | |
| SF Symbols | Utilisés si applicable | [oui/non/N/A] | ✅/N/A | |
| Swipe back | Gestionnaire configuré | [oui/non] | ✅/❌ | |
```

### Étape 3 — Audit Android (Material 3)

```markdown
## Audit Material 3 — Android

| Critère | Valeur attendue | Valeur trouvée | Statut | Action |
|---------|----------------|----------------|--------|--------|
| Zones tactiles | ≥ 48x48dp | [valeur] | ✅/❌ | [correction] |
| FAB présent | Si action principale | [oui/non/N/A] | ✅/N/A | |
| Bottom Navigation | 3-5 items | [nombre] | ✅/❌ | |
| Ripple effect | Annoté | [oui/non] | ✅/❌ | |
| Dynamic Color | Material You | [oui/non] | ✅/❌ | |
| Edge-to-edge | Window insets respectés | [oui/non] | ✅/❌ | |
| Typographie | Styles Android/* | [styles trouvés] | ✅/❌ | |
| Élévation | Tokens shadow/* | [oui/non] | ✅/❌ | |
| Material Icons | Utilisés si applicable | [oui/non/N/A] | ✅/N/A | |
```

### Étape 4 — Audit tokens et nomenclature

```markdown
## Audit tokens et nomenclature

| Critère | Attendu | Trouvé | Statut |
|---------|---------|--------|--------|
| Couleurs | Tokens sémantiques uniquement | [oui/non] | ✅/❌ |
| Typographie | Styles DS uniquement | [oui/non] | ✅/❌ |
| Spacing | Tokens spacing/* | [oui/non] | ✅/❌ |
| Radius | Tokens radius/* | [oui/non] | ✅/❌ |
| Nomenclature layers | Convention rules/figma.md | [oui/non] | ✅/❌ |
| Auto layout | Activé sur tous les composants | [oui/non] | ✅/❌ |
| Variants nommés | Convention State/Variant/Platform | [oui/non] | ✅/❌ |
```

### Étape 5 — Rapport de conformité

```markdown
# Rapport Conformité Guidelines — [NomComposant/Écran] — [YYYY-MM-DD]

## Résultat global
- iOS HIG : [✅ Conforme / ⚠️ Réserves / ❌ Non conforme]
- Material 3 : [✅ Conforme / ⚠️ Réserves / ❌ Non conforme]
- Tokens DS : [✅ Conforme / ⚠️ Réserves / ❌ Non conforme]

## Non-conformités bloquantes
| # | Plateforme | Critère | Correction requise |
|---|-----------|---------|-------------------|
| 1 | iOS | Zone tactile bouton X : 36pt < 44pt | Agrandir padding à 4pt minimum |

## Non-conformités mineures
| # | Plateforme | Critère | Correction suggérée |
|---|-----------|---------|-------------------|
| 1 | Android | Ripple non annoté | Ajouter annotation layer |

## Anti-patterns détectés
| # | Anti-pattern | Élément | Correction |
|---|-------------|---------|-----------|
| 1 | [Anti-pattern de la liste `rules/figma.md`] | [Layer/Frame] | [Correction] |

## Verdict
- [ ] ✅ Conforme — composant/écran validé pour la suite
- [ ] ❌ Non conforme — corrections bloquantes à apporter avant de continuer
```

### Étape 5 — Vérification anti-patterns (checklist systématique)

Vérifier la liste des anti-patterns de `rules/figma.md` — section "Anti-patterns UI mobiles" :

**Critiques — bloquants :**
- [ ] Aucun emoji utilisé comme icône de navigation
- [ ] Information jamais transmise par couleur seule
- [ ] Focus rings visibles sur tous les éléments interactifs
- [ ] Boutons désactivés pendant les opérations async
- [ ] Gestes système non bloqués (swipe back iOS, back Android)
- [ ] Toutes les zones tactiles ≥ 44pt iOS / 48dp Android

**Hauts — importants :**
- [ ] Messages d'erreur au niveau du champ — pas uniquement en haut
- [ ] Labels visibles — placeholder ne remplace pas le label
- [ ] `prefers-reduced-motion` respecté sur toutes les animations
- [ ] Aucune valeur hex en dur dans les composants — tokens uniquement
- [ ] Swipe actions ont un affordance visuel
- [ ] Skeleton screens pour les opérations > 1s

**Moyens — à corriger avant handoff :**
- [ ] Un seul jeu d'icônes dans le projet
- [ ] Dark mode : variantes dédiées — pas d'inversion
- [ ] Listes 50+ items : virtualisation prévue (annotation Dev)

## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. Toutes les non-conformités bloquantes ont-elles été corrigées dans Figma ? (oui / non + éléments restants)
2. Les zones tactiles sont-elles conformes sur iOS (≥ 44pt) ET Android (≥ 48dp) sur tous les éléments interactifs ? (oui / non)
3. Aucune valeur de couleur, typographie ou spacing en dur dans les layers — uniquement des tokens DS ? (oui / non + layers à corriger)
4. Tous les anti-patterns critiques de la checklist sont-ils absents ? (oui / non + anti-patterns détectés)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ check-guidelines-compliance "$ARGUMENTS" terminé
📋 Rapport : design/$ARGUMENTS/guidelines-compliance-report.md

Prochaines étapes :
→ /review-figma-scope $ARGUMENTS (PO valide la conformité fonctionnelle)
→ /write-figma-handoff $ARGUMENTS (après validation PO)

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```

---

## Étape 3 — Documentation Design System
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

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```

---

## Étape 4 — Specs Documentation UX/UI (documentation dans Figma)

> Documentation UX/UI génère 5 specs directement dans le fichier Figma DS, sur la page du composant.
> Prérequis : composant publié dans la bibliothèque (Étape 1 terminée) · MCP Figma connecté.

Exécuter les 5 specs dans l'ordre suivant, en fournissant l'URL Figma du component set et une description courte du composant :

---

### 4a — Anatomy (structure interne)

```
/create-anatomy [URL Figma component set]

Ce composant [description courte — ex: "bouton primaire avec état loading et icône optionnelle"].
Inclure tous les éléments cachés (boolean properties).
```

**Ce qui est généré dans Figma :**
- Markers numérotés sur chaque élément du composant
- Tableau 4 colonnes : numéro · type (instance/text) · nom du layer · rôle sémantique
- Sections séparées pour les sous-composants imbriqués

**Règles mobiles spécifiques :**
- Les éléments cachés (boolean toggles) sont inclus et marqués `(hidden)`
- Les sous-composants Spacer/Divider sont automatiquement ignorés
- Nommer les layers en snake_case avant de lancer (les noms deviennent les labels)

---

### 4b — API (propriétés et configuration)

```
/create-api [URL Figma component set]

Propriétés configurables : [variant : primary/secondary/destructive] [state : default/disabled/loading/error]
[has_icon : boolean, défaut false] [label : text, requis]
États persistants uniquement — hover et pressed sont gérés par la plateforme native.
```

**Ce qui est généré dans Figma :**
- Tableau principal : propriétés · valeurs · requis/optionnel · défaut · notes
- Tableaux séparés pour les sous-composants configurables
- 2 à 4 exemples de configuration

**Règles mobiles spécifiques :**
- `hover` et `pressed` → **ne pas inclure** (états runtime iOS/Android, pas des props)
- `disabled`, `selected`, `loading`, `error` → **inclure** (états persistants)
- Pour SwiftUI : les props correspondent aux `@Binding Bool` et `enum`
- Pour Compose : les props correspondent aux paramètres de la fonction composable

---

### 4c — Color Annotation (tokens par état)

```
/create-color [URL Figma component set]

Composant avec variants [primary, secondary, destructive] et états [default, disabled, loading, error].
Inclure le mode dark — les tokens sémantiques gèrent la bascule light/dark automatiquement.
```

**Ce qui est généré dans Figma :**
- Tableau par état : mapping élément → token couleur
- Sections séparées par variant (primary / secondary / destructive)
- Light et dark **ne nécessitent pas de sections séparées** — les tokens sémantiques le gèrent

**Règles mobiles spécifiques :**
- Vérifier que chaque couleur est un token sémantique — jamais une valeur hex en dur
- Pour les composants avec variable modes (ex: Tag color), mentionner la collection et ses modes

---

### 4d — Structure (dimensions pt/dp)

```
/create-structure [URL Figma component set]

Documenter les tailles : [small/medium/large si applicable, ou single size].
Unités : pt pour iOS · dp pour Android.
Inclure : hauteur container · padding horizontal · padding vertical · gap icône-label · taille icône.
Zone tactile minimale : 44×44pt iOS / 48×48dp Android.
```

**Ce qui est généré dans Figma :**
- Tableau par taille/densité : hauteur · padding · gap · dimensions sous-éléments
- Références aux tokens de spacing quand ils existent

**Règles mobiles spécifiques :**
- Toujours indiquer l'unité : `pt` pour iOS, `dp` pour Android (pas de `px`)
- Si zone tactile visuelle < minimum → documenter le padding invisible qui compense
- Density modes (compact/default) → une colonne par mode
- Ne pas documenter de breakpoints desktop

---

### 4e — Screen Reader (VoiceOver + TalkBack)

```
/create-voice [URL Figma component set]

[Description des parties interactives et leur comportement d'annonce]
Ex pour un bouton : "Composant simple — 1 focus stop. Label et hint mergent dans l'annonce.
État disabled : announcé comme non disponible."
Ex pour un champ texte : "Composant compound — 2 focus stops : le champ (label + hint + placeholder mergés)
et le bouton Clear (focus stop séparé, visible uniquement si texte saisi)."
```

**Ce qui est généré dans Figma — par plateforme :**

| Plateforme | Propriétés documentées |
|-----------|----------------------|
| iOS (VoiceOver) | `accessibilityLabel` · `accessibilityTraits` · `accessibilityHint` · `accessibilityValue` |
| Android (TalkBack) | `contentDescription` · `role` · `stateDescription` |

**Règles mobiles spécifiques :**
- **Composant simple** (1 focus stop) : bouton, checkbox, switch, toggle
- **Composant compound** (2+ focus stops) : champ texte + clear button, chip + dismiss, tab bar
- Icônes décoratives → `hidden from screen readers` (ne pas documenter comme focus stop)
- Icônes fonctionnelles → focus stop séparé avec label explicite
- Labels, hints et placeholders → mergent dans l'annonce du contrôle parent
- Documenter comment l'annonce change selon l'état (ex: switch → "activé" / "désactivé")

---

### Rapport Documentation UX/UI — fin d'étape 4

Après les 5 specs, produire le récapitulatif :

```markdown
## Rapport Documentation UX/UI — [ComponentSlug] — [YYYY-MM-DD]

| Spec | Statut | URL Figma frame | Notes |
|------|--------|----------------|-------|
| Anatomy | ✅ / ❌ | [URL] | |
| API | ✅ / ❌ | [URL] | |
| Color Annotation | ✅ / ❌ | [URL] | |
| Structure | ✅ / ❌ | [URL] | |
| Screen Reader | ✅ / ❌ | [URL] | |

Layers Figma nommés correctement avant génération : ✅ / ❌
Tokens sémantiques uniquement (0 hex en dur) : ✅ / ❌
Zone tactile documentée (pt iOS / dp Android) : ✅ / ❌
```

---

## Résumé de fin d'exécution

```
✅ write-component "$ARGUMENTS" terminé

Cycle 4 étapes :
  1. Création    → composant set publié dans DS
  2. Vérification → conformité HIG/M3 validée
  3. Documentation → design-system/documentation/[slug].md
  4. Documentation UX/UI       → 5 specs dans Figma (Anatomy · API · Color · Structure · Screen Reader)

🎨 Figma DS → composant publié · specs Documentation UX/UI rendues
📁 design-system/documentation/[component-slug].md

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]

Prochaines étapes :
→ /write-component $ARGUMENTS/[composant-suivant]
→ Gate 5 quand tous les composants sont créés
```
