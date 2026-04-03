# Conventions Figma — Projet et Design System

## ✅ MIS À JOUR — Outils MCP Figma disponibles

Le MCP Figma expose deux types d'opérations : **lecture** et **écriture**. La règle fondamentale est de toujours lire avant d'écrire.

```
Règle : get_metadata → get_variable_defs → use_figma
         (lire la structure)  (lire les tokens)  (écrire)
```

### Outils de lecture

| Outil MCP | Action | Quand l'utiliser |
|----------|--------|-----------------|
| `get_metadata` | Structure XML légère d'une sélection — IDs, noms, types, positions | Avant d'écrire — vérifier ce qui existe |
| `get_design_context` | Contexte design complet d'un frame (React+Tailwind par défaut) | Lire un écran existant pour générer du code |
| `get_variable_defs` | Variables et styles d'une sélection (couleurs, spacing, typo) | Vérifier les tokens disponibles avant toute création |
| `get_screenshot` | Screenshot de la sélection | Vérifier visuellement un frame ou composant |
| `get_figjam` | Contenu d'un diagram FigJam en XML | Lire un flow ou diagramme existant |
| `get_code_connect_map` | Mapping node Figma → composant code | Vérifier les connexions Code Connect existantes |
| `whoami` | Identité de l'utilisateur authentifié | Vérifier les permissions avant d'écrire |

### Outils d'écriture

| Outil MCP | Action | Skill associé |
|----------|--------|--------------|
| `use_figma` | Outil universel d'écriture — crée/modifie frames, composants, variables, auto layout | `figma-use-wrapper` (obligatoire avant tout) |
| `generate_diagram` | Génère un diagramme FigJam depuis Mermaid | `write-figma-userflow` |
| `add_code_connect_map` | Ajoute un mapping node → code | `figma-code-connect` |
| `send_code_connect_mappings` | Confirme les mappings Code Connect | `figma-code-connect` |
| `create_design_system_rules` | Génère un fichier de rules depuis le design system Figma | `setup-figma-tokens` |

### Outils de suggestion (Figma-prompted)

| Outil MCP | Action |
|----------|--------|
| `get_code_connect_suggestions` | Suggère des mappings Figma → code automatiquement |
| `search_design_system` | Recherche composants, variables et styles dans les bibliothèques connectées |

---

## ✅ NOUVEAU — Règle fondamentale : lire avant écrire

Avant tout `use_figma`, l'agent doit vérifier :

```
1. get_metadata        → Quels layers existent déjà dans le frame ?
2. get_variable_defs   → Quels tokens sont déjà disponibles ?
3. search_design_system → Quels composants existent dans les bibliothèques ?
4. use_figma           → Écrire en réutilisant ce qui existe
```

**Pourquoi ?** Sans lecture préalable, l'agent crée des doublons, ignore les composants existants et utilise des valeurs en dur au lieu des tokens déjà définis.

---

## ✅ NOUVEAU — Distinction Remote vs Desktop MCP

| Mode | Accès écriture | Accès lecture | Contrainte |
|------|---------------|--------------|------------|
| **Remote** (recommandé) | ✅ Write to canvas disponible | ✅ Complet | Requiert Full Seat pour écrire hors drafts |
| **Desktop** | ✅ Selection-based | ✅ Complet | Dev Seat = lecture uniquement hors drafts |

**Règle projet :** Utiliser le serveur Remote pour toutes les opérations d'écriture. Si un Dev Seat est utilisé, restreindre les agents aux opérations de lecture uniquement.

**Vérification de permissions avant écriture :**
```
whoami → vérifier seat type → Full Seat requis pour use_figma hors drafts
```

---

## ✅ NOUVEAU — figma-use comme skill fondateur

`figma-use` est le skill officiel Figma qui enseigne à l'agent comment utiliser correctement `use_figma`. Il est **obligatoire** avant tout skill d'écriture sur le canvas.

**Ordre d'invocation :**
```
/figma-use-wrapper [action]   ← notre skill local qui wrappe figma-use
    → invoque figma-use (skill officiel Figma)
    → utilise use_figma (outil MCP)
```

