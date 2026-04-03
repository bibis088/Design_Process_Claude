---
name: write-handoff
description: "Prépare le handoff Dev complet — connexion Code Connect composants vers SwiftUI/Compose et package handoff Figma. Use when user says 'handoff', 'handoff dev', 'code connect', 'prépare le handoff', or 'write-handoff'."
argument-hint: "[feature-slug]"
agent: ui-designer
---

## Rôle
Préparer le handoff Dev en deux étapes : Code Connect puis package Figma.


## Prérequis
- [ ] Documentation UX/UI exécuté sur tous les composants — 5 specs (Anatomy · API · Color · Structure · Screen Reader) rendues dans Figma

- [ ] MCP Figma connecté · frames `Ready for dev`
- [ ] Gate 10 — `US-###` Approved · frames validées
- [ ] `US-###` et CA disponibles pour les annotations
- [ ] `specs/[epic-slug]/figma-urls.md` disponible
- [ ] `FLUX-###` disponibles — handoff organisé par flux

## Étape 1 — Code Connect (composants → SwiftUI / Jetpack Compose)
### Étape 1 — Lire les mappings existants
```
Appeler : get_code_connect_map
→ Retourne : mapping actuel node ID Figma → composant code
→ Identifier : quels composants sont déjà connectés
```

### Étape 2 — Obtenir les suggestions automatiques
```
Appeler : get_code_connect_suggestions
→ Figma analyse les noms et structures des composants
→ Retourne : suggestions de mappings Figma → code
→ Vérifier chaque suggestion avant de confirmer
```

### Étape 3 — Présenter les mappings pour validation

```markdown
## Mappings Code Connect suggérés — [feature-slug]

| Composant Figma | Node ID | Implémentation iOS | Implémentation Android | Confiance |
|----------------|---------|-------------------|----------------------|-----------|
| button/primary/default | [ID] | `PrimaryButton.swift` | `PrimaryButton.kt` | Haute |
| input/text/default | [ID] | `TextInput.swift` | `TextInput.kt` | Haute |
| Card / Product | [ID] | `ProductCard.swift` | `ProductCard.kt` | Moyenne |

Confirmes-tu ces mappings ?
```

### Étape 4 — Confirmer et créer les mappings
```
Appeler : send_code_connect_mappings
→ Confirme les mappings validés
→ Code Connect est établi dans Figma

Ou pour chaque mapping individuel :
Appeler : add_code_connect_map
→ node_id : [ID du composant Figma]
→ code_connect_src : [chemin vers le fichier code]
→ code_connect_name : [nom du composant dans le code]
```

### Étape 5 — Vérifier le résultat dans Dev Mode

Via `get_code_connect_map` :
```
→ Vérifier que tous les composants de la feature ont un mapping
→ Documenter les composants sans mapping (à connecter manuellement)
```

### Étape 6 — Produire le rapport de connexion

```markdown
# Rapport Code Connect — [feature-slug] — [YYYY-MM-DD]

## Composants connectés
| Composant Figma | iOS | Android | Statut |
|----------------|-----|---------|--------|
| Button / Primary | `PrimaryButton.swift` | `PrimaryButton.kt` | ✅ |
| Input / Text | `TextInput.swift` | `TextInput.kt` | ✅ |

## Composants non connectés
| Composant Figma | Raison | Action |
|----------------|--------|--------|
| [Nom] | Implémentation absente | Créer `/write-component-ui` |

## Impact sur get_design_context
Avec Code Connect actif, `get_design_context` retournera les composants réels :
- iOS : `clientFrameworks: "SwiftUI"` → retourne les vrais `PrimaryButton`, etc.
- Android : `clientFrameworks: "Jetpack Compose"` → retourne les vrais composants
```

## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. Tous les composants publiés dans la bibliothèque DS ont-ils un mapping Code Connect iOS ET Android ? (oui / non + composants non connectés)
2. Les suggestions automatiques de Figma ont-elles été vérifiées manuellement avant confirmation — aucun mapping incorrect ? (oui / non)
3. `get_design_context` retourne-t-il maintenant les composants réels plutôt que du code générique ? (oui / non — tester sur un composant connecté)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ figma-code-connect "$ARGUMENTS" terminé
📋 Rapport : design/$ARGUMENTS/code-connect-report.md

Prochaines étapes :
→ /write-figma-handoff $ARGUMENTS (Code Connect actif avant handoff)
→ Les Dev peuvent naviguer de Figma Dev Mode → code source
→ get_design_context retourne maintenant les vrais composants

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```

---

## Étape 2 — Package Handoff Figma
### Étape 1 — Nettoyer et organiser les frames

Via `use_figma` :
1. Déplacer toutes les frames `Ready for dev` vers la page `Ready for dev`
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

Sur la page `Ready for dev`, créer une frame "UI Kit — [NomFeature]" listant :
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
- Page Handoff : [URL directe vers la page Ready for dev]
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
🚀 Figma Projet → page Ready for dev prête
📋 Document : design/$ARGUMENTS/handoff.md

Prochaines étapes :
→ /write-store-assets $ARGUMENTS (si publication store prévue)
→ Transmettre le lien Figma et le document handoff aux Dev iOS et Android
→ /write-qa-report [epic-slug]/[us-slug] (QA depuis les frames handoff)

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```

---

## Résumé de fin d'exécution

```
✅ write-handoff "$ARGUMENTS" terminé
🎨 Figma → page Ready for dev complète
📋 Code Connect : composants liés

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]

Prochaines étapes :
→ Gate 10 — Product Designer valide le handoff
→ /write-store-assets [epic-slug]
```
