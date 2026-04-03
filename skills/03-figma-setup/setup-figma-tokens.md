---
name: setup-figma-tokens
description: "Met en place la structure de tokens sémantiques dans Figma Design System — couleurs, typographies, styles de texte, spacing, ombres, motion. Exécuté par le design-system-manager via `use_figma`."
argument-hint: "[epic-slug]"
agent: design-system-manager
---

## Rôle
Initialiser et structurer les tokens de design dans le fichier Figma Design System — depuis les tokens documentés dans `design-system/tokens/` vers les styles et variables Figma.

## Agents consommateurs
Design System Manager  · UI Designer 

## Prérequis
- [ ] Fichier Figma Design System créé via `/setup-figma-project $ARGUMENTS`
- [ ] `design-system/design-tokens.md` disponible (valeurs remplies ou direction stylistique définie)
- [ ] `rules/figma.md` consulté pour les grilles et conventions
- [ ] `/figma-read-design` exécuté — structure et tokens existants identifiés
- [ ] MCP Figma connecté
- [ ] `cadrage.md` disponible — direction stylistique et OS de base définis
- [ ] Fichier Figma DS créé et ouvert (`setup-figma-project` exécuté)
- [ ] Direction stylistique définie (étape 0 du skill — type produit → palette recommandée)

## Gestion des erreurs
> ❌ Fichier Design System Figma manquant — lancer d'abord `/setup-figma-project $ARGUMENTS`.
> ⚠️ MCP Figma indisponible — produire le document de configuration des tokens à appliquer manuellement.

## Processus de génération

### Étape 0 — Générer la direction stylistique (si nouveau projet)

Si aucun token n'existe encore, recommander une direction stylistique avant de créer quoi que ce soit.

Depuis `specs/[epic-slug]/cadrage.md` et `specs/[epic-slug]/research/competitive-analysis.md`, extraire :
- Le type de produit (wellness, fintech, e-commerce, SaaS, healthcare...)
- Le secteur d'activité
- La cible utilisateur (B2C grand public, B2B, expert...)
- Le ton souhaité (premium, accessible, technique, ludique...)

Produire la recommandation :

```markdown
## Recommandation design system — [NomProjet]

### Type de produit détecté
[Type] — [Secteur]

### Direction stylistique recommandée
Style : [ex: Clean Minimal / Material Expressive / Premium Dark]
Justification : [Pourquoi ce style convient à ce produit et cette cible]

### Palette recommandée
Couleur primaire : [Couleur + justification — ex: Bleu confiance pour une app fintech]
Couleur secondaire : [Couleur]
Couleur d'accent : [Couleur]
Approche dark mode : [Tonale / Inversée / Adaptée]

### Typographie recommandée
iOS : [Style Figma — ex: ios/type/* basé sur SF Pro]
Android : [Style Figma — ex: android/type/* basé sur Roboto / Google Sans]
Hiérarchie : [Bold titres, Regular corps, Medium labels]

### Tokens d'ambiance
Radius : [Arrondi généreux / Angles nets / Intermédiaire] → [valeur recommandée]
Ombres : [Plates / Douces / Prononcées]
Motion : [Rapide (150ms) / Standard (250ms) / Fluide (300ms)]

### Références visuelles du secteur
[3 patterns UX standards identifiés dans le benchmark]

⚠️ Validation Product Designer requise avant de créer les tokens
"Cette direction stylistique est-elle alignée avec la vision du projet ?"
```

### Étape 1 — Lire les tokens existants
Depuis `design-system/tokens/`, extraire :
- `colors.md` — palette primitive + tokens sémantiques light/dark
- `typography.md` — échelle typographique iOS/Android
- `spacing.md` — grille d'espacements
- `shadows.md` — élévations
- `motion.md` — durées et courbes
- `radius.md` — rayons de bordure

Si ces fichiers n'existent pas → créer les tokens de base depuis la recommandation de l'étape 0 (voir `/write-token`).

### Étape 2 — Configurer les couleurs dans Figma

Via `use_figma`, sur la page `🎨 foundations` :

**Palette primitive** (variables Figma — collection `Primitives`) :
```
blue/50, blue/100, ... blue/900
gray/50, gray/100, ... gray/950
red/50, ... red/900
[etc. pour chaque ramp]
```

**Tokens sémantiques** (variables Figma — collection `Semantic`) :
```
Mode Light :
  color/content/primary → gray/900
  color/content/secondary → gray/600
  color/background/primary → white
  color/interactive/primary → blue/500
  [etc.]

Mode Dark :
  color/content/primary → gray/100
  color/content/secondary → gray/400
  color/background/primary → gray/950
  color/interactive/primary → blue/500
  [etc.]
```