Nos skills Figma (`setup-figma-project`, `create-figma-component`, etc.) sont des extensions de ce skill fondateur — ils le présupposent installé.

---

## Référentiel technique

| Mode | Usage | Skill déclencheur |
|------|-------|------------------|
| `use_figma` (Remote MCP) | Écriture — frames, composants, variables, auto layout | `figma-use-wrapper` |
| Outils de lecture MCP | Lecture — structure, tokens, composants existants | `figma-read-design` |
| `web_fetch` + `use_figma` | Injection contenu web → frames Figma | `fetch-content-for-frames` |
| Instructions manuelles | Fallback si MCP indisponible ou Dev Seat | Tous les skills (section fallback) |



## Convention de nomenclature générale

### Règle fondamentale
```
snake_case pour les mots
/  pour les niveaux hiérarchiques (composants, tokens, pages)
_  seul pour les frames et layers (pas de /)
IDs en MAJUSCULES : US_042, EPIC_001, FLUX_003
Tout le reste en minuscules
```

### Récapitulatif par élément

| Élément | Convention | Exemple |
|---------|-----------|---------|
| Frame | `snake_case` | `US_042_login_ios_default` |
| Layer | `snake_case` | `button_primary_default` |
| Composant | `snake_case/niveaux` | `button/primary_with_icon/default` |
| Token couleur | `snake_case/niveaux` | `color/interactive/primary` |
| Token typo | `snake_case/niveaux` | `ios/type/body` |
| Token spacing | `snake_case/niveaux` | `spacing/md` |
| Page Figma | `emoji + snake_case` | `🎨 foundations` |
| Section dans une page | `snake_case` | `US_042_login` |
| Mode variable | `snake_case` | `light`, `dark`, `brand_a` |
| Prototype | `snake_case` | `prototype_login_flow_v1.html` |
| ID (US, EPIC, FLUX) | `MAJUSCULES_###` | `US_042`, `EPIC_001`, `FLUX_003` |

---

## Structure des fichiers Figma

### Deux fichiers distincts par projet

```
[nom_projet]_design_system.fig
    ├── 🎨 foundations
    │   ├── colors
    │   ├── typography
    │   ├── spacing
    │   ├── grid
    │   ├── shadows
    │   └── motion
    ├── 🧩 components
    │   ├── atoms (boutons, inputs, badges...)
    │   ├── molecules (cards, forms, list items...)
    │   └── organisms (headers, navbars, modals...)
    ├── 📐 patterns
    │   └── [patterns réutilisables]
    └── 📖 documentation
        └── [changelog, usage, guidelines]

[nom_projet]_[EPIC_###]_[nom_epic].fig
    ├── ✏️ Design
    │   ├── Work In Progress UI     ← écrans en cours (sections par US_###)
    │   ├── Validated UI            ← écrans validés par le Product Designer
    │   ├── Ready for dev           ← écrans prêts pour implémentation
    │   └── Accessibility           ← annotations RAAM
    ├── 🔬 Research & UX
    │   ├── Userflow                ← flows visuels FLUX_###
    │   ├── Work In Progress Wireframes
    │   └── Validated wireframe
    ├── 📚 Ressources
    │   ├── Moodboard
    │   ├── Benchmark
    │   └── Client documentation
    ├── 📦 Deliverables
    │   ├── App Icon
    │   └── Store screen
    ├── 🗄️ Archives
    ├── 🧪 Sandbox
    └── 🖼️ Cover
```

---

## Nomenclature snake_case — règle universelle

Tous les noms dans Figma suivent le **snake_case strict** sans exception :
- Frames, layers, sections, composants, variables, pages
- Jamais d'espaces, de majuscules, de caractères spéciaux
- Jamais de noms auto Figma (Frame 1, Group 7, Rectangle...)
- IDs en MAJUSCULES uniquement : `US_042`, `EPIC_001`, `FEAT_003`

---

## Nomenclature des pages Figma

