---
name: tracking-plan
description: "Produit et révise le plan de tagage — définit les événements analytics depuis les KPIs, les mappe sur les écrans Figma, puis révise après les choix UI. Use when user says 'plan de tagage', 'tracking plan', 'événements analytics', 'KPIs', or 'tracking-plan'."
argument-hint: "[epic-slug]/[feature-slug]"
agent: product-owner
---

## Rôle
Deux phases : création du plan depuis les KPIs, puis révision après la phase UI.


## Prérequis
- [ ] MCP Figma connecté (Phase B — révision)
- [ ] `cadrage.md` disponible — KPIs définis
- [ ] Gate 9 en cours (Phase A) · Gate 8 validée (Phase B)
- [ ] `FLUX-###` disponibles — événements mappés sur les flux

## Phase A — Création du plan de tagage (après Gate 9 initiale)
### Étape 1 — Extraire les KPIs du cadrage

Depuis `specs/[epic-slug]/cadrage.md`, lister les KPIs définis :

```markdown
## KPIs à mesurer

| KPI | Définition | Cible | Source de données |
|-----|-----------|-------|------------------|
| Taux de complétion onboarding | % utilisateurs qui terminent les N étapes | ≥ 70% | Événements onboarding |
| Taux de conversion connexion | % tentatives qui aboutissent | ≥ 85% | Événements auth |
| Temps moyen première action | Délai entre 1er lancement et 1ère action à valeur | ≤ 3 min | Événements session |
```

### Étape 2 — Identifier les événements nécessaires par KPI

Pour chaque KPI, identifier les événements à tracker :

```markdown
## Mapping KPI → Événements

| KPI | Événements requis |
|-----|------------------|
| Taux de complétion onboarding | `onboarding_step_viewed`, `onboarding_step_completed`, `onboarding_skipped`, `onboarding_completed` |
| Taux de conversion connexion | `login_attempted`, `login_succeeded`, `login_failed` |
```

### Étape 3 — Mapper sur les écrans (UX Designer)

Pour chaque événement, identifier l'écran et le déclencheur exact depuis les specs d'écran :

```markdown
## Mapping Événements → Écrans

| Événement | Écran | Déclencheur | FLUX-### |
|-----------|-------|------------|---------|
| `onboarding_step_viewed` | S-01-onboarding-step1 | Affichage de l'écran | FLUX-001 |
| `onboarding_step_completed` | S-01 à S-03 | Tap bouton "Suivant" | FLUX-001 |
| `login_attempted` | S-05-login | Tap bouton "Se connecter" | FLUX-002 |
```

### Étape 4 — Définir les propriétés par événement

```markdown
# Plan de Tagage — [FEAT-###] [NomFeature] — [YYYY-MM-DD]

**Version :** [MAJOR.MINOR]
**Feature :** [FEAT-###]
**Epic :** [EPIC-###]
**Outil cible :** [À définir par le Dev — Mixpanel / Amplitude / Firebase / autre]

---

## Événements

### `[nom_evenement]`
> Convention de nommage : `[objet]_[action]` en snake_case — ex: `login_succeeded`

| Champ | Valeur |
|-------|--------|
| **Nom** | `[nom_evenement]` |
| **Description** | [Ce que cet événement mesure en une phrase] |
| **Écran source** | S-XX — [NomÉcran] — [URL Figma] |
| **Déclencheur** | [Action exacte qui déclenche l'événement — ex: "Tap bouton Connexion"] |
| **FLUX** | [FLUX-###] |
| **KPI alimenté** | [KPI concerné] |

**Propriétés :**

| Propriété | Type | Exemple | Description |
|-----------|------|---------|-------------|
| `user_id` | String | `"usr_123abc"` | Identifiant unique utilisateur |
| `session_id` | String | `"sess_xyz789"` | Identifiant de session |
| `feature` | String | `"[nom-feature]"` | Feature source de l'événement |
| `[propriété spécifique]` | [String/Int/Bool] | `[exemple]` | [Description] |

---

[Répéter pour chaque événement]

---

## Récapitulatif — Tous les événements

| # | Événement | Écran | KPI | Priorité |
|---|-----------|-------|-----|---------|
| 1 | `[nom]` | S-XX | [KPI] | [Must / Should / Could] |
| 2 | `[nom]` | S-XX | [KPI] | |

**Total : [N] événements**

---

## Propriétés globales (sur tous les événements)

Ces propriétés sont présentes sur **chaque** événement — ne pas les répéter dans les specs individuelles :

| Propriété | Type | Description |
|-----------|------|-------------|
| `user_id` | String | Identifiant utilisateur unique |
| `session_id` | String | Identifiant de session |
| `platform` | String | `"ios"` ou `"android"` |
| `app_version` | String | `"1.2.0"` |
| `feature` | String | Nom de la feature source |
| `timestamp` | ISO 8601 | Horodatage de l'événement |

---

## Convention de nommage des événements

```
[objet]_[action]

Objets : screen, button, form, onboarding, login, item, flow, error
Actions : viewed, tapped, completed, failed, skipped, submitted, dismissed

Exemples valides :
✅ login_succeeded
✅ onboarding_step_viewed
✅ form_submitted
✅ error_displayed

Exemples invalides :
❌ LoginSuccess (PascalCase)
❌ login-success (kebab-case)
❌ userClickedLoginButton (trop verbeux)
```

---

## Événements hors scope (V2+)

| Événement | KPI potentiel | Raison du report |
|-----------|--------------|-----------------|
| `[nom]` | [KPI] | [Raison] |
```

## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. Chaque KPI du cadrage est-il couvert par au moins un événement ? (oui / non + KPIs non couverts)
2. Chaque événement est-il mappé sur un écran existant avec une URL Figma valide ? (oui / non + événements sans écran)
3. La convention de nommage snake_case est-elle respectée sur tous les événements ? (oui / non + événements à renommer)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-tracking-plan "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/tracking-plan.md

⚠️ Validation Product Designer requise (lot Gate 9)
"Le plan de tagage couvre-t-il tous les KPIs définis au cadrage ?"

Prochaines étapes :
→ Après Gate 8 : /review-tracking-plan [epic-slug]/[feature-slug]
→ Plan révisé inclus dans /write-functional-doc [epic-slug]
→ Plan transmis au Dev avec /write-figma-handoff [feature-slug]
→ Implémentation tracking — hors scope conception

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```

---

## Phase B — Révision selon les choix UI (après Gate 8)
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

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```

---

## Résumé de fin d'exécution

```
✅ tracking-plan "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/tracking-plan.md (v finale)

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]

Prochaines étapes :
→ /write-functional-doc [epic-slug]
→ Gate 9 (lot)
```
