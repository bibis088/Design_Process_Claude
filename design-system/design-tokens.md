# Design Tokens — [Nom du projet]

<!-- Format compatible Google Stitch / IA générative -->
<!-- Géré par : Design System Manager -->
<!-- Dernière mise à jour : [date] -->
<!-- Référence process : voir ../design.md -->

---

## Colors

### Brand
- Primary: `#[000000]`               <!-- Couleur principale de la marque -->
- Primary Dark: `#[000000]`          <!-- Variante sombre pour hover/pressed -->
- Primary Light: `#[000000]`         <!-- Variante claire pour backgrounds -->
- Secondary: `#[000000]`             <!-- Couleur secondaire / accent -->
- Secondary Dark: `#[000000]`
- Secondary Light: `#[000000]`

### Semantic
- Success: `#[000000]`               <!-- Validation, confirmation -->
- Warning: `#[000000]`               <!-- Alerte non critique -->
- Error: `#[000000]`                 <!-- Erreur, destructif -->
- Info: `#[000000]`                  <!-- Information neutre -->

### Surfaces
- Background: `#[000000]`            <!-- Fond de page principal -->
- Surface: `#[000000]`               <!-- Fond de carte, panel -->
- Surface Elevated: `#[000000]`      <!-- Fond de modal, dropdown -->
- Border: `#[000000]`                <!-- Contour par défaut -->
- Border Strong: `#[000000]`         <!-- Contour accentué -->
- Overlay: `rgba(0,0,0,0.XX)`        <!-- Fond semi-transparent de modal -->

### Text
- Text Primary: `#[000000]`          <!-- Corps de texte principal -->
- Text Secondary: `#[000000]`        <!-- Texte secondaire, labels -->
- Text Disabled: `#[000000]`         <!-- Texte inactif -->
- Text Inverse: `#[000000]`          <!-- Texte sur fond sombre -->
- Text On Primary: `#[000000]`       <!-- Texte sur bouton primary -->

### Dark Mode
- Background (dark): `#[000000]`
- Surface (dark): `#[000000]`
- Text Primary (dark): `#[000000]`
- Border (dark): `#[000000]`

---

## Typography

### Font Families
- Font Primary: `[NomPolice], [fallback], sans-serif`   <!-- Corps, UI -->
- Font Display: `[NomPolice], [fallback], serif`         <!-- Titres (si différent) -->
- Font Mono: `[NomPolice], monospace`                   <!-- Code, données -->

### Type Scale
| Token | Taille | Graisse | Line Height | Usage |
|-------|--------|---------|-------------|-------|
| Display 1 | `[0]px` | `[000]` | `[0.0]` | Hero, écran d'accueil |
| Heading 1 | `[0]px` | `[000]` | `[0.0]` | Titre de page |
| Heading 2 | `[0]px` | `[000]` | `[0.0]` | Titre de section |
| Heading 3 | `[0]px` | `[000]` | `[0.0]` | Titre de carte |
| Body Large | `[0]px` | `[000]` | `[0.0]` | Corps de texte principal |
| Body | `[0]px` | `[000]` | `[0.0]` | Corps de texte standard |
| Body Small | `[0]px` | `[000]` | `[0.0]` | Texte secondaire |
| Caption | `[0]px` | `[000]` | `[0.0]` | Légendes, métadonnées |
| Label | `[0]px` | `[000]` | `[0.0]` | Labels de champs |
| Button | `[0]px` | `[000]` | `[0.0]` | Texte de bouton |
| Code | `[0]px` | `[000]` | `[0.0]` | Snippets, monospace |

### Letter Spacing
- Tight: `[-0.0]em`                  <!-- Titres larges -->
- Normal: `[0]em`                    <!-- Corps de texte -->
- Wide: `[0.0]em`                    <!-- Labels, caps -->

---

## Spacing

- Base unit: `[0]px`                 <!-- Unité de base de la grille (4 ou 8px) -->

| Token | Valeur | Usage typique |
|-------|--------|---------------|
| space-1 | `[0]px` | Micro — gap entre icône et label |
| space-2 | `[0]px` | XS — padding interne compact |
| space-3 | `[0]px` | SM — padding de composant |
| space-4 | `[0]px` | MD — gap entre éléments |
| space-5 | `[0]px` | LG — padding de section |
| space-6 | `[0]px` | XL — séparation entre blocs |
| space-8 | `[0]px` | 2XL — marges de page |
| space-10 | `[0]px` | 3XL — sections majeures |
| space-12 | `[0]px` | 4XL — espacement hero |

