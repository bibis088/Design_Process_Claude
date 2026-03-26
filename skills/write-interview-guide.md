---
name: write-interview-guide
description: Produit le guide d'entretien utilisateur structuré — questions ouvertes, scénarios, protocole. Guide le Product Designer pour conduire des interviews qualitatives efficaces. Exécuté par le ux-researcher.
argument-hint: [epic-slug]
disable-model-invocation: false
context: fork
agent: ux-researcher
---

## Rôle
Produire un guide d'entretien structuré et actionnable que le Product Designer utilise pour conduire des interviews utilisateurs semi-directives.

## Agents consommateurs
- UX Researcher (pilote — produit le guide)
- Product Designer (exécute — conduit les interviews)

## Prérequis
- [ ] Plan de recherche validé (`write-research-plan`)
- [ ] Hypothèses à valider identifiées
- [ ] Personas hypothétiques disponibles

## Processus

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
```
