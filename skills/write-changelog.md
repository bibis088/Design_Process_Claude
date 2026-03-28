---
name: write-changelog
description: "Rédige une entrée de changelog MAJOR.MINOR.PATCH pour le design system. Exécuté par le design-system-manager."
argument-hint: "[version] [type: MAJOR|MINOR|PATCH]"
disable-model-invocation: false
context: fork
agent: design-system-manager
---

## Rôle
Rédiger une entrée de changelog selon la convention MAJOR.MINOR.PATCH — pour le design system et les specs.

## Agents consommateurs
- Design System Manager (pilote — pour le design system)
- Business Analyst (contributeur — pour les specs)
- Product Owner (contributeur — pour les specs)

## Convention de versioning

```
MAJOR.MINOR.PATCH
```

| Type | Quand | Exemples |
|------|-------|---------|
| `MAJOR` | Breaking change — renommage, suppression, API modifiée | Token renommé, composant supprimé |
| `MINOR` | Ajout rétrocompatible | Nouveau token, nouveau composant, nouvelle guideline |
| `PATCH` | Correction sans impact | Valeur corrigée, doc mise à jour, typo |

## Processus de génération

### Étape 1 — Identifier le type de changement
Avant d'écrire l'entrée, répondre à ces questions :
- Est-ce que ce changement casse quelque chose d'existant ? → MAJOR
- Est-ce que ce changement ajoute quelque chose de nouveau ? → MINOR
- Est-ce que ce changement corrige quelque chose sans rien ajouter ? → PATCH

### Étape 2 — Rédiger l'entrée

```markdown
## [X.Y.Z] — [YYYY-MM-DD]

### ⚠️ BREAKING CHANGE (si MAJOR uniquement)
[Description courte du breaking change en 1 phrase]

### Ajouté
- [Ce qui a été ajouté — token, composant, guideline, règle]
- [Un item par ligne]

### Modifié
- [Ce qui a changé — avec ancien nom → nouveau nom si renommage]

### Corrigé
- [Ce qui a été corrigé]

### Déprécié
- `[nom]` — sera supprimé en [X+1].0.0 — utiliser `[remplaçant]` à la place

### Supprimé
- `[nom]` — déprécié depuis [X-1].0.0 — remplacé par `[remplaçant]`

### Migration (si MAJOR uniquement)
**Fichiers impactés** : [liste des fichiers ou composants concernés]

**Étapes de migration** :
1. [Action à effectuer]
2. [Action à effectuer]

**Agents à notifier** : [UI Designer / Dev iOS / Dev Android / PO]
```

### Étape 3 — Notifier les agents consommateurs

Pour tout changement MAJOR, notifier explicitement :
- UI Designer — impact sur les composants visuels
- Dev iOS — impact sur l'implémentation SwiftUI
- Dev Android — impact sur l'implémentation Compose
- PO — si impact sur les specs fonctionnelles


## Gestion des erreurs

Si les prérequis ne sont pas remplis :
> ❌ Prérequis manquant — vérifier les fichiers sources avant de relancer.

Si un fichier source est introuvable via chemin relatif, utiliser Glob avec le pattern `**/design-system/$ARGUMENTS/**` pour localiser les fichiers depuis leur chemin absolu.

## Règles de qualité

- Une entrée de changelog est **obligatoire** pour tout changement au design system
- Les sections vides sont **omises** — ne pas inclure `### Ajouté` si rien n'a été ajouté
- Les dépréciations anticipent la suppression d'au moins une version MAJOR
- La migration est **obligatoire** pour tout MAJOR — jamais de breaking change sans guide

## Chemin de fichier
```
design-system/CHANGELOG.md
```


## Phase de validation — niveau léger

Avant de clore ce skill, confirme ce point :

1. Le type de changement est-il correct — MAJOR / MINOR / PATCH ? (confirme ou corrige)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-changelog "$ARGUMENTS" terminé
📁 design-system/CHANGELOG.md

Prochaines étapes :
→ /review-dod $ARGUMENTS (breaking change uniquement)
```
