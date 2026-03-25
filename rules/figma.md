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



## Structure des fichiers Figma

### Deux fichiers distincts par projet

```
[NomProjet] — Design System.fig
    ├── 🎨 Foundations
    │   ├── Colors
    │   ├── Typography
    │   ├── Spacing
    │   ├── Grid
    │   ├── Shadows
    │   └── Motion
    ├── 🧩 Components
    │   ├── Atoms (boutons, inputs, badges...)
    │   ├── Molecules (cards, forms, list items...)
    │   └── Organisms (headers, navbars, modals...)
    ├── 📐 Patterns
    │   └── [Patterns réutilisables]
    └── 📖 Documentation
        └── [Changelog, usage, guidelines]

[NomProjet] — [EPIC-###] [NomEpic].fig
    ├── 🗺️ User Flows
    │   └── FLUX-###-[slug]
    ├── 📱 Screens
    │   ├── [US-###] [NomUS]
    │   │   ├── S-01-[nom] / iOS
    │   │   ├── S-01-[nom] / Android
    │   │   └── S-01-[nom] / States (loading, empty, error)
    │   └── ...
    ├── 🔧 Components (locaux — feature uniquement)
    ├── 🚀 Handoff
    │   └── [Frames prêtes pour dev]
    └── 📦 Store Assets
        ├── App Icon
        ├── Android Banner
        └── Screenshots (smartphone + tablet)
```

---

## Nomenclature des pages Figma

### Fichier Design System
| Page | Nom exact | Description |
|------|----------|-------------|
| Fondations | `🎨 Foundations` | Tokens primitifs et sémantiques |
| Composants | `🧩 Components` | Bibliothèque de composants |
| Patterns | `📐 Patterns` | Assemblages réutilisables |
| Documentation | `📖 Documentation` | Changelog et guidelines |

### Fichier Projet (par EPIC)
| Page | Nom exact | Description |
|------|----------|-------------|
| User Flows | `🗺️ User Flows` | Flows visuels FLUX-### |
| Écrans | `📱 Screens` | Frames par US |
| Composants locaux | `🔧 Components` | Composants spécifiques à la feature |
| Handoff | `🚀 Handoff` | Frames Dev Ready |
| Store Assets | `📦 Store Assets` | Assets pour publication |

---

## Nomenclature des frames

### Convention obligatoire
```
[US-###] [NomUS] / [Plateforme] / [État]
```

Exemples :
```
[US-042] Login / iOS / Default
[US-042] Login / iOS / Loading
[US-042] Login / iOS / Error
[US-042] Login / Android / Default
[US-001] Onboarding / iOS / Step 1
```

### Règles
- Toujours préfixer avec l'ID de la US couverte
- Toujours préciser la plateforme : `iOS` / `Android`
- Toujours préciser l'état : `Default` / `Loading` / `Empty` / `Error` / `Success`
- Jamais d'espaces dans les noms de frames — utiliser des espaces normaux, pas de tirets
- Jamais de majuscules aléatoires — sentence case uniquement

---

## Nomenclature des layers

### Règle générale
```
[type] — [nom] — [état si applicable]
```

| Type | Préfixe | Exemple |
|------|---------|---------|
| Frame | Aucun — nom direct | `Header` |
| Groupe | Aucun — nom descriptif | `Navigation Bar` |
| Composant | Nom du composant | `Button / Primary / Default` |
| Texte | Contenu ou rôle | `Title`, `Body`, `Label` |
| Image | Rôle | `Hero Image`, `Avatar` |
| Icône | Nom de l'icône | `Icon / Search` |
| Rectangle décoratif | `BG` + description | `BG Overlay` |

### À ne jamais faire
- ❌ `Rectangle 1`, `Group 4`, `Frame 23` — noms auto Figma
- ❌ `Calque copie 2` — doublon non renommé
- ❌ `TITRE` — tout en majuscules
- ❌ `btn-primary-hover-active-v3-FINAL` — suffixes de version

---

## Nomenclature des composants

### Convention
```
[Catégorie] / [Nom] / [Variante] / [État]
```

Exemples :
```
Button / Primary / Default
Button / Primary / Hover
Button / Secondary / Disabled
Input / Text / Default
Input / Text / Error
Card / Product / With Image
Navigation / Tab Bar / iOS
Navigation / Tab Bar / Android
```

### Propriétés de variants obligatoires
Tout composant avec plusieurs états doit avoir ces propriétés définies :

| Propriété | Valeurs possibles |
|-----------|-----------------|
| `State` | Default, Hover, Pressed, Disabled, Loading, Error |
| `Platform` | iOS, Android (si divergence) |
| `Size` | SM, MD, LG (si applicable) |
| `Variant` | Primary, Secondary, Destructive (si applicable) |

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
