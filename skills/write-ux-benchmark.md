---
name: write-ux-benchmark
description: "Benchmark automatique des patterns UX/UI du secteur — navigation, onboarding, composants récurrents, conventions d'interaction. Collecte via web_search + web_fetch, analyse par le Product Designer. Exécuté par le ux-researcher avant le cadrage."
argument-hint: "[project-slug]"
agent: ux-researcher
---

## Rôle
Identifier et documenter les patterns UX/UI standards du secteur — ce que les utilisateurs s'attendent à trouver, les conventions qui fonctionnent, et les opportunités de différenciation.

## Agents consommateurs
UX Researcher  · UX Designer  · UI Designer 

## Prérequis
- [ ] Secteur d'activité et type d'application identifiés
- [ ] Analyse concurrentielle terminée (`write-competitive-analysis`)
- [ ] Stack cible connue (iOS natif / Android natif / cross-platform)

## Processus

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
```
