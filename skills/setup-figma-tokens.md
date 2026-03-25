---
name: setup-figma-tokens
description: Met en place la structure de tokens sémantiques dans Figma Design System — couleurs, typographies, styles de texte, spacing, ombres, motion. Exécuté par le design-system-manager via `use_figma`.
argument-hint: [epic-slug]
disable-model-invocation: false
context: fork
agent: design-system-manager
---

## Rôle
Initialiser et structurer les tokens de design dans le fichier Figma Design System — depuis les tokens documentés dans `design-system/tokens/` vers les styles et variables Figma.

## Agents consommateurs
- Design System Manager (pilote)
- UI Designer (consommateur — applique les tokens dans les composants et frames)

## Prérequis
- [ ] Fichier Figma Design System créé via `/setup-figma-project $ARGUMENTS`
- [ ] Tokens documentés dans `design-system/tokens/` (ou à créer si nouveau projet)
- [ ] `rules/figma.md` consulté pour les grilles et conventions
- [ ] `/figma-read-design` exécuté — structure et tokens existants identifiés
- [ ] MCP Figma connecté

## Gestion des erreurs

Si les prérequis ne sont pas remplis :
> ❌ Fichier Design System Figma manquant — lancer d'abord `/setup-figma-project $ARGUMENTS`.

Si MCP Figma n'est pas accessible :
> ⚠️ MCP Figma indisponible — produire le document de configuration des tokens à appliquer manuellement.

## Processus de génération

### Étape 1 — Lire les tokens existants
Depuis `design-system/tokens/`, extraire :
- `colors.md` — palette primitive + tokens sémantiques light/dark
- `typography.md` — échelle typographique iOS/Android
- `spacing.md` — grille d'espacements
- `shadows.md` — élévations
- `motion.md` — durées et courbes
- `radius.md` — rayons de bordure

Si ces fichiers n'existent pas → créer les tokens de base avant de continuer (voir `/write-token`).

### Étape 2 — Configurer les couleurs dans Figma

Via `use_figma`, sur la page `🎨 Foundations` :

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

Via `use_figma`, créer les styles de texte sur la page `🎨 Foundations` :

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

### Étape 6 — Créer les swatches de documentation

Sur la page `🎨 Foundations`, créer une frame de documentation visuelle montrant :
- Tous les tokens de couleur avec leur valeur light et dark
- L'échelle typographique avec un exemple de texte pour chaque style
- L'échelle d'espacement avec des rectangles de taille correspondante

## Phase de validation — niveau approfondi

Avant de valider ce livrable et de passer à la suite, réponds à ces questions dans l'ordre :

1. Toutes les variables de couleur sémantiques ont-elles un mode Light ET un mode Dark configurés ? (oui / non + tokens manquants)
2. Les styles de texte iOS et Android sont-ils distincts et correctement nommés `iOS/[nom]` et `Android/[nom]` ? (oui / non)
3. Les variables d'espacement sont-elles basées sur une unité de 4pt/dp — toutes les valeurs sont-elles multiples de 4 ? (oui / non + valeurs incorrectes)
4. Les swatches de documentation sont-ils lisibles et couvrent-ils tous les tokens créés ? (oui / non)
5. Les tokens Figma correspondent-ils exactement aux tokens documentés dans `design-system/tokens/` — aucune divergence ? (oui / non + divergences)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ setup-figma-tokens "$ARGUMENTS" terminé
📁 design-system/tokens/ (référence)
🎨 Figma Design System → page 🎨 Foundations mise à jour

Prochaines étapes :
→ /setup-figma-grid $ARGUMENTS
→ /write-figma-userflow $ARGUMENTS (après grilles)
```
