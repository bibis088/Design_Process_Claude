---
name: write-tracking-plan
description: "Produit le plan de tagage d'une feature — liste des événements analytics à implémenter, avec écran source, propriétés et KPI alimenté. Le PO définit quoi mesurer, l'UX Designer mappe sur les écrans. Exécuté après la Gate 9."
argument-hint: "[epic-slug]/[feature-slug]"
agent: product-owner
---

## Rôle
Produire le plan de tagage complet d'une feature — document de référence pour le Dev qui implémente le tracking et pour le Product Designer qui analysera les données en production.

## Responsabilités partagées

| Étape | Agent | Action |
|-------|-------|--------|
| Définir les KPIs à mesurer | `product-owner` | Depuis les KPIs du cadrage et les critères d'acceptance |
| Identifier les événements nécessaires | `product-owner` | 1 KPI = N événements |
| Mapper les événements sur les écrans | `ux-designer` | Depuis les specs d'écrans et les user flows |
| Définir les propriétés par événement | `product-owner` | Données contextuelles utiles à l'analyse |
| Valider le plan complet | Product Designer | Gate 9 (lot avec la doc fonctionnelle) |
| Implémenter | Dev (hors scope conception) | À partir du plan livré |

## Prérequis
- [ ] Gate 9 validée — review scope Figma approuvée
- [ ] KPIs définis dans `specs/[epic-slug]/cadrage.md`
- [ ] Specs d'écrans `S-XX-[nom].md` disponibles
- [ ] User flows `FLUX-###` disponibles

## Gestion des erreurs
> ❌ KPIs manquants — compléter la section KPIs dans `cadrage.md` avant de produire le plan de tagage.

## Processus de génération

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
```
