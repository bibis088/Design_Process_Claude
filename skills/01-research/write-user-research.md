---
name: write-user-research
description: "Prépare et synthétise le research utilisateur complet — plan de recherche, guide d'entretien et insights. Product Designer conduit les sessions. Use when user says 'research utilisateur', 'plan de recherche', 'guide entretien', 'insights utilisateurs', or 'write-user-research'."
argument-hint: "[epic-slug]"
agent: ux-researcher
---

## Rôle
Produire le research utilisateur en 3 étapes séquentielles.
Le Product Designer conduit les sessions entre l'étape 2 et l'étape 3.


## Prérequis
- [ ] Secteur d'activité et cible utilisateur identifiés
- [ ] Gate -1 research stratégique en cours ou validée
- [ ] Participants potentiels identifiés pour les sessions
- [ ] `PERSONA-###` Draft disponibles ou contexte utilisateur connu
- [ ] Contexte projet disponible (brief initial ou `project-init`)

## Étape 1 — Plan de recherche
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

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```

---

## Étape 2 — Guide d'entretien
### Étape 1 — Structure du guide

```markdown
# Guide d'entretien — [NomProjet] — [YYYY-MM-DD]

## Informations pratiques
- Durée : [45-60 minutes]
- Format : [Distanciel / Présentiel]
- Enregistrement : [Oui — avec consentement signé / Non]
- Intervieweur : [Product Designer]
- Observateur : [Optionnel]

