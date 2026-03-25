---
name: setup-figma-grid
description: Définit les grilles et formats de frames dans Figma selon la stack iOS/Android — colonnes, gouttières, marges, safe areas. Exécuté par le design-system-manager via `use_figma`.
argument-hint: [epic-slug]
disable-model-invocation: false
context: fork
agent: design-system-manager
---

## Rôle
Créer les frames de base avec les grilles correctes pour iOS et Android, et les mettre à disposition dans le fichier Design System comme référence pour l'UI Designer.

## Agents consommateurs
- Design System Manager (pilote)
- UI Designer (consommateur — utilise ces frames comme base pour tous les écrans)

## Prérequis
- [ ] Fichier Figma Design System créé et tokens configurés
- [ ] Plateformes cibles confirmées (iOS / Android / Les deux)
- [ ] `rules/figma.md` consulté pour les valeurs de grille

## Gestion des erreurs

Si les prérequis ne sont pas remplis :
> ❌ Tokens Figma non configurés — lancer d'abord `/setup-figma-tokens $ARGUMENTS`.

Si MCP Figma n'est pas accessible :
> ⚠️ MCP Figma indisponible — produire les spécifications de grille à appliquer manuellement.

## Processus de génération

### Étape 1 — Créer les frames de base iOS

Via `use_figma`, sur la page `🎨 Foundations` du Design System, créer :

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

Sur la page `📖 Documentation`, créer une frame "Grilles & Formats" montrant :
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
```