### Fichier Design System — structure Neow DS

| Page | Nom exact | Contenu |
|------|----------|---------|
| Accueil | `Get started` | Purpose, usage, contacts |
| Changelog | `Change log` | Historique des versions |
| **Foundations** | `🧱 Foundations` | En-tête de section |
| Couleurs | `Colors` | Tokens primitifs + sémantiques + dark mode |
| Typographie | `Typography` | Type scale iOS · Android |
| Icônes | `Icons` | Bibliothèque + règles d'usage |
| Grilles | `Grid & spacing` | Grilles iOS/Android · spacing tokens |
| Logo | `Logo` | Assets logo + règles |
| Ombres | `Shadows & Blur` | Élévations, blur |
| Images | `Images` | Règles images, placeholders |
| Animations | `Animations` | Tokens motion, easing |
| **Components** | `⚛️ Components` | En-tête de section |
| Actions | `Actions` | Boutons, FAB, liens |
| Navigation | `Navigation` | Tab bar, nav bar, drawer |
| Contenu | `Content` | Cards, listes, media |
| Champs | `TextField` | Inputs, search, textarea |
| Formulaires | `Forms` | Formulaires, validation |
| Contrôles | `Controls` | Checkbox, radio, toggle |
| Listes | `Lists` | List items, groups |
| Onglets | `Tabs` | Tabs, segmented control |
| Avatar | `Avatar` | Avatars, user badges |
| Feedbacks | `Feedbacks` | Toasts, alerts, banners |
| Accordéon | `Accordion` | Collapse, expand |
| Indicateurs | `Indicators` | Badges, chips, tags, progress |
| Cartes | `Cards` | Card variants |
| Graphiques | `Chart` | Data viz (vide si non applicable) |
| Slider | `Slider` | Sliders, range |
| Patterns | `Pattern` | Assemblages réutilisables |
| Modales | `Modals & Pop-up` | Modales, bottom sheets, popovers |
| Loaders | `Loader` | Spinners, skeletons, progress |
| Mockup | `Mock up` | Device frames |
| Natif | `Native Elements` | Status bar, keyboard, system UI |
| Archivage | `🗃️ Unused Components` | Composants archivés |
| Outils | `🛠️ Utilities` | Grilles, annotations |
| Couverture | `🎇 Cover` | Thumbnail fichier |

### Fichier Projet (par EPIC)

**Section Design**
| Page | Nom exact | Contenu |
|------|----------|---------|
| UI en cours | `Work In Progress UI` | Frames actives — sections par `US_###` |
| UI validée | `Validated UI` | Frames approuvées par le Product Designer |
| Prêt pour dev | `Ready for dev` | Frames handoff avec annotations complètes |
| Accessibilité | `Accessibility` | Annotations RAAM sur les frames validées |

**Section Research & UX**
| Page | Nom exact | Contenu |
|------|----------|---------|
| Flows | `Userflow` | Flows visuels `FLUX_###` |
| Wireframes en cours | `Work In Progress Wireframes` | Wireframes actifs |
| Wireframes validés | `Validated wireframe` | Wireframes approuvés |

**Section Ressources**
| Page | Nom exact | Contenu |
|------|----------|---------|
| Moodboard | `Moodboard` | Références visuelles et inspiration |
| Benchmark | `Benchmark` | Captures et analyse UI concurrentielle |
| Docs client | `Client documentation` | Brief, charte, assets fournis |

**Section Deliverables**
| Page | Nom exact | Contenu |
|------|----------|---------|
| Icône | `App Icon` | Toutes tailles iOS + Android |
| Store | `Store screen` | Screenshots store iOS + Android |

**Pages indépendantes**
| Page | Nom exact | Contenu |
|------|----------|---------|
| Archives | `Archives` | Écrans et versions obsolètes |
| Sandbox | `Sandbox` | Explorations libres et tests visuels |
| Cover | `Cover` | Thumbnail du fichier |

### Flux des frames entre les pages Design

