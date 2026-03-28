---
name: write-regles-metier
description: "Formalise une matrice de règles métier RB-### exploitable par PO, Dev et QA. Exécuté par le business-analyst."
argument-hint: "[epic-slug]"
agent: business-analyst
---

## Rôle
Formaliser une matrice de règles métier `RB-###` exploitable par le PO, le Dev et le QA.

## Agents consommateurs
Business Analyst  · Product Owner 

## Prérequis
- [ ] Les 6 questions du Cadrage initial ont reçu une réponse
- [ ] Le Brief Fonctionnel est au moins en `Draft`
- [ ] Les flux fonctionnels correspondants sont identifiés

## Processus de génération

### Étape 1 — Identifier les règles
Depuis les réponses au Cadrage initial et le Brief Fonctionnel, extraire :
- Toute contrainte qui s'applique indépendamment du flux ("toujours", "jamais", "seulement si")
- Toute règle de validation (format, plage de valeurs, unicité)
- Toute contrainte légale ou réglementaire
- Toute décision métier qui impacte le comportement du système

### Étape 2 — Qualifier chaque règle
Chaque règle doit avoir :
- Une **source** : qui l'a définie et pourquoi
- Une **priorité** : Critique (bloquant MVP) / Haute / Moyenne
- Un **impact** : où elle s'applique dans les flux et les composants

### Étape 3 — Rédiger la matrice

```markdown
# Règles Métier — [EPIC-###] [Nom de l'épic]

## Métadonnées
- Epic : [EPIC-###]
- Business Analyst : [nom]
- Statut : [Draft / In Review / Approved]
- Dernière mise à jour : [YYYY-MM-DD]

## Matrice des règles

| ID | Règle | Priorité | Source | Impact | Flux concernés |
|----|-------|----------|--------|--------|---------------|
| RB-001 | [Énoncé précis et non ambigu] | Critique | [Qui / quoi] | [Composant / écran] | [FLUX-###] |
| RB-002 | [Énoncé précis et non ambigu] | Haute | [Qui / quoi] | [Composant / écran] | [FLUX-###] |

## Détail des règles critiques

### [RB-001] — [Titre court]
**Énoncé** : [Formulation complète et non ambiguë]
**Origine** : [Contrainte légale / Décision métier / Contrainte technique]
**Source** : [Qui l'a définie — client, PO, légal]
**Exemples** :
- ✅ Cas conforme : [exemple]
- ❌ Cas non conforme : [exemple]
**Critère d'acceptance associé** : [CA-## de la US-###]
**Impact sur les tests** : [Ce que le QA doit vérifier]

### [RB-002] — [Titre court]
[Même structure]

## Règles dépréciées
| ID | Règle | Remplacée par | Date |
|----|-------|--------------|------|
| [RB-###] | [Ancienne règle] | [RB-###] | [YYYY-MM-DD] |
```


## Gestion des erreurs
> ❌ Prérequis manquant — vérifier les fichiers sources avant de relancer.

## Règles de qualité

- Une règle métier est **immuable dans le contexte du projet** — si elle change, créer une nouvelle version
- L'énoncé doit être **testable** : éviter "le système doit être rapide", préférer "le chargement doit être < 2s"
- Chaque règle critique a obligatoirement un exemple conforme ET non conforme
- Les règles dépréciées ne sont jamais supprimées — elles sont archivées avec leur remplaçante

## Chemin de fichier
```
specs/[epic-slug]/regles-metier.md
```


## Phase de validation — niveau léger

Avant de clore ce skill, confirme ce point :

1. Chaque règle a-t-elle une source identifiée et une priorité définie ? (oui / non + règles à compléter)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-regles-metier "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/regles-metier.md

Prochaines étapes :
→ /write-user-story $ARGUMENTS
→ /review-dod RB-###
```
