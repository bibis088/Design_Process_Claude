---
name: write-research-plan
description: "Produit le plan de recherche utilisateur — méthodes, questions de recherche, profils participants, calendrier. Guide le Product Designer pour recruter et planifier les sessions. Exécuté par le ux-researcher après le brief fonctionnel."
argument-hint: "[epic-slug]"
agent: ux-researcher
---

## Rôle
Structurer le plan de recherche utilisateur depuis les hypothèses du BA — quelles questions valider, quelle méthode, qui recruter, comment planifier.

## Agents consommateurs
UX Researcher  · Product Designer 

## Prérequis
- [ ] Brief Fonctionnel `specs/$ARGUMENTS/EPIC.md` disponible
- [ ] Personas hypothétiques `specs/$ARGUMENTS/personas/` disponibles
- [ ] Flux fonctionnels `FLUX-###` disponibles
- [ ] Gate -1 validée (research stratégique terminé)

## Gestion des erreurs
> ❌ Brief ou personas manquants — lancer d'abord `/write-brief-fonctionnel $ARGUMENTS`.

## Processus

### Étape 1 — Extraire les hypothèses à valider

Depuis le Brief Fonctionnel et les personas, lister les hypothèses non vérifiées :

```markdown
## Hypothèses à valider

| # | Hypothèse | Source | Priorité |
|---|-----------|--------|---------|
| H1 | [PERSONA-001] utilise l'app en déplacement | BA cadrage | Haute |
| H2 | Le flux [FLUX-001] est intuitif sans onboarding | BA flux | Haute |
| H3 | Les utilisateurs préfèrent X à Y | Supposition | Moyenne |
```

### Étape 2 — Définir les questions de recherche

```markdown
## Questions de recherche

### Questions primaires (bloquantes pour le design)
1. [Question dont la réponse change le design si fausse]
2. [Question sur le flux principal]

### Questions secondaires (enrichissantes)
3. [Question sur les préférences]
4. [Question sur le contexte d'usage]
```

### Étape 3 — Choisir les méthodes

```markdown
## Méthodes recommandées

| Méthode | Questions adressées | Nombre sessions | Durée | Quand |
|---------|--------------------|-----------------|----- |-------|
| Entretien semi-directif | H1, H2, H3 | 5-7 | 45-60 min | Avant design |
| Test d'utilisabilité | H2 | 5 | 30-45 min | Sur maquettes |
| Observation contextuelle | H1 | 2-3 | 1-2h | Optionnel |
```

### Étape 4 — Définir les profils participants

```markdown
## Profils recherchés

### Profil 1 — [Nom du persona correspondant]
Critères d'inclusion :
- [Critère 1 — ex: utilise des apps mobiles quotidiennement]
- [Critère 2]
Critères d'exclusion :
- [Ce qui disqualifie — ex: travaille dans le secteur]
Nombre : [N participants]

### Profil 2 — [Nom du persona correspondant]
[Même structure]
```

### Étape 5 — Calendrier et logistique

```markdown
## Plan opérationnel

| Semaine | Action | Responsable |
|---------|--------|------------|
| S1 | Recrutement participants | Product Designer |
| S2 | Sessions entretiens (N × 45 min) | Product Designer |
| S3 | Analyse + synthèse insights | Product Designer + UX Researcher |
| S4 | Tests utilisabilité sur maquettes | Product Designer (après maquettes UX) |

## Outils suggérés
- Recrutement : [Calendly, Typeform, ou réseau direct]
- Sessions à distance : [Teams, Zoom, Google Meet]
- Prise de notes : [Notion, Dovetail, ou notes texte structurées]
- Enregistrement : [Avec consentement explicite]

## Template de consentement
[L'agent génère un template simple de consentement à faire signer aux participants]
```

## Phase de validation — niveau approfondi

Avant de valider ce livrable et de passer à la suite, réponds à ces questions dans l'ordre :

1. Les hypothèses à valider sont-elles extraites du Brief et des personas — pas inventées ? (oui / non + hypothèses à revoir)
2. Le nombre de participants est-il réaliste selon les contraintes du projet (budget, temps) ? (oui / non + ajustements)
3. Les profils participants correspondent-ils aux personas hypothétiques du BA ? (oui / non + écarts)
4. Le calendrier est-il compatible avec le planning projet ? (oui / non + ajustements)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-research-plan "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/research/research-plan.md

Prochaines étapes :
→ Product Designer : recruter les participants selon les profils définis
→ /write-interview-guide $ARGUMENTS (guide d'entretien)
→ Sessions à planifier avant de démarrer le UX Design
```