```
Work In Progress UI  →  Validated UI  →  Ready for dev
     (en cours)           (validé PD)        (handoff)
         ↓
    Accessibility (annotations ajoutées en parallèle de Validated)
```

> ⚠️ Règle stricte : une frame ne passe jamais de WIP directement à Ready for dev.
> Elle doit d'abord être dans Validated UI.

### Sections dans `Work In Progress UI`
```
snake_case — ID US en majuscules
Ex : US_042_login
Ex : US_001_onboarding
```

---

## Nomenclature des frames

### Convention obligatoire
```
US_###_[nom_us]_[plateforme]_[état]
```

Règles :
- `US_###` en majuscules — identifiant unique
- Tout le reste en minuscules et snake_case
- Plateforme : `ios` / `android`
- État : `default` / `loading` / `empty` / `error` / `success`

Exemples :
```
US_042_login_ios_default
US_042_login_ios_loading
US_042_login_ios_error
US_042_login_android_default
US_001_onboarding_ios_step_1
US_001_onboarding_ios_step_2
```

### À ne jamais faire
- ❌ `Rectangle 1`, `Group 4`, `Frame 23` — noms auto Figma
- ❌ `Calque copie 2` — doublon non renommé
- ❌ `Login Screen FINAL v3` — majuscules + suffixes de version
- ❌ `[US-042] Login / iOS / Default` — ancienne convention

---

## Nomenclature des layers

### Convention obligatoire
```
snake_case — tout en minuscules
```

| Type | Convention | Exemple |
|------|-----------|---------|
| Frame conteneur | `snake_case` | `header`, `content`, `footer` |
| Groupe | `snake_case` | `navigation_bar`, `form_section` |
| Composant lié | Nom du composant | `button/primary/default` |
| Texte | Rôle ou contenu court | `title`, `body`, `label_email` |
| Image | Rôle | `hero_image`, `avatar` |
| Icône | `icon_[nom]` | `icon_search`, `icon_close` |
| Rectangle décoratif | `bg_[description]` | `bg_overlay`, `bg_card` |
| Layer ignoré VoiceOver | `decoration_[description]` | `decoration_background` |

### À ne jamais faire
- ❌ `Rectangle 1`, `Group 4` — noms auto
- ❌ `Hero Image` — espaces
- ❌ `Icon / Search` — slash dans un layer (réservé aux composants)
- ❌ `BG Overlay` — majuscules

---

## Nomenclature des composants

### Convention obligatoire
```
[catégorie]/[nom_composant]/[variante]/[état]
snake_case pour chaque niveau, / pour séparer les niveaux
```

Exemples :
```
button/primary/default
button/primary/loading
button/primary_with_icon/default
button/secondary/disabled
input/text/default
input/text/error
input/password/default
card/product/with_image
card/product/without_image
navigation/tab_bar/ios
navigation/tab_bar/android
navigation/bottom_sheet/default
```

### Propriétés de variants obligatoires

Tout composant avec plusieurs états doit avoir ces propriétés en snake_case :

| Propriété | Valeurs possibles |
|-----------|-----------------|
| `state` | `default`, `hover`, `pressed`, `disabled`, `loading`, `error` |
| `platform` | `ios`, `android` (si divergence visuelle) |
| `size` | `sm`, `md`, `lg` (si applicable) |
| `variant` | `primary`, `secondary`, `destructive` (si applicable) |
| `has_icon` | `true`, `false` (si applicable) |

### Modes de variables

```
light
dark
brand_a
brand_b
high_contrast
```

---

## Nomenclature des tokens

### Convention déjà en snake_case — inchangée
```
color/[catégorie]/[rôle]
Ex : color/interactive/primary
Ex : color/content/secondary
Ex : color/feedback/error

spacing/[taille]
Ex : spacing/xs, spacing/sm, spacing/md, spacing/lg, spacing/xl

radius/[taille]
Ex : radius/sm, radius/md, radius/lg

ios/type/[style]
Ex : ios/type/body, ios/type/headline, ios/type/caption

android/type/[style]
Ex : android/type/body_large, android/type/label_medium
```

