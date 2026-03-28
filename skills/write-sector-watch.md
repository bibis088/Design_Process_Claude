---
name: write-sector-watch
description: "Collecte automatique des tendances sectorielles, actualités récentes et réglementations applicables au projet. Collecte via web_search + web_fetch, analyse par le Product Designer. Exécuté par le ux-researcher avant le cadrage."
argument-hint: "[project-slug]"
agent: ux-researcher
---

## Rôle
Collecter et structurer les tendances du secteur, les actualités pertinentes et les contraintes réglementaires applicables — pour contextualiser le projet et anticiper les contraintes.

## Agents consommateurs
UX Researcher  · Business Analyst  · Product Owner 

## Prérequis
- [ ] Secteur d'activité identifié
- [ ] Pays / marchés cibles connus
- [ ] Stack technique connue (pour les réglementations spécifiques)

## Processus

### Étape 1 — Tendances sectorielles

```
web_search: "[secteur] tendances [année en cours]"
web_search: "[secteur] mobile app trends [année en cours]"
web_search: "[secteur] innovations UX [année en cours]"
web_search: "[type de produit] user expectations [année en cours]"
```

```markdown
## Tendances sectorielles — [secteur] — [YYYY-MM]

| Tendance | Description | Impact potentiel | Source |
|----------|-------------|-----------------|--------|
| [Tendance 1] | [Description] | [Fort / Moyen / Faible] | [URL] |
| [Tendance 2] | [Description] | [Fort / Moyen / Faible] | [URL] |
```

### Étape 2 — Actualités récentes (6 derniers mois)

```
web_search: "[secteur] actualités [mois année]"
web_search: "[secteur] nouveautés applications mobiles [mois année]"
web_search: "[nom concurrents] lancement mise à jour [mois année]"
```

```markdown
## Actualités — [YYYY-MM]

| Date | Titre | Source | Pertinence |
|------|-------|--------|-----------|
| [Date] | [Titre article] | [Source] | [Haute / Moyenne / Faible] |
```

### Étape 3 — Réglementations applicables

```
web_search: "[secteur] réglementation RGPD mobile [année]"
web_search: "[secteur] conformité légale application [pays]"
web_search: "[type de données collectées] réglementation [pays]"
web_search: "accessibilité numérique obligation légale [pays] [année]"
```

```markdown
## Réglementations applicables — [project-slug]

| Réglementation | Domaine | Niveau d'obligation | Impact sur le projet | Source |
|---------------|---------|--------------------|--------------------|--------|
| RGPD | Données personnelles | Obligatoire — EU | [Impact] | [URL] |
| RAAM | Accessibilité mobile | Obligatoire — FR (secteur public) / Recommandé (privé) | [Impact] | [URL] |
| [Réglementation secteur] | [Domaine] | [Niveau] | [Impact] | [URL] |

### Contraintes légales identifiées
- [ ] [Contrainte 1] — à intégrer dans les règles métier `RB-###`
- [ ] [Contrainte 2] — à intégrer dans les règles métier `RB-###`
```

### Étape 4 — Synthèse pour le cadrage

```markdown
## Ce que le BA doit intégrer dans le cadrage

### Opportunités de marché
- [Tendance exploitable 1]
- [Tendance exploitable 2]

### Risques à anticiper
- [Risque réglementaire ou marché 1]
- [Risque 2]

### Contraintes non négociables
- [Contrainte légale 1 — obligatoire]
- [Contrainte légale 2 — obligatoire]
```

## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. Les tendances couvrent-elles les 6 derniers mois minimum avec sources datées ? (oui / non + période manquante)
2. Les réglementations applicables au secteur ET au pays cible sont-elles toutes identifiées ? (oui / non + réglementations à vérifier)
3. Les contraintes légales sont-elles clairement distinguées des simples recommandations ? (oui / non)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-sector-watch "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/research/sector-watch.md

Prochaines étapes :
→ /write-ux-benchmark $ARGUMENTS
→ Gate -1 après les 3 skills de research stratégique
```