### Étape 3 — Configurer la typographie dans Figma

Via `use_figma`, créer les styles de texte sur la page `🎨 foundations` :

| Style Figma | iOS | Android | Poids | Line height |
|------------|-----|---------|-------|-------------|
| `type/display` | 34pt | 34sp | Bold | 41 |
| `type/title1` | 28pt | 28sp | Bold | 34 |
| `type/title2` | 22pt | 22sp | SemiBold | 28 |
| `type/headline` | 17pt | 16sp | SemiBold | 22 |
| `type/body` | 17pt | 16sp | Regular | 22 |
| `type/callout` | 16pt | 14sp | Regular | 21 |
| `type/caption` | 12pt | 12sp | Regular | 16 |

Créer deux jeux de styles : `iOS/[nom]` et `Android/[nom]`.

### Étape 4 — Configurer l'espacement dans Figma

Via `use_figma`, créer les variables d'espacement (collection `Spacing`) :
```
spacing/xs → 4
spacing/sm → 8
spacing/md → 16
spacing/lg → 24
spacing/xl → 32
spacing/xxl → 48
```

### Étape 5 — Configurer les ombres et le radius

Via `use_figma`, créer les effets et variables :
- Ombres : styles d'effet `shadow/card`, `shadow/modal`, `shadow/overlay`
- Radius : variables `radius/sm → 4`, `radius/md → 8`, `radius/lg → 12`, `radius/xl → 16`

### Étape 6 — Configurer les modes de variables light / dark

Via `use_figma`, sur la collection de variables sémantiques :

```
Collection : "Semantic"
Mode 1 : light
  color/interactive/primary    → [valeur hex light]
  color/content/primary        → [valeur hex light]
  color/surface/default        → [valeur hex light]
  color/feedback/error         → [valeur hex light]
  [... tous les tokens sémantiques]

Mode 2 : dark
  color/interactive/primary    → [valeur hex dark — désaturée/plus claire]
  color/content/primary        → [valeur hex dark]
  color/surface/default        → [valeur hex dark]
  color/feedback/error         → [valeur hex dark]
  [... tous les tokens sémantiques]
```

Règles dark mode :
- Jamais d'inversion des couleurs light — utiliser des variantes désaturées
- Surface dark : fond sombre (ex: `#1C1C1E` iOS / `#121212` Android)
- Tester le contraste 4.5:1 en dark mode indépendamment du light

### Étape 7 — Créer les Text Styles partagés

Les Text Styles sont distincts des tokens de typographie — ce sont des objets Figma qui permettent l'annotation et le `Ctrl+T`. Via `use_figma` :

```
iOS Text Styles (dans le fichier Design System) :
ios/large_title    → SF Pro Display, 34pt, Regular
ios/title_1        → SF Pro Display, 28pt, Regular
ios/title_2        → SF Pro Display, 22pt, Regular
ios/title_3        → SF Pro Display, 20pt, Semibold
ios/headline       → SF Pro Text, 17pt, Semibold
ios/body           → SF Pro Text, 17pt, Regular
ios/callout        → SF Pro Text, 16pt, Regular
ios/subheadline    → SF Pro Text, 15pt, Regular
ios/footnote       → SF Pro Text, 13pt, Regular
ios/caption_1      → SF Pro Text, 12pt, Regular
ios/caption_2      → SF Pro Text, 11pt, Regular

Android Text Styles :
android/display_large   → Roboto, 57sp, Regular
android/display_medium  → Roboto, 45sp, Regular
android/headline_large  → Roboto, 32sp, Regular
android/headline_medium → Roboto, 28sp, Regular
android/title_large     → Roboto, 22sp, Regular
android/title_medium    → Roboto, 16sp, Medium
android/title_small     → Roboto, 14sp, Medium
android/body_large      → Roboto, 16sp, Regular
android/body_medium     → Roboto, 14sp, Regular
android/label_large     → Roboto, 14sp, Medium
android/label_medium    → Roboto, 12sp, Medium
android/label_small     → Roboto, 11sp, Medium
```

### Étape 8 — Créer les swatches de documentation

Sur la page `🎨 foundations`, créer une frame de documentation visuelle montrant :
- Tous les tokens de couleur avec valeur light ET dark côte à côte
- L'échelle typographique avec un exemple de texte pour chaque style
- L'échelle d'espacement avec des rectangles de taille correspondante
- Un exemple de composant en light et en dark mode

