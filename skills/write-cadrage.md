---
name: write-cadrage
description: Pose les 6 questions de cadrage en une fois et structure les réponses en résumé de cadrage. Exécuté par le business-analyst.
argument-hint: [epic-slug]
disable-model-invocation: false
context: fork
agent: business-analyst
---

## Rôle
Poser les 6 questions de cadrage en une seule fois, structurer les réponses et déclencher la production des premiers livrables BA.

## Agents consommateurs
- Business Analyst (pilote)
- Product Owner (contributeur)

## Quand utiliser ce skill
- Au démarrage d'un nouveau projet ou feature
- Quand un besoin brut arrive depuis le `backlog/` (item `Validated`)
- Quand des specs existantes sont incomplètes ou ambiguës (trigger élargi du BA)

## Processus

### Étape 1 — Poser les 6 questions en une fois

Présenter toujours ce bloc complet, sans en omettre aucune, sans attendre les réponses une par une.

```markdown
## Cadrage initial — [Nom du projet / feature]

Pour produire des spécifications exploitables, j'ai besoin de réponses à ces 6 questions.
Réponds à toutes en une fois — des réponses courtes suffisent pour démarrer.

**1. Qui** utilise cette fonctionnalité ?
→ Décris le ou les types d'utilisateurs : rôle, contexte, niveau technique.
→ Ex: "Des managers terrain, sur mobile, souvent en déplacement"

**2. Quand** est-ce utilisé ?
→ Fréquence, moment dans le parcours, contexte d'usage.
→ Ex: "Quotidiennement, en début de journée, avant les réunions"

**3. Quoi** doit-il se passer exactement ?
→ Décris le flux attendu, étape par étape.
→ Ex: "L'utilisateur ouvre l'app, voit sa liste de tâches, peut en marquer comme faites"

**4. Pourquoi** ce besoin ou cette règle existe-t-il ?
→ Origine métier, contrainte légale, décision produit.
→ Ex: "Les managers perdent du temps à chercher leurs tâches dans leurs emails"

**5. Qu'est-ce qui peut mal tourner ?**
→ Edge cases, erreurs réseau, permissions, données manquantes.
→ Ex: "Pas de connexion, liste vide au premier lancement, tâches en retard"

**6. Comment mesure-t-on le succès ?**
→ KPIs, métriques cibles, critères d'acceptance.
→ Ex: "80% des managers utilisent la fonctionnalité au moins 3x par semaine"
```

### Étape 2 — Analyser les réponses

Une fois les réponses reçues, vérifier avant de produire quoi que ce soit :

| Question | Vérification |
|----------|-------------|
| Qui | Au moins 1 persona identifiable — déclencher `write-persona` |
| Quand | Contexte mobile précisé (iOS / Android, fréquence, environnement) |
| Quoi | Flux principal describable en 3-5 étapes — déclencher `write-flux-fonctionnel` |
| Pourquoi | Problème métier clair — pas une solution déguisée |
| Mal tourner | Au moins 2 edge cases identifiés |
| Succès | Au moins 1 KPI mesurable |

### Étape 3 — Identifier les questions ouvertes

Si une réponse est manquante ou insuffisante :
- Ne pas bloquer la production — noter comme question ouverte dans le Brief
- Signaler explicitement à l'interlocuteur quelle question nécessite plus de précision
- Reprendre uniquement les éléments bloquants avant de produire le Brief Fonctionnel

### Étape 4 — Déclencher les livrables

Depuis les réponses au Cadrage initial, produire dans cet ordre :

```
1. PERSONA-###         ← depuis Q1 (write-persona)
2. FLUX-###            ← depuis Q3 (write-flux-fonctionnel)
3. Brief Fonctionnel   ← depuis Q1 à Q6 (write-brief-fonctionnel)
```

Le Brief Fonctionnel ne peut pas être produit avant que les flux et personas soient au moins en `Draft`.

## Format du résumé de cadrage

Après réception des réponses, produire ce résumé avant de démarrer les livrables :

```markdown
## Résumé du Cadrage — [Nom du projet / feature]

### Ce qu'on a compris
- **Utilisateurs** : [Résumé Q1]
- **Contexte d'usage** : [Résumé Q2]
- **Flux principal** : [Résumé Q3 en 1 phrase]
- **Problème résolu** : [Résumé Q4]

### Stack technique
- **Type d'application** : [Native iOS / Native Android / Native iOS + Android / Cross-platform (React Native, Flutter) / Hybride / Web mobile]
- **Plateformes cibles** : [iOS / Android / Les deux]
- **Versions minimum** : [iOS XX+ / Android XX+]
- **Contraintes techniques connues** : [ex: biométrie, widgets, offline, notifications push]

> Cette information est critique — elle détermine les guidelines applicables (HIG pour iOS natif, Material 3 pour Android natif, conventions propres pour cross-platform) et les conventions de code des skills UI.

### Périmètre V1 pressenti
- [Fonctionnalité 1]
- [Fonctionnalité 2]

### Questions ouvertes
| # | Question | Bloquant ? | Responsable |
|---|----------|-----------|-------------|
| 1 | [Question sans réponse] | [Oui / Non] | [Qui doit répondre] |

### Prochaines étapes
- [ ] Créer [PERSONA-###] — [slug]
- [ ] Créer [FLUX-###] — [nom du flux]
- [ ] Produire le Brief Fonctionnel [EPIC-###]
```


## Gestion des erreurs

Si les prérequis ne sont pas remplis :
> ❌ Prérequis manquant — vérifier les fichiers sources avant de relancer.

Si un fichier source est introuvable via chemin relatif, utiliser Glob avec le pattern `**/specs/$ARGUMENTS/**` pour localiser les fichiers depuis leur chemin absolu.

## Règles de qualité

- Les 6 questions sont **toujours posées en une seule fois** — jamais une par une
- Une réponse vague est préférable à une absence de réponse — démarrer avec ce qu'on a
- Le cadrage ne produit pas de specs — il produit des inputs pour les skills de production
- Si le besoin vient du `backlog/`, vérifier que l'item est en statut `Validated` avant de démarrer


## Phase de validation — niveau approfondi

Avant de valider ce livrable et de passer à la suite, réponds à ces questions dans l'ordre :

1. Les 6 questions ont-elles toutes reçu une réponse — même partielle ? (oui / non + questions sans réponse)
2. Le problème métier est-il clairement identifié depuis la réponse Q4 — pas une feature déguisée ? (oui / reformuler)
3. Le contexte mobile est-il précisé — ios, android ou les deux, fréquence, environnement d'usage ? (oui / non)
4. Au moins un KPI mesurable a-t-il été identifié depuis la réponse Q6 ? (oui / non + proposer un KPI)
5. ✅ NOUVEAU [2] — La stack technique est-elle définie — type d'application (native, cross-platform, hybride), plateformes et versions minimum ? (oui / non — bloquant pour les étapes UI)

### Gate de validation humaine — obligatoire
Le résumé de cadrage doit être relu et validé par le **Product Designer** avant de lancer les étapes suivantes.

> "Product Designer — confirmes-tu le résumé de cadrage ci-dessus, en particulier la stack technique et le périmètre V1 pressenti ?"

Le process ne démarre pas sans cette confirmation explicite.

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-cadrage "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/cadrage.md

Prochaines étapes :
→ /write-persona $ARGUMENTS
→ /write-flux-fonctionnel $ARGUMENTS
→ /write-brief-fonctionnel $ARGUMENTS
```
