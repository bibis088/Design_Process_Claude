---
name: review-figma-scope
description: "Vérifie la conformité des écrans Figma et prototypes selon le scope fonctionnel — EPIC, MVP, critères d'acceptance. Exécuté par le product-owner via `use_figma`."
argument-hint: "[epic-slug]/[feature-slug]"
agent: product-owner
---

## Rôle
Auditer les frames Figma d'une feature pour vérifier qu'elles couvrent exactement le scope fonctionnel défini — ni plus, ni moins — et que les critères d'acceptance sont visuellement satisfaits.

## Agents consommateurs
Product Owner 

## Prérequis
- [ ] Frames Figma créées et peuplées de contenu
- [ ] Composants conformes aux guidelines (après `/check-guidelines-compliance`)
- [ ] User stories `US-###` et critères d'acceptance `CA-##` disponibles
- [ ] MCP Figma connecté

## Gestion des erreurs
> ❌ Frames Figma ou specs fonctionnelles manquantes — s'assurer que `/setup-figma-frames` et `/write-user-story` ont été exécutés.

## Processus de vérification

### Étape 1 — Lire le scope fonctionnel
Depuis `specs/$ARGUMENTS/EPIC.md` et `stories/`, extraire :
- Les objectifs produit du MVP
- La liste des US-### et leurs critères d'acceptance
- Ce qui est explicitement hors périmètre

### Étape 2 — Lire les frames Figma via `use_figma`
Lister toutes les frames de la page `📱 screens` et vérifier :
- Chaque US-### a ses frames correspondantes (Default + états)
- Les frames sont nommées selon la convention

### Étape 3 — Audit scope par US

Pour chaque `US-###` :

```markdown
## US-### — [Titre]

### Couverture des critères d'acceptance
| CA | Énoncé | Couvert dans Figma | Frame | Notes |
|----|--------|-------------------|-------|-------|
| CA-01 | [Énoncé] | ✅/❌ | S-01/iOS/Default | |
| CA-02 | [Énoncé] | ✅/❌ | | |

### Hors périmètre détecté
- [ ] [Fonctionnalité visible dans Figma mais hors scope] — à supprimer ou reporter en V2

### Manquants
- [ ] [CA non couvert visuellement] — frame ou état à créer
```

### Étape 4 — Vérifier la conformité MVP

```markdown
## Conformité MVP

| Objectif produit | Couvert | Evidence Figma | Notes |
|-----------------|---------|---------------|-------|
| [Objectif 1] | ✅/❌ | [Frame(s)] | |
| [Objectif 2] | ✅/❌ | [Frame(s)] | |

## Features hors périmètre détectées dans Figma
| Feature | Frame | Décision | Action |
|---------|-------|---------|--------|
| [Feature non prévue] | [Frame] | Reporter V2 / Garder | [Action] |
```

### Étape 5 — Vérifier le prototype de navigation

Via `use_figma`, vérifier que les liens de prototype couvrent les parcours principaux :
- Flux heureux complet navigable
- Retours arrière configurés
- États d'erreur accessibles

### Étape 6 — Émettre le verdict

```markdown
# Rapport Review Scope Figma — [FEAT-###] — [YYYY-MM-DD]

## Résultat global
[✅ Conforme au scope / ⚠️ Ajustements mineurs / ❌ Non conforme]

## US couvertes : [N/N]
## CA couverts : [N/N]

## Actions requises
| Priorité | Action | Responsable | Deadline |
|----------|--------|-------------|---------|
| Bloquant | [Action] | UI Designer | [Date] |
| Mineur | [Action] | UI Designer | V+1 |

## Verdict
- [ ] ✅ Approuvé — frames conformes au scope, handoff autorisé
- [ ] ❌ Refusé — [N] éléments bloquants à corriger
```

## Phase de validation — niveau approfondi

Avant de valider ce livrable et de passer à la suite, réponds à ces questions dans l'ordre :

1. Tous les critères d'acceptance de toutes les US sont-ils couverts visuellement dans Figma — aucun CA manquant ? (oui / non + CA non couverts)
2. Des fonctionnalités hors scope ont-elles été détectées dans les frames — à supprimer ou reporter ? (non / oui + liste)
3. Le prototype permet-il de naviguer le flux heureux de bout en bout sans blocage ? (oui / non + points de blocage)
4. Les objectifs MVP de l'EPIC sont-ils tous adressés par les frames disponibles ? (oui / non + objectifs non couverts)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ review-figma-scope "$ARGUMENTS" terminé
📋 Rapport : specs/$ARGUMENTS/figma-scope-review.md

Prochaines étapes :
→ /write-figma-handoff $ARGUMENTS (si approuvé)
→ Retour UI Designer si corrections requises
```