## Phase de validation — niveau approfondi

Avant de valider ce livrable et de passer à la suite, réponds à ces questions dans l'ordre :

1. Toutes les variables de couleur sémantiques ont-elles un mode `light` ET un mode `dark` configurés avec des valeurs distinctes ? (oui / non + tokens manquants)
2. Le contraste 4.5:1 est-il respecté en dark mode indépendamment — pas uniquement testé en light ? (oui / non)
3. Les Text Styles iOS et Android sont-ils créés et nommés `ios/[nom]` et `android/[nom]` — distincts des tokens de variables ? (oui / non)
4. Les variables d'espacement sont-elles basées sur une unité de 4pt/dp — toutes multiples de 4 ? (oui / non + valeurs incorrectes)
5. Les tokens Figma correspondent-ils exactement aux tokens documentés dans `design-system/tokens/` — aucune divergence ? (oui / non + divergences)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Étape 9 — Grilles iOS et Android (depuis setup-figma-grid)

### Étape 1 — Créer les frames de base iOS

Via `use_figma`, sur la page `🎨 foundations` du Design System, créer :

**iPhone SE / iPhone 13 mini (375 × 812pt)**
- Grille : 4 colonnes, gouttière 16pt, marge 16pt
- Safe area top : 44pt (status bar + notch)
- Safe area bottom : 34pt (home indicator)

**iPhone 14 Pro / 15 (393 × 852pt)**
- Grille : 4 colonnes, gouttière 16pt, marge 16pt
- Safe area top : 59pt (Dynamic Island)
- Safe area bottom : 34pt

**iPhone 14 Pro Max / 15 Plus (430 × 932pt)**
- Grille : 4 colonnes, gouttière 16pt, marge 20pt
- Safe area top : 59pt
- Safe area bottom : 34pt

**iPad (768 × 1024pt)**
- Grille : 8 colonnes, gouttière 24pt, marge 24pt

### Étape 2 — Créer les frames de base Android

Via `use_figma` :

**Android Mobile standard (360 × 800dp)**
- Grille : 4 colonnes, gouttière 16dp, marge 16dp
- Status bar : 24dp
- Navigation bar : 48dp (3-button) / 0dp (gesture)

**Android Mobile Large (412 × 915dp)**
- Grille : 4 colonnes, gouttière 16dp, marge 16dp

**Android Tablet (600 × 1024dp)**
- Grille : 8 colonnes, gouttière 24dp, marge 24dp

### Étape 3 — Créer les composants de frame réutilisables

Via `use_figma`, créer des composants Figma pour les éléments structurels communs :

**iOS :**
```
Frame Base / iPhone 375 / Portrait
Frame Base / iPhone 393 / Portrait
Frame Base / iPhone 430 / Portrait
Frame Base / iPad 768 / Portrait
```

**Android :**
```
Frame Base / Android 360 / Portrait
Frame Base / Android 412 / Portrait
Frame Base / Android Tablet 600 / Portrait
```

Chaque frame base inclut :
- Grille configurée et visible
- Safe areas marquées en overlay (couleur semi-transparente)
- Status bar placeholder
- Navigation bar placeholder

### Étape 4 — Documenter les grilles

Sur la page `📖 documentation`, créer une frame "Grilles & Formats" montrant :
- Toutes les frames de base côte à côte
- Les dimensions annotées
- Les safe areas expliquées

## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. Les frames de base iOS couvrent-elles tous les formats requis — iPhone 375, 393, 430 et iPad 768 ? (oui / non + formats manquants)
2. Les safe areas sont-elles correctement définies sur les frames iPhone avec Dynamic Island ? (oui / non)
3. Les grilles Android respectent-elles la base unit de 8dp — toutes les valeurs sont-elles multiples de 8 ? (oui / non + valeurs incorrectes)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ setup-figma-grid "$ARGUMENTS" terminé
🎨 Figma Design System → frames de base disponibles

Prochaines étapes :
→ /write-figma-userflow $ARGUMENTS (UX Designer)
→ /setup-figma-frames $ARGUMENTS (UI Designer)
→ Les frames de base sont prêtes à être dupliquées pour chaque écran

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```

---

## Résumé de fin d'exécution

```
✅ setup-figma-tokens "$ARGUMENTS" terminé
📁 design-system/tokens/ (référence)
🎨 Figma Design System → page 🎨 foundations mise à jour
  - Variables : light + dark modes configurés
  - Text Styles : iOS + Android créés

Prochaines étapes :
→ /setup-figma-library $ARGUMENTS (connexion DS → fichier Projet)

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```