> Les tokens sont déjà en snake_case + `/` — aucun changement requis.

---

## Grilles par plateforme

### iOS
| Contexte | Colonnes | Gouttière | Marge | Base unit |
|----------|----------|-----------|-------|-----------|
| iPhone (375pt) | 4 | 16pt | 16pt | 8pt |
| iPhone Large (428pt) | 4 | 16pt | 20pt | 8pt |
| iPad (768pt) | 8 | 24pt | 24pt | 8pt |

### Android
| Contexte | Colonnes | Gouttière | Marge | Base unit |
|----------|----------|-----------|-------|-----------|
| Mobile (360dp) | 4 | 16dp | 16dp | 8dp |
| Mobile Large (412dp) | 4 | 16dp | 16dp | 8dp |
| Tablet (600dp+) | 8 | 24dp | 24dp | 8dp |

### Auto Layout — règles
- Padding interne des composants : multiples de 4pt/dp
- Espacement entre éléments : multiples de 4pt/dp
- Toujours activer Auto Layout sur les composants — jamais de positionnement fixe
- `Hug contents` pour les composants de taille variable
- `Fill container` pour les éléments qui doivent s'étendre

---

## Guidelines de conformité plateforme

### iOS — Human Interface Guidelines (HIG)
| Règle | Valeur | Élément concerné |
|-------|--------|-----------------|
| Zones tactiles min. | 44x44pt | Tous les éléments interactifs |
| Navigation | Push + Bottom sheet | Navigation principale |
| Geste retour | Swipe depuis bord gauche | Navigation back |
| Action principale | Bouton en bas de l'écran | CTA principal |
| Tab bar | 5 items max | Navigation globale |
| Modales | Bottom sheet (≥ iOS 15) | Contenu contextuel |
| Safe areas | Respecter les insets | iPhone avec notch/Dynamic Island |

### Android — Material Design 3
| Règle | Valeur | Élément concerné |
|-------|--------|-----------------|
| Zones tactiles min. | 48x48dp | Tous les éléments interactifs |
| Navigation | Navigation back système | Navigation retour |
| Action principale | FAB (Floating Action Button) | Action primaire |
| Bottom Navigation | 3-5 items | Navigation globale |
| Snackbars | 4s display max | Notifications temporaires |
| Dynamic Color | Material You | Theming adaptatif |
| Edge-to-edge | Respecter les window insets | Contenu plein écran |

---

## ✅ NOUVEAU — Règles UX atomiques par priorité

Règles d'interaction et de qualité UI à vérifier dans `check-guidelines-compliance`. Organisées par priorité décroissante.

### Priorité 1 — Accessibilité (CRITIQUE)

| Règle | Description | Référence |
|-------|-------------|-----------|
| `color_contrast` | Ratio min. 4.5:1 texte normal, 3:1 grand texte | WCAG AA |
| `focus_states` | Focus rings visibles sur tous les éléments interactifs (2-4px) | HIG, MD |
| `alt_text` | Alt text descriptif sur toutes les images informatives | WCAG |
| `aria_labels` | `accessibilityLabel` sur tous les boutons icône | HIG |
| `color_not_only` | Ne jamais transmettre l'info par couleur seule — ajouter icône ou texte | WCAG |
| `dynamic_type` | Supporter le scaling texte système — pas de troncature | HIG Dynamic Type |
| `reduced_motion` | Respecter `prefers-reduced-motion` — désactiver les animations | HIG |
| `voiceover_sr` | Ordre de lecture logique pour VoiceOver/TalkBack | HIG, MD |
| `escape_routes` | Toujours fournir Cancel/Back dans les modales et flows multi-étapes | HIG |

### Priorité 2 — Touch & Interaction (CRITIQUE)

