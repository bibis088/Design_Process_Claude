---
name: write-figma-handoff
description: Prépare le UI kit et le fichier Figma pour le handoff Dev — nommage, inspection, liens specs, annotations. Exécuté par le ui-designer via `use_figma`.
argument-hint: [feature-slug]
disable-model-invocation: false
context: fork
agent: ui-designer
---

## Rôle
Préparer le fichier Figma pour la transmission aux développeurs — frames organisées, layers nommés, tokens référencés, annotations complètes, prototype navigable.

## Agents consommateurs
- UI Designer (pilote)
- Dev iOS / Android (destinataires finaux)

## Prérequis
- [ ] Review scope PO validée (`review-figma-scope` en statut Approuvé)
- [ ] Conformité guidelines vérifiée (`check-guidelines-compliance` sans non-conformité bloquante)
- [ ] Annotations RAAM complètes (`write-accessibility-annotations` terminé)
- [ ] `/figma-read-design` exécuté — structure et tokens existants identifiés
- [ ] MCP Figma connecté

## Gestion des erreurs

Si les prérequis ne sont pas remplis :
> ❌ Validation PO ou conformité guidelines manquante — compléter ces étapes avant le handoff.

## Processus de génération

### Étape 1 — Nettoyer et organiser les frames

Via `use_figma` :
1. Déplacer toutes les frames `Ready for dev` vers la page `🚀 Handoff`
2. Supprimer les frames de travail (brouillons, explorations, variantes rejetées)
3. Vérifier que toutes les frames sont nommées selon la convention
4. Vérifier qu'aucun layer n'est nommé `Rectangle 1`, `Group 4`, etc.

### Étape 2 — Vérifier les tokens sur toutes les frames

Via `use_figma`, auditer que :
- Aucune couleur n'est définie en dur (hex) — uniquement tokens `color/*`
- Aucune taille de police n'est définie en dur — uniquement styles `iOS/*` ou `Android/*`
- Aucun espacement en dur — uniquement tokens `spacing/*`

### Étape 3 — Compléter les descriptions de frames

Via `use_figma`, dans la description de chaque frame `Ready for dev` :
```
Spec : S-XX-[nom] | US-###, US-### | FLUX-###
Statut : Ready for dev
Annotations RAAM : Done
Grille : iOS 393pt / 4 colonnes — Android 412dp / 4 colonnes
Tokens DS : v[X.X.X]
Dev notes : [notes spécifiques à l'implémentation si applicable]
```

### Étape 4 — Configurer le prototype final

Via `use_figma` :
1. Vérifier que tous les liens de prototype des flux principaux sont actifs
2. Configurer le point de départ du prototype sur l'écran d'entrée de la feature
3. Ajouter les transitions de prototype correspondant aux specs UX

### Étape 5 — Créer le UI Kit de la feature

Sur la page `🚀 Handoff`, créer une frame "UI Kit — [NomFeature]" listant :
- Tous les composants utilisés dans la feature (avec liens vers les composants DS)
- La palette de couleurs utilisée (tokens sémantiques)
- L'échelle typographique utilisée
- Les espacements appliqués
- Les radius utilisés

### Étape 6 — Générer le document de handoff

```markdown
# Handoff Dev — [FEAT-###] [NomFeature] — [YYYY-MM-DD]

## Liens Figma
- Fichier Projet : [URL]
- Page Handoff : [URL directe vers la page 🚀 Handoff]
- Prototype : [URL du prototype]
- Design System : [URL fichier DS]

## Frames livrées
| Frame | US | iOS | Android | Statut |
|-------|----|----|---------|--------|
| S-01 Login | US-042 | ✅ | ✅ | Ready for dev |

## Composants utilisés
| Composant | Source | Version DS |
|-----------|--------|-----------|
| Button / Primary | DS Bibliothèque | v1.2.0 |

## Tokens design system
- Version DS utilisée : v[X.X.X]
- Changelog depuis dernière version : [lien]

## Notes d'implémentation
- [Comportement spécifique à noter pour le Dev]
- [Geste ou animation à implémenter]
- [Donnée dynamique à brancher sur l'API]

## Assets exportables
| Asset | Format | Usage |
|-------|--------|-------|
| [Icône X] | SVG | Navigation |
| [Illustration Y] | PNG @2x @3x | Écran vide |
```

## Phase de validation — niveau approfondi

Avant de valider ce livrable et de passer à la suite, réponds à ces questions dans l'ordre :

1. Tous les layers sont-ils nommés correctement — aucun nom auto Figma (`Rectangle 1`, `Group 4`) ? (oui / non + frames concernées)
2. Les descriptions de toutes les frames `Ready for dev` contiennent-elles la référence spec `S-XX | US-###` ? (oui / non + frames à compléter)
3. Le prototype est-il navigable de bout en bout pour le flux heureux principal ? (oui / non + points de blocage)
4. Le document de handoff liste-t-il tous les composants utilisés avec leur version DS ? (oui / non)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-figma-handoff "$ARGUMENTS" terminé
🚀 Figma Projet → page 🚀 Handoff prête
📋 Document : design/$ARGUMENTS/handoff.md

Prochaines étapes :
→ /write-store-assets $ARGUMENTS (si publication store prévue)
→ Transmettre le lien Figma et le document handoff aux Dev iOS et Android
→ /write-qa-report [epic-slug]/[us-slug] (QA depuis les frames handoff)
```
