---
name: write-strategic-research
description: "Réalise le research stratégique complet en une passe — analyse concurrentielle, veille sectorielle et benchmark UX/UI. Use when user says 'research stratégique', 'lance le research', 'analyse la concurrence', 'competitive analysis', 'sector watch', or 'benchmark ux'."
argument-hint: "[project-slug]"
agent: ux-researcher
---

## Rôle
Produire les 3 livrables du research stratégique en une seule exécution.
Toujours lancé avant le cadrage — Gate -1.


## Prérequis
- [ ] Accès web disponible (web_search + web_fetch)
- [ ] Nom du projet et secteur d'activité connus
## Étape 1 — Analyse concurrentielle
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

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```

---

## Étape 2 — Veille sectorielle
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

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```

---

## Étape 3 — Benchmark UX/UI
### Étape 1 — Identifier les patterns de référence

```
web_search: "[secteur] mobile app UX patterns best practices [année]"
web_search: "[type de produit] onboarding UX examples"
web_search: "[secteur] app navigation patterns iOS Android"
web_search: "mobile UX [secteur] case study [année]"
```

Catégoriser les sources :
- **Primaires** — études de cas, articles UX de référence (Nielsen Norman, UX Collective, etc.)
- **Secondaires** — blogs, analyses d'applications
- **Observationnelles** — captures d'écran publiques, vidéos de démo

### Étape 2 — Documenter les patterns par catégorie

```markdown
## Benchmark UX/UI — [project-slug] — [YYYY-MM]

### Navigation
| Pattern | Fréquence dans le secteur | Applications exemples | Recommandation |
|---------|--------------------------|----------------------|----------------|
| Tab bar (iOS) | Très fréquent | [App A, App B] | Standard attendu |
| Bottom Nav (Android) | Très fréquent | [App A, App B] | Standard attendu |
| Navigation latérale | Rare | [App C] | À éviter |

### Onboarding
| Pattern | Fréquence | Exemple | Note |
|---------|-----------|---------|------|
| Walkthrough 3 écrans | Fréquent | [App] | Efficace si contenu court |
| Empty state actif | Tendance croissante | [App] | Recommandé pour apps simples |
| Permission tardive | Best practice | [App] | Toujours recommandé |

### Composants récurrents dans le secteur
| Composant | Usage standard | Variante observée | Source |
|-----------|---------------|------------------|--------|
| [Composant] | [Description standard] | [Variante intéressante] | [App] |

### Conventions d'interaction
| Action | Convention standard iOS | Convention standard Android |
|--------|------------------------|---------------------------|
| Retour | Swipe gauche | Bouton back système |
| Supprimer | Swipe gauche → confirmation | Long press → menu |
| Partager | Share sheet natif | Share intent natif |

### Patterns à éviter dans ce secteur
| Pattern | Raison | Source |
|---------|--------|--------|
| [Pattern négatif] | [Pourquoi les utilisateurs le détestent] | [Avis / étude] |
```

### Étape 3 — Opportunités de différenciation UX

```markdown
## Différenciation UX possible pour [project-slug]

### Ce que tous font de la même façon (standards à respecter)
- [Pattern universel 1 — ne pas innover ici]
- [Pattern universel 2]

### Ce que personne ne fait bien (opportunité)
- [Gap UX 1 — opportunité de se différencier]
- [Gap UX 2]

### Tendances émergentes à considérer
- [Tendance UX émergente 1]
- [Tendance UX émergente 2]
```

### Étape 4 — Recommandations pour le UX Designer

```markdown
## Pour le UX Designer

### Patterns à adopter (standards du secteur)
- [Pattern] — les utilisateurs s'y attendent

### Patterns à éviter
- [Pattern] — frustration documentée dans le secteur

### Patterns à innover
- [Domaine] — opportunité de différenciation identifiée
```

## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. Le benchmark couvre-t-il min. 5 applications du secteur — dont au moins 2 références internationales ? (oui / non + applications manquantes)
2. Les patterns sont-ils distingués entre "standards attendus" et "opportunités de différenciation" ? (oui / non)
3. Les recommandations pour le UX Designer sont-elles actionnables — pas juste descriptives ? (oui / non + recommandations à retravailler)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-ux-benchmark "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/research/ux-benchmark.md

⚠️ Gate -1 — Validation Humaine requise (research stratégique complet)
Lot : competitive-analysis + sector-watch + ux-benchmark
"Les analyses stratégiques donnent-elles une base suffisante pour démarrer le cadrage ?"

Prochaines étapes :
→ Gate -1 validée → /write-cadrage $ARGUMENTS
→ Les 3 rapports alimentent le cadrage et les règles métier

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```

---

## Résumé de fin d'exécution

```
✅ write-strategic-research "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/research/competitive-analysis.md
📁 specs/$ARGUMENTS/research/sector-watch.md
📁 specs/$ARGUMENTS/research/ux-benchmark.md

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]

Prochaines étapes :
→ /write-user-research $ARGUMENTS
→ /write-discovery-interview $ARGUMENTS (en parallèle)
→ Gate -1 après validation des 3 livrables
```