| Règle | Description | Référence |
|-------|-------------|-----------|
| `touch_target_size` | Min 44×44pt iOS / 48×48dp Android — étendre au-delà du visuel si besoin | HIG, MD |
| `touch_spacing` | Min 8pt/dp d'espace entre deux zones tactiles | HIG, MD |
| `hover_vs_tap` | Jamais d'interaction critique hover-only | — |
| `loading_buttons` | Désactiver le bouton pendant une opération async — montrer un spinner | HIG |
| `error_feedback` | Message d'erreur au plus proche du champ problématique | HIG |
| `gesture_conflicts` | Éviter le swipe horizontal sur le contenu principal | HIG |
| `standard_gestures` | Utiliser les gestes standard de la plateforme — ne pas les redéfinir | HIG |
| `system_gestures` | Ne pas bloquer les gestes système (Control Center, swipe back...) | HIG |
| `press_feedback` | Feedback visuel immédiat au tap (ripple MD / highlight iOS) | HIG, MD |
| `haptic_feedback` | Haptic sur confirmations importantes — ne pas en abuser | HIG |
| `safe_area_awareness` | Zones tactiles loin du notch, Dynamic Island, gesture bar | HIG |
| `swipe_clarity` | Swipe actions doivent avoir un affordance visible (chevron, label) | HIG |
| `tap_feedback_speed` | Feedback visuel dans les 100ms après le tap | HIG |

### Priorité 3 — Performance (HAUTE)

| Règle | Description | Référence |
|-------|-------------|-----------|
| `progressive_loading` | Skeleton screens au lieu de spinners pour les opérations >1s | HIG |
| `input_latency` | Latence input < 100ms pour taps/scroll | MD |
| `debounce_throttle` | Debounce/throttle sur scroll, resize, input haute fréquence | — |
| `main_thread_budget` | Travail par frame < 16ms pour 60fps | HIG, MD |
| `offline_support` | Message d'état offline + fallback minimal | — |
| `virtualize_lists` | Virtualiser les listes de plus de 50 items | HIG, MD |

### Priorité 4 — Style et cohérence (HAUTE)

| Règle | Description | Référence |
|-------|-------------|-----------|
| `no_emoji_icons` | Utiliser des icônes SVG — jamais d'emojis dans la navigation | HIG |
| `platform_adaptive` | Respecter les idiomes plateforme : navigation, contrôles, typo, motion | HIG, MD |
| `state_clarity` | États hover/pressed/disabled visuellement distincts | MD |
| `elevation_consistent` | Échelle d'élévation/ombre cohérente — pas de valeurs aléatoires | MD |
| `dark_mode_pairing` | Concevoir light et dark ensemble pour garantir la cohérence | HIG |
| `icon_style_consistent` | Un seul jeu d'icônes par projet | HIG |

### Priorité 5 — Typographie et couleurs (MOYENNE)

| Règle | Description | Référence |
|-------|-------------|-----------|
| `text_styles_system` | Utiliser le système de styles plateforme — Dynamic Type iOS / Type roles MD | HIG, MD |
| `weight_hierarchy` | Bold titres (600-700), Regular corps (400), Medium labels (500) | MD |
| `color_semantic` | Tokens sémantiques uniquement — jamais de hex brut dans les composants | MD |
| `color_dark_mode` | Dark mode : variantes désaturées/plus claires — pas de couleurs inversées | HIG, MD |
| `color_accessible_pairs` | Paires fond/texte vérifiées à 4.5:1 (AA) ou 7:1 (AAA) | WCAG |

### Priorité 6 — Animation (MOYENNE)

| Règle | Description | Référence |
|-------|-------------|-----------|
| `animation_duration` | Durée 150-300ms — pas d'animation instantanée (0ms) ni trop longue | HIG, MD |
| `motion_meaning` | L'animation doit avoir un sens — pas de décoratif pur | HIG |
| `spatial_continuity` | Les éléments qui bougent doivent suivre une trajectoire logique | HIG |

### Priorité 7 — Formulaires et feedback (MOYENNE)

| Règle | Description | Référence |
|-------|-------------|-----------|
| `visible_labels` | Labels toujours visibles — ne pas utiliser le placeholder comme label | HIG |
| `error_near_field` | Erreur affichée au plus proche du champ concerné | HIG |
| `progressive_disclosure` | Ne pas surcharger le formulaire — révéler progressivement | HIG |