## Objectifs de la session
1. [Valider hypothèse H1]
2. [Valider hypothèse H2]
3. [Explorer le contexte d'usage]

---

## Introduction (5 min)
Script :
"Bonjour, je m'appelle [X]. Merci de participer à cette session.
Je travaille sur [description vague du projet — ne pas influencer].
Il n'y a pas de bonne ou mauvaise réponse — je veux comprendre
comment vous faites les choses aujourd'hui, pas tester vos compétences.
Puis-je enregistrer la session pour mes notes personnelles ? [Consentement]
Des questions avant de commencer ?"

---

## Warm-up — contexte personnel (5-10 min)
Questions brise-glace, non liées au produit :
- "Pouvez-vous me décrire votre journée type ?"
- "Quelle place occupe [domaine lié au produit] dans votre quotidien ?"
- "Quelles applications utilisez-vous le plus souvent ? Pourquoi ?"

---

## Corps — exploration des hypothèses (25-35 min)

### Thème 1 — [Lié à H1]
Objectif : [Ce qu'on veut apprendre]

Questions :
- "[Question ouverte principale — commence par Comment / Quand / Décrivez-moi]"
- "[Question de relance si silence : Pouvez-vous me donner un exemple ?]"
- "[Question d'approfondissement : Qu'est-ce qui vous a amené à faire ça ?]"

⚠️ À éviter : questions fermées (oui/non), questions suggestives ("Est-ce que vous aimeriez X ?")

### Thème 2 — [Lié à H2]
[Même structure]

### Thème 3 — [Lié à H3]
[Même structure]

---

## Test de concept (optionnel — 10 min)
Si des maquettes ou concepts sont disponibles :
"Je vais vous montrer quelques idées — dites-moi ce que vous comprenez,
ce que vous feriez, ce qui vous surprend."

- Présenter sans expliquer
- Observer le comportement avant de poser des questions
- "Qu'est-ce que vous attendez qu'il se passe si vous tapez ici ?"

---

## Clôture (5 min)
- "Y a-t-il quelque chose que vous auriez aimé que je vous demande ?"
- "Si vous deviez améliorer [domaine] en une chose, ce serait quoi ?"
- "Avez-vous des questions pour moi ?"
- Remercier, expliquer la suite

---

## Grille de prise de notes

| Thème | Citation directe | Observation comportementale | Insight pressenti |
|-------|-----------------|----------------------------|------------------|
| [Thème 1] | "[Citation]" | [Ce que l'interviewé a fait] | [H1 confirmée / infirmée ?] |
```

## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. Chaque hypothèse du plan de recherche est-elle couverte par au moins une question dans le guide ? (oui / non + hypothèses non couvertes)
2. Les questions sont-elles toutes ouvertes — aucune question suggestive ou fermée ? (oui / non + questions à reformuler)
3. La durée totale estimée est-elle ≤ 60 minutes — le guide n'est pas surchargé ? (oui / non + sections à alléger)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-interview-guide "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/research/interview-guide.md

Prochaines étapes :
→ Product Designer : conduire les sessions avec ce guide
→ /write-research-insights $ARGUMENTS (après les sessions)

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```

---

⚠️ **Pause — Product Designer conduit les sessions d'interviews**
Reprendre quand les notes brutes sont disponibles.

---

## Étape 3 — Synthèse des insights
### Étape 1 — Consolider les données brutes

Le Product Designer fournit les notes — l'agent les structure :

```markdown
## Données brutes consolidées

### Participant 1 — [Profil]
- Citation clé : "[...]"
- Comportement observé : [...]
- Surprise / inattendu : [...]

### Participant 2 — [Profil]
[Même structure]
```

### Étape 2 — Identifier les patterns

Regrouper les observations récurrentes (min. 3 occurrences pour un insight) :

```markdown
## Patterns identifiés

| Pattern | Occurrences | Citations représentatives | Lié à H# |
|---------|-------------|--------------------------|----------|
| [Pattern 1] | [N/N participants] | "[Citation]" | H1 |
| [Pattern 2] | [N/N participants] | "[Citation]" | H2 |
```

### Étape 3 — Valider / invalider les hypothèses

```markdown
## Bilan des hypothèses

| Hypothèse | Statut | Evidence | Impact sur le design |
|-----------|--------|----------|---------------------|
| H1 : [Énoncé] | ✅ Confirmée | [N/N participants ont...] | [Conséquence] |
| H2 : [Énoncé] | ❌ Infirmée | [Tous ont dit que...] | [Ce qu'on doit changer] |
| H3 : [Énoncé] | ⚠️ Partielle | [Nuance observée] | [Conséquence] |
```

### Étape 4 — Insights priorisés

```markdown
## Insights — [epic-slug] — [YYYY-MM-DD]

### Insight 1 — [Titre court]
**Observation :** [Ce qu'on a observé]
**Fréquence :** [N/N participants]
**Citation :** "[Citation directe représentative]"
**Impact design :** [Ce que ça implique concrètement]
**Priorité :** [Critique / Importante / Intéressante]

### Insight 2 — [Titre court]
[Même structure]
```

### Étape 5 — Mettre à jour les personas

Pour chaque `PERSONA-###` existant :

```markdown
## Mise à jour PERSONA-### — [Nom]

### Confirmé
- [Aspect du persona confirmé par la recherche]

### Enrichi
- [Nouvelle nuance ajoutée — ex: contexte d'usage plus précis]

### Corrigé
- ~~[Ancienne hypothèse]~~ → [Réalité observée]

### Nouveau besoin identifié
- [Besoin non anticipé dans le cadrage]
```

### Étape 6 — Recommandations pour le UX Designer

```markdown
## Recommandations design

### Priorité critique — à adresser impérativement
- [Recommandation 1 avec justification insight]

### Priorité haute — fortement recommandé
- [Recommandation 2]

### Opportunités — si temps disponible
- [Recommandation 3]

### À ne pas faire — anti-patterns identifiés
- [Ce que les utilisateurs détestent dans les solutions existantes]
```

## Phase de validation — niveau approfondi

Avant de valider ce livrable et de passer à la suite, réponds à ces questions dans l'ordre :

1. Chaque insight repose-t-il sur min. 3 occurrences — aucune généralisation depuis 1 participant ? (oui / non + insights à pondérer)
2. Les hypothèses infirmées sont-elles clairement documentées — pas minimisées ? (oui / non)
3. Les personas ont-ils été mis à jour pour refléter la réalité observée ? (oui / non + personas à mettre à jour)
4. Les recommandations sont-elles actionnables par le UX Designer sans ambiguïté ? (oui / non + recommandations à préciser)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-research-insights "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/research/insights.md
📁 specs/$ARGUMENTS/personas/ (personas mis à jour)

⚠️ Gate 4a — Validation Humaine requise
"Les insights confirment-ils les hypothèses de personas et de flux ?"

Prochaines étapes :
→ Gate 4a validée → /write-navigation-map $ARGUMENTS (UX Designer)
→ /write-usability-test $ARGUMENTS (après maquettes UX)

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```

---

## Résumé de fin d'exécution

```
✅ write-user-research "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/research/research-plan.md
📁 specs/$ARGUMENTS/research/interview-guide.md
📁 specs/$ARGUMENTS/research/insights.md

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]

Prochaines étapes :
→ Gate -1 (lot avec write-strategic-research + write-discovery-interview)
```
