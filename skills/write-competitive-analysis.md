---
name: write-competitive-analysis
description: "Analyse automatique des concurrents directs et indirects — positionnement, fonctionnalités, UX, forces et faiblesses. Collecte via web_search + web_fetch, analyse par le Product Designer. Exécuté par le ux-researcher avant le cadrage."
argument-hint: "[project-slug]"
agent: ux-researcher
---

## Rôle
Collecter et structurer automatiquement les informations sur les concurrents du projet — pour alimenter le cadrage et orienter les décisions de design et de positionnement.

## Agents consommateurs
UX Researcher  · Business Analyst  · UX Designer 

## Prérequis
- [ ] Nom du projet ou secteur d'activité fourni par le Product Designer
- [ ] Liste initiale de concurrents pressentis (ou à identifier via recherche)

## Gestion des erreurs
> Lancer d'abord une recherche : `web_search "[secteur] app mobile concurrents leaders [année]"`

## Processus

### Étape 1 — Identifier les concurrents

```
web_search: "[secteur] meilleures applications mobiles [année]"
web_search: "[secteur] app mobile concurrents [pays cible]"
web_search: "[type de produit] alternatives comparatif"
```

Catégoriser :
- **Directs** (même cible, même usage) — analyser en profondeur — min. 3
- **Indirects** (cible ou usage différent mais compétitifs) — analyser en surface — min. 2
- **Inspirants** (hors secteur mais UX de référence) — noter les patterns — min. 2

### Étape 2 — Collecter les données par concurrent

Pour chaque concurrent direct, via `web_fetch` sur leur site, App Store / Play Store, et articles de presse :

```markdown
## [Nom du concurrent]

### Identité
- Type : [Direct / Indirect / Inspirant]
- Plateforme : [iOS / Android / Les deux / Web]
- Cible : [Qui utilise ce produit]
- Modèle : [Freemium / Payant / B2B / B2C]
- Évaluation store : [Note / Nombre d'avis]
- Dernière mise à jour : [Date]
- URL : [Store ou site]

### Fonctionnalités clés
- [Fonctionnalité 1]
- [Fonctionnalité 2]
- [Fonctionnalité 3]

### Points forts (d'après avis et presse)
- [Force 1]
- [Force 2]

### Points faibles (d'après avis et presse)
- [Faiblesse 1]
- [Faiblesse 2]

### Observations UX (si screenshots disponibles)
- [Observation 1]
- [Observation 2]

### Sources
- [URL 1]
- [URL 2]
```

### Étape 3 — Matrice comparative

```markdown
## Matrice concurrentielle — [project-slug]

| Critère | Notre projet | [Concurrent 1] | [Concurrent 2] | [Concurrent 3] |
|---------|-------------|---------------|---------------|---------------|
| Plateforme | iOS + Android | [val] | [val] | [val] |
| Cible | [val] | [val] | [val] | [val] |
| Fonctionnalité A | ❓ | ✅ | ✅ | ❌ |
| Fonctionnalité B | ❓ | ❌ | ✅ | ✅ |
| Accessibilité | ❓ | [val] | [val] | [val] |
| Note store | ❓ | [val] | [val] | [val] |
| Prix | ❓ | [val] | [val] | [val] |
```

### Étape 4 — Opportunités identifiées

```markdown
## Opportunités pour [project-slug]

### Ce que personne ne fait bien
- [Gap identifié 1]
- [Gap identifié 2]

### Ce que tous font — à implémenter
- [Standard du secteur 1]
- [Standard du secteur 2]

### Ce à éviter — frustrations communes
- [Problème récurrent 1 dans les avis]
- [Problème récurrent 2]
```

## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. L'analyse couvre-t-elle min. 3 concurrents directs avec données suffisantes ? (oui / non + concurrents à compléter)
2. Les sources sont-elles citées pour chaque donnée clé — aucune affirmation sans source ? (oui / non)
3. Les opportunités identifiées sont-elles des gaps réels — pas des suppositions ? (oui / non)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-competitive-analysis "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/research/competitive-analysis.md

⚠️ Validation Humaine requise — Gate -1 (partielle)
"L'analyse concurrentielle est-elle complète et pertinente pour orienter le cadrage ?"

Prochaines étapes :
→ /write-sector-watch $ARGUMENTS
→ /write-ux-benchmark $ARGUMENTS
→ Gate -1 complète après ces 3 skills
```