---

## Layout

### Grid
- Columns mobile: `[0]`
- Columns tablet: `[0]`
- Columns desktop: `[0]`
- Gutter: `[0]px`
- Margin mobile: `[0]px`
- Margin tablet: `[0]px`
- Margin desktop: `[0]px`

### Breakpoints
- Mobile: `0px – [000]px`
- Tablet: `[000]px – [000]px`
- Desktop: `[000]px+`

### Container
- Max width: `[0000]px`
- Content max width: `[000]px`       <!-- Pour le texte long -->

---

## Components

### Border Radius
- Radius None: `0px`
- Radius XS: `[0]px`                 <!-- Chips, tags -->
- Radius SM: `[0]px`                 <!-- Inputs, cartes compactes -->
- Radius MD: `[0]px`                 <!-- Boutons, cartes standard -->
- Radius LG: `[0]px`                 <!-- Modales, panels -->
- Radius XL: `[0]px`                 <!-- Cartes hero -->
- Radius Full: `9999px`              <!-- Boutons pill, avatars -->

### Shadows / Elevation
- Shadow None: `none`
- Shadow XS: `[0px 0px 0px 0px rgba(0,0,0,0.00)]`     <!-- Focus ring -->
- Shadow SM: `[0px 0px 0px 0px rgba(0,0,0,0.00)]`     <!-- Carte au repos -->
- Shadow MD: `[0px 0px 0px 0px rgba(0,0,0,0.00)]`     <!-- Carte hover -->
- Shadow LG: `[0px 0px 0px 0px rgba(0,0,0,0.00)]`     <!-- Dropdown -->
- Shadow XL: `[0px 0px 0px 0px rgba(0,0,0,0.00)]`     <!-- Modal -->

### Borders
- Border Width Default: `[0]px`
- Border Width Strong: `[0]px`
- Border Style: `solid`

### Buttons
- Height SM: `[0]px`
- Height MD: `[0]px`                 <!-- Taille par défaut -->
- Height LG: `[0]px`
- Padding horizontal SM: `[0]px`
- Padding horizontal MD: `[0]px`
- Padding horizontal LG: `[0]px`
- Min width: `[0]px`

### Inputs / Forms
- Height: `[0]px`
- Padding horizontal: `[0]px`
- Border radius: `→ Radius SM`
- Border color default: `→ Border`
- Border color focus: `→ Primary`
- Border color error: `→ Error`

### Icons
- Size SM: `[0]px`                   <!-- Inline dans du texte -->
- Size MD: `[0]px`                   <!-- Taille standard UI -->
- Size LG: `[0]px`                   <!-- Icône seule, illustrative -->

### Avatars
- Size XS: `[0]px`
- Size SM: `[0]px`
- Size MD: `[0]px`
- Size LG: `[0]px`

---

## Motion

- Duration Fast: `[000]ms`           <!-- Micro-interactions, toggles -->
- Duration Default: `[000]ms`        <!-- Transitions standard -->
- Duration Slow: `[000]ms`           <!-- Modales, entrées de page -->
- Easing Default: `cubic-bezier(0.0, 0.0, 0.0, 0.0)`
- Easing Enter: `cubic-bezier(0.0, 0.0, 0.0, 0.0)`    <!-- Entrée d'élément -->
- Easing Exit: `cubic-bezier(0.0, 0.0, 0.0, 0.0)`     <!-- Sortie d'élément -->

---

## Accessibility

- Focus ring color: `→ Primary`
- Focus ring width: `[0]px`
- Focus ring offset: `[0]px`
- Min touch target iOS: `44x44pt`    <!-- Ne pas modifier — RAAM 13.1 -->
- Min touch target Android: `48x48dp` <!-- Ne pas modifier — RAAM 13.1 -->
- Contrast ratio body text: `AA (4.5:1 minimum)`
- Contrast ratio large text: `AA (3:1 minimum)`

---

## Références

- Fichier Figma : `[URL du fichier Figma — page Design System]`
- Process & conventions : `../design.md`
- Tokens source (Figma Variables / Style Dictionary) : `[URL ou chemin]`
- Responsable : `Design System Manager`
