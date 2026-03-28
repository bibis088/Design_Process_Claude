---
name: review-tracking-plan
description: "Vérifie que chaque événement du plan de tagage a un élément visuel correspondant dans les frames Figma, et adapte le plan selon les choix UI réels — nouveaux déclencheurs, événements obsolètes, opportunités non prévues. Exécuté après Gate 8 (tous les écrans)."
argument-hint: "[epic-slug]/[feature-slug]"
agent: product-owner
---

## Rôle
Aligner le plan de tagage sur la réalité des maquettes UI finalisées — vérifier la couverture, adapter les déclencheurs aux choix visuels et structurels, identifier les nouvelles opportunités de tracking.

## Agents consommateurs
Product Owner  · UX Designer  · Product Designer 

## Prérequis
- [ ] Plan de tagage initial disponible (`specs/$ARGUMENTS/tracking-plan.md`)
- [ ] Tous les écrans finalisés — Gate 8 validée
- [ ] Frames Figma accessibles via MCP (`figma-read-design`)

## Gestion des erreurs
> ❌ Plan de tagage manquant — lancer d'abord `/write-tracking-plan $ARGUMENTS`.
> ⚠️ Utiliser les specs d'écrans `S-XX-[nom].md` comme source de vérification alternative.

## Processus

### Étape 1 — Lire le plan de tagage existant

Depuis `specs/$ARGUMENTS/tracking-plan.md`, extraire :
- Liste complète des événements
- Déclencheur prévu pour chaque événement
- Écran source associé
- KPI alimenté

### Étape 2 — Lire les frames finalisées via MCP Figma

```
get_metadata [URL page screens] → inventaire des frames et layers
get_screenshot [URL frame] → vérification visuelle par écran
```

Pour chaque frame, identifier :
- Les éléments interactifs présents (boutons, gestures, champs, toggles)
- Les écrans affichés (views, modales, bottom sheets)
- Les éléments trackables non prévus au plan initial

### Étape 3 — Vérification couverture événement par événement

```markdown
## Audit couverture tagage — [feature-slug] — [YYYY-MM-DD]

| Événement | Déclencheur prévu | Écran prévu | Élément visuel trouvé | Statut |
|-----------|------------------|------------|----------------------|--------|
| `login_succeeded` | Tap bouton "Se connecter" | US_042_login_ios_default | ✅ Bouton CTA présent | ✅ Couvert |
| `onboarding_skipped` | Tap lien "Passer" | US_001_onboarding_ios_step_1 | ❌ Lien supprimé — remplacé par swipe | ⚠️ À adapter |
| `form_submitted` | Tap bouton "Valider" | US_015_profile_ios_default | ✅ Bouton présent | ✅ Couvert |
```

**Statuts possibles :**
- ✅ Couvert — élément visuel identifiable, déclencheur intact
- ⚠️ À adapter — élément présent mais déclencheur modifié par les choix UI
- ❌ Obsolète — élément supprimé ou flow restructuré
- ➕ Nouveau — opportunité de tracking identifiée, non prévue initialement

### Étape 4 — Adapter les déclencheurs modifiés

Pour chaque événement ⚠️ À adapter :

```markdown
## Adaptations requises

### `onboarding_skipped`
**Situation :** Le lien "Passer" a été remplacé par un swipe gesture vers la droite
lors de la direction artistique.
**Déclencheur initial :** Tap lien "Passer"
**Nouveau déclencheur :** Swipe right depuis n'importe quel écran onboarding
**Propriétés :** Ajouter `step_index: int` pour savoir à quelle étape le skip intervient
**Impact KPI :** Aucun — le KPI "taux de skip onboarding" reste mesurable

### `[autre événement]`
[Même structure]
```

### Étape 5 — Identifier les nouvelles opportunités

Les choix UI créent parfois des interactions non prévues au plan initial :

```markdown
## Nouvelles opportunités de tracking

### ➕ `bottom_sheet_dismissed`
**Origine :** La direction artistique a introduit une bottom sheet pour les filtres
— non prévue dans les specs initiales.
**Intérêt :** Mesurer si les utilisateurs ouvrent et ferment les filtres sans appliquer
**Déclencheur :** Swipe down ou tap fond pour fermer la bottom sheet
**Écran :** US_023_catalog_ios_default
**KPI potentiel :** Taux d'engagement avec les filtres
**Recommandation :** Ajouter au plan — [Must / Should / Could]

### ➕ `[autre opportunité]`
[Même structure]
```

### Étape 6 — Traiter les événements obsolètes

```markdown
## Événements obsolètes

### ❌ `tutorial_video_played`
**Raison :** La vidéo tutoriel a été supprimée lors de la direction artistique
— remplacée par un onboarding illustré.
**Décision :** Supprimer du plan / Remplacer par `illustration_step_viewed`
```

### Étape 7 — Produire le plan de tagage révisé

Mettre à jour `specs/$ARGUMENTS/tracking-plan.md` avec :
- Déclencheurs corrigés
- Nouveaux événements ajoutés (validés)
- Événements obsolètes marqués et supprimés
- Version incrémentée (MINOR)

```markdown
## Résumé des révisions — v[X.Y]

| Type | Nombre | Détail |
|------|--------|--------|
| ✅ Événements confirmés | [N] | Aucun changement |
| ⚠️ Déclencheurs adaptés | [N] | [liste] |
| ➕ Nouveaux événements ajoutés | [N] | [liste] |
| ❌ Événements supprimés | [N] | [liste] |

**Couverture KPIs :** [N/N KPIs couverts]
**Version plan :** v[X.Y-initial] → v[X.Y-révisé]
```

## Phase de validation — niveau approfondi

Avant de valider ce livrable et de passer à la suite, réponds à ces questions dans l'ordre :

1. Chaque événement du plan initial a-t-il un statut explicite — aucun laissé sans décision ? (oui / non + événements sans statut)
2. Tous les déclencheurs ⚠️ ont-ils un nouveau déclencheur défini précisément — pas juste "à revoir" ? (oui / non + déclencheurs à préciser)
3. Les nouveaux événements ➕ ont-ils une recommandation Must/Should/Could pour aider la décision Product Designer ? (oui / non)
4. La couverture des KPIs du cadrage est-elle maintenue après les révisions — aucun KPI laissé sans événement ? (oui / non + KPIs découverts)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ review-tracking-plan "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/tracking-plan.md (mis à jour — v[X.Y])

⚠️ Validation Product Designer requise (lot Gate 9)
"Le plan de tagage révisé reflète-t-il les choix UI finaux ?
Les nouveaux événements identifiés sont-ils à ajouter ?"

Prochaines étapes :
→ /write-functional-doc [epic-slug] (inclure le plan révisé)
→ /write-figma-handoff [feature-slug]
→ Plan transmis au Dev avec le handoff
```