### Priorité 8 — Navigation (HAUTE)

| Règle | Description | Référence |
|-------|-------------|-----------|
| `predictable_back` | Comportement Back toujours prévisible | HIG |
| `bottom_nav_limit` | Bottom navigation : 3 à 5 items max | HIG, MD |
| `deep_linking` | Prévoir le deep linking pour les flux critiques | HIG, MD |

---

## Anti-patterns UI mobiles — À ne jamais faire

Ces patterns sont vérifiés dans `check-guidelines-compliance` avant tout handoff.

| ❌ Anti-pattern | ✅ Alternative | Priorité |
|----------------|--------------|---------|
| Emojis comme icônes de navigation | SVG icons (SF Symbols iOS / Material Icons Android) | Critique |
| Couleur seule pour transmettre une info | Couleur + icône ou texte | Critique |
| Focus rings supprimés | Focus rings toujours visibles | Critique |
| Bouton actif pendant une opération async | Désactiver + spinner | Critique |
| Gestes système bloqués (swipe back iOS) | Ne jamais intercepter les gestes système | Critique |
| Zone tactile < 44pt / 48dp | Padding invisible pour agrandir la zone | Critique |
| Message d'erreur uniquement en haut du formulaire | Erreur au niveau du champ concerné | Haute |
| Placeholder utilisé comme label | Label visible au-dessus du champ | Haute |
| Animation sans `prefers-reduced-motion` | Vérifier et respecter le setting système | Haute |
| Valeurs hex en dur dans les composants | Tokens sémantiques uniquement | Haute |
| Swipe action sans affordance visuelle | Chevron ou label visible | Haute |
| Spinner bloquant pour toute opération >1s | Skeleton screen | Moyenne |
| Icônes de styles différents dans le même écran | Un seul jeu d'icônes par projet | Moyenne |
| Dark mode = couleurs inversées | Variantes désaturées/plus claires dédiées | Moyenne |
| Listes de 50+ items non virtualisées | Virtualisation obligatoire | Moyenne |

---

## Process de handoff

### Critères pour qu'un frame soit `Ready for dev`
- [ ] Nommé selon la convention `[US-###] [NomUS] / [Plateforme] / [État]`
- [ ] Tous les layers nommés (aucun `Rectangle 1` ou `Group 4`)
- [ ] Auto Layout activé sur tous les composants
- [ ] Grille active sur les frames principaux
- [ ] Tokens design system appliqués — aucune valeur en dur
- [ ] Annotations RAAM complètes (voir `rules/accessibility.md`)
- [ ] Référence spec `.md` dans la description du frame
- [ ] Composants liés à la bibliothèque Design System (pas de copie locale)

### Structure des annotations de handoff
Dans la description de chaque frame `Ready for dev` :
```
Spec : S-XX-[nom] | US-###, US-### | FLUX-###
Statut : Ready for dev
Annotations RAAM : Done
Grille : iOS 375pt / 4 colonnes
Tokens : Design System v[X.X.X]
```

---

## Assets Store — formats obligatoires

### App Icon
| Plateforme | Taille | Format |
|-----------|--------|--------|
| iOS App Store | 1024x1024pt | PNG sans transparence |
| Android Play Store | 512x512dp | PNG |
| iOS Home Screen | Multiple — généré depuis 1024pt | Auto via Xcode |
| Android Launcher | Multiple — généré depuis 512dp | Auto via Android Studio |

### Screenshots Store
| Plateforme | Tailles requises | Nombre min. |
|-----------|-----------------|------------|
| iOS App Store | 6.7" (1290x2796px) + 5.5" (1242x2208px) | 5 par taille |
| iPad App Store | 12.9" (2048x2732px) | 5 |
| Android Play Store | 1080x1920px (smartphone) | 5 |
| Android Play Store | 1200x1920px (tablet) | 2 |

### Android Feature Banner
| Format | Taille | Usage |
|--------|--------|-------|
| Feature Graphic | 1024x500px | Mise en avant Play Store |
