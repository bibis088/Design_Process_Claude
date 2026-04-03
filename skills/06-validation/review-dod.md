---
name: review-dod
description: "Vérifie la DoD d'un livrable avant passage au statut suivant. Applicable à tous les agents et types de livrables."
argument-hint: "[type-livrable]/[id-livrable]"
agent: product-owner
---

## Rôle
Vérifier qu'une Definition of Done est complète avant de passer un livrable au statut suivant.


## Prérequis
- [ ] Livrables de la phase en cours disponibles
- [ ] `US-###` et CA de la feature disponibles
- [ ] `FLUX-###` du scope disponibles — vérification couverture
- [ ] Livrables de la gate en cours disponibles
- [ ] Gate ciblée explicitement précisée dans l'argument

## Agents consommateurs
Tous les agents  · Product Owner  · Tech Lead 

## Quand utiliser ce skill
- Avant de changer le statut d'un livrable de `In Progress` → `In Review`
- Avant de valider un livrable de `In Review` → `Approved`
- Avant de transmettre un livrable à l'agent aval

## Processus de vérification

### Étape 1 — Identifier le type de livrable

| Type de livrable | DoD de référence | Agent responsable |
|-----------------|-----------------|------------------|
| Document BA (Brief, Flux, Règles, Glossaire) | `agents/business-analyst.md` — DoD BA | BA |
| User story (`US-###`) | `agents/product-owner.md` — DoD PO | PO |
| Spec UX (écran, composant, user flow, navigation map) | `agents/ux-designer.md` — DoD UX | UX Designer |
| Spec accessibilité RAAM | `rules/accessibility.md` — checklist A + AA | UX Designer |
| Annotations accessibilité (`S-XX-annotations.md`) | `skills/write-accessibility-annotations.md` | UX Designer |
| Composant UI (code) | `agents/ui-designer.md` — DoD composant | UI Designer |
| Écran UI (code) | `agents/ui-designer.md` — DoD écran | UI Designer |
| Token design system | `agents/design-system-manager.md` — DoD token | Design System Manager |
| Composant design system | `agents/design-system-manager.md` — DoD composant | Design System Manager |
| Guideline design system | `agents/design-system-manager.md` — DoD guideline | Design System Manager |
| Breaking change design system | `agents/design-system-manager.md` — DoD breaking change | Design System Manager |
| Rapport QA (`QA-###`) | `agents/qa-engineer.md` — DoD QA | QA Engineer |

### Critères transversaux — vérifiés sur tous les livrables

En plus de la DoD spécifique au type de livrable, vérifier systématiquement :

| Critère transversal | Applicable à | Vérification |
|--------------------|-------------|-------------|
| URL Figma renseignée | US-###, S-XX-[nom].md | Section "Références Figma" présente et URL valide |
| Frame Figma référence le doc | S-XX-[nom].md | Description Figma contient `S-XX \| US-### \| FLUX-###` |
| Conformité RAAM niveau A | US-###, S-XX-[nom].md, QA-### | Critères A cochés ou documentés comme N/A avec justification |
| Statut cohérent | Tous | Statut du doc = statut attendu selon la gate de validation |

### Étape 2 — Appliquer la DoD correspondante

Générer le rapport de vérification dans ce format :

```markdown
## Vérification DoD — [Type de livrable] — [ID ou nom]

### Résultat global
[✅ Prêt pour review / ⚠️ Blocages mineurs / ❌ Bloqué]

### Socle non négociable
| Critère | Statut | Note |
|---------|--------|------|
| [Critère 1] | ✅ / ❌ / ⚠️ | [Observation si nécessaire] |
| [Critère 2] | ✅ / ❌ / ⚠️ | [Observation si nécessaire] |

### Exigences additionnelles cochées
| Critère | Statut | Note |
|---------|--------|------|
| [Critère additionnel applicable] | ✅ / ❌ | [Observation] |

### Points bloquants (❌)
- [Description du point bloquant — action requise avant de continuer]

### Points à surveiller (⚠️)
- [Observation non bloquante — à adresser en review]

### Recommandation
[Action à prendre : soumettre en review / corriger les points bloquants / escalader au PO]
```

### Étape 3 — Décision

| Résultat | Action |
|----------|--------|
| ✅ Tous les critères cochés | Passer au statut suivant |
| ⚠️ Blocages mineurs | Soumettre avec note — points à adresser en review |
| ❌ Critères bloquants | Ne pas soumettre — corriger d'abord |


## Gestion des erreurs
> ❌ Prérequis manquant — vérifier les fichiers sources avant de relancer.

## Règles de qualité

- Un critère non applicable est marqué `N/A` — jamais laissé vide
- Un critère ❌ bloque toujours la soumission — aucune exception
- Le rapport de vérification est joint au livrable soumis en review
- Ce skill ne remplace pas la validation humaine (PO, Tech Lead) — il la prépare


## Phase de validation — niveau approfondi

Avant de valider ce livrable et de passer à la suite, réponds à ces questions dans l'ordre :

1. Tous les critères bloquants (❌) ont-ils un responsable et une deadline assignés ? (oui / non + critères à compléter)
2. Les critères non applicables (⚠️) ont-ils tous une justification écrite ? (oui / non)
3. Le statut cible est-il atteignable avec les critères actuellement cochés ? (oui / réviser le statut cible)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ review-dod "$ARGUMENTS" terminé
📁 — (rapport affiché en sortie, pas de fichier généré)

Prochaines étapes :
→ /Statut mis à jour sur le livrable concerné

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```
