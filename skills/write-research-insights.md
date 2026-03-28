---
name: write-research-insights
description: "Synthétise les notes brutes des interviews en insights priorisés et recommandations actionnables pour le UX Designer. Met à jour les personas hypothétiques. Exécuté par le ux-researcher après les sessions."
argument-hint: "[epic-slug]"
disable-model-invocation: false
context: fork
agent: ux-researcher
---

## Rôle
Transformer les notes brutes des interviews en insights structurés, prioriser les découvertes, mettre à jour les personas et produire des recommandations actionnables pour le UX Designer.

## Agents consommateurs
- UX Researcher (pilote — structure les insights)
- Product Designer (valide — confirme les interprétations)
- UX Designer (consommateur — base pour les specs d'écran)
- Business Analyst (consommateur — peut impacter les règles métier)

## Prérequis
- [ ] Notes brutes des interviews disponibles (fournies par le Product Designer)
- [ ] Grilles de prise de notes remplies
- [ ] Hypothèses initiales du plan de recherche disponibles

## Gestion des erreurs

Si les notes sont insuffisantes :
> ⚠️ Notes insuffisantes pour synthèse fiable — min. 3 sessions recommandées. Documenter les limites de la recherche dans le rapport.

## Processus

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
```
