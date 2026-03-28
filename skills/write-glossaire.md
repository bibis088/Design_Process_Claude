---
name: write-glossaire
description: "Construit et maintient le glossaire projet — référence partagée entre tous les agents. Exécuté par le business-analyst."
argument-hint: "[epic-slug]"
agent: business-analyst
---

## Rôle
Construire et maintenir un glossaire projet — référence partagée entre tous les agents pour éviter les ambiguïtés terminologiques.

## Agents consommateurs
Business Analyst  · Tous les agents 

## Prérequis
- [ ] Le Brief Fonctionnel est au moins en `Draft`
- [ ] Les premiers flux fonctionnels sont identifiés

## Processus de génération

### Étape 1 — Identifier les termes ambigus
Parcourir les documents produits et relever :
- Tout terme métier spécifique au domaine client
- Tout terme technique utilisé dans un sens particulier sur ce projet
- Tout synonyme potentiellement confusant (ex: "utilisateur" vs "client" vs "member")
- Tout acronyme ou abréviation

### Étape 2 — Rédiger le glossaire

```markdown
# Glossaire — [Nom du projet]

## Métadonnées
- Epic(s) couverts : [EPIC-###, EPIC-###]
- Business Analyst : [nom]
- Dernière mise à jour : [YYYY-MM-DD]

## Termes

### [Terme]
- **Définition** : [Définition précise dans le contexte de ce projet]
- **Synonymes acceptés** : [Autres mots utilisés pour désigner la même chose]
- **À ne pas confondre avec** : [Terme proche mais différent]
- **Utilisé dans** : [FLUX-###, US-###, RB-###]

### [Terme 2]
[Même structure]

## Acronymes et abréviations

| Abréviation | Forme complète | Contexte d'usage |
|-------------|---------------|-----------------|
| [ABC] | [Forme complète] | [Où et quand on l'utilise] |

## Termes exclus (à ne pas utiliser)
Ces termes créent de la confusion — utiliser les termes du glossaire à la place.

| Terme exclu | Utiliser à la place | Raison |
|-------------|--------------------|----|
| [Terme] | [Terme préféré] | [Pourquoi ce terme est confus] |
```


## Gestion des erreurs
> ❌ Prérequis manquant — vérifier les fichiers sources avant de relancer.

## Règles de qualité

- Le glossaire est **vivant** — mis à jour à chaque nouveau terme introduit dans les specs
- Un terme sans définition dans le glossaire ne doit pas apparaître dans les specs
- Si deux agents utilisent des termes différents pour la même chose → arbitrage BA → glossaire mis à jour
- Les termes exclus sont aussi importants que les termes définis

## Chemin de fichier
```
specs/[epic-slug]/glossaire.md
```


## Phase de validation — niveau léger

Avant de clore ce skill, confirme ce point :

1. Tous les termes ambigus du Brief Fonctionnel sont-ils couverts ? (oui / non + termes manquants)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-glossaire "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/glossaire.md

Prochaines étapes :
→ /write-brief-fonctionnel $ARGUMENTS
```
