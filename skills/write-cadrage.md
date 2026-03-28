---
name: write-cadrage
description: "Pose les 10 questions de cadrage en une fois — fonctionnelles et contractuelles — et structure les réponses en résumé de cadrage. Exécuté par le business-analyst."
argument-hint: "[epic-slug]"
agent: business-analyst
---

## Rôle
Poser les 10 questions de cadrage (6 fonctionnelles + 4 contractuelles) en une seule fois, structurer les réponses et déclencher la production des premiers livrables BA.

## Agents consommateurs
Business Analyst  · Product Owner 

## Quand utiliser ce skill
- Au démarrage d'un nouveau projet ou feature
- Quand un besoin brut arrive depuis le `backlog/` (item `Validated`)
- Quand des specs existantes sont incomplètes ou ambiguës (trigger élargi du BA)

## Processus

### Étape 1 — Poser les 10 questions en une fois

Présenter toujours ce bloc complet, sans en omettre aucune, sans attendre les réponses une par une.

```markdown
## Cadrage initial — [Nom du projet / feature]

Pour produire des spécifications exploitables, j'ai besoin de réponses à ces 10 questions (6 fonctionnelles + 4 contractuelles).
Réponds à toutes en une fois — des réponses courtes suffisent pour démarrer.

**Questions 1-6 — Fonctionnel**

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

**Questions 7-10 — Contractuel**

**7. Quel est le cadre contractuel des itérations client ?**
→ Nombre de cycles de retours client inclus dans le contrat (prototype, maquettes).
→ Délai de réponse client attendu par cycle.
→ Procédure si les retours dépassent le nombre de cycles contractuels.
→ Ex: "2 cycles de retours sur le prototype, 5 jours ouvrés par cycle, au-delà = avenant"

**8. Quels sont les livrables contractuels exacts ?**
→ Liste des documents et fichiers inclus dans le contrat.
→ Format de livraison attendu par le client.
→ Ce qui est explicitement hors contrat.
→ Ex: "Figma + specs fonctionnelles + prototype React. Hors contrat : code natif, publication store."

**9. Quelles sont les obligations d'accessibilité et de conformité légale ?**
→ Niveau d'accessibilité requis contractuellement (RAAM A, AA, WCAG...).
→ Réglementations sectorielles applicables (RGPD, santé, finance, éducation...).
→ Obligation de conformité avec date butoir contractuelle.
→ Ex: "RAAM niveau AA requis — secteur public. RGPD : consentement cookies obligatoire."

**10. Quel est le cadre de garantie et de maintenance post-livraison ?**
→ Durée de garantie après livraison finale.
→ Périmètre de la garantie (corrections bugs design, mises à jour guidelines...).
→ Qui maintient le design system après la fin du projet.
→ Ex: "Garantie 30 jours sur les livrables. Design system maintenu par le client au-delà."
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
| **Itérations** | **Nombre de cycles contractuels défini — bloquant si absent** |
| **Livrables** | **Liste et format des livrables confirmés — bloquant si absent** |
| **Accessibilité** | **Niveau RAAM/WCAG requis défini — détermine les critères de sortie** |
| **Garantie** | **Durée de garantie et conditions de transfert documentées** |
| **Livrables** | **Liste des livrables inclus/exclus confirmée — bloquant si absent** |
| **Accessibilité** | **Niveau requis contractuellement identifié — bloquant si absent** |
| **Garantie** | **Durée et périmètre de garantie définis — bloquant si absent** |

### Étape 3 — Identifier les questions ouvertes

Si une réponse est manquante ou insuffisante :
- Ne pas bloquer la production — noter comme question ouverte dans le Brief
- Signaler explicitement à l'interlocuteur quelle question nécessite plus de précision
- Reprendre uniquement les éléments bloquants avant de produire le Brief Fonctionnel

### Étape 4 — Déclencher les livrables

Depuis les réponses au Cadrage initial, produire dans cet ordre :

```
1. PERSONA-001/002/003 ← depuis Q1 (write-persona — génère les 3)
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

### Cadre contractuel des itérations

> ⚠️ Cette section est **contractuelle** — elle fait référence lors de tout dépassement du nombre de cycles convenu.

| Livrable | Cycles inclus | Délai de réponse client | Au-delà du contrat |
|----------|--------------|------------------------|-------------------|
| Prototype React | [N cycles] | [N jours ouvrés] | [Avenant / Facturation complémentaire / À définir] |
| Maquettes Figma | [N cycles] | [N jours ouvrés] | [Procédure] |
| Specs fonctionnelles | [N cycles] | [N jours ouvrés] | [Procédure] |

**Procédure si dépassement :**
[Décrire la procédure contractuelle — ex: "Avenant signé avant toute itération supplémentaire"]

**Décision en cas de dépassement :**
PO + Product Designer (côté prestation) statuent sur chaque demande :
- Appliquer dans le cadre du contrat existant si l'effort est mineur
- Déclencher un avenant si l'effort est significatif
- Reporter en V2 si la demande sort du scope contractuel

### Livrables contractuels

> ⚠️ Cette section est **contractuelle** — référence en cas de désaccord sur le périmètre des livrables.

**Inclus dans le contrat :**
| Livrable | Format de livraison | Critère d'acceptation |
|----------|--------------------|-----------------------|
| [Livrable 1] | [Figma / Google Doc / HTML / autre] | [Condition de validation] |
| [Livrable 2] | [Format] | [Condition] |

**Explicitement hors contrat :**
- [Livrable exclu 1 — ex: assets store]
- [Livrable exclu 2 — ex: design system complet]

### Exigences d'accessibilité et conformité légale

> ⚠️ Cette section est **contractuelle** — détermine les critères de sortie de chaque livrable.

**Niveau d'accessibilité requis :**
- Référentiel : [RAAM / WCAG 2.1 / WCAG 2.2]
- Niveau : [A / AA / AAA]
- Audit tiers prévu : [Oui / Non — si oui, par qui et quand]

**Réglementations applicables :**
| Réglementation | Domaine | Obligation | Date butoir |
|---------------|---------|-----------|------------|
| RGPD | Données personnelles | [Obligatoire / Recommandé] | [Date ou N/A] |
| [RAAM] | Accessibilité | [Obligatoire / Recommandé] | [Date ou N/A] |
| [Autre] | [Domaine] | [Niveau] | [Date] |

**Impact sur le process :**
- Niveau RAAM/WCAG contractuel → critère bloquant dans `check-guidelines-compliance`
- Réglementations → intégrées dans `write-regles-metier` et `write-sector-watch`

### Garantie et maintenance post-livraison

> ⚠️ Cette section est **contractuelle** — référence en cas de demande de correction après livraison.

**Garantie :**
- Durée : [N jours / mois après la livraison finale]
- Périmètre : [Défauts de conception uniquement / Bugs implémentation / Autre]
- Conditions d'exclusion : [Modifications demandées par le client après livraison / Changements de scope]

**Transfert des livrables :**
- Figma : [Transfert du fichier dans la team client / Export des assets]
- Design system : [Transféré au client / Conservé par le prestataire / Licence d'usage]
- Propriété intellectuelle : [Cédée au client à la livraison finale / Licence d'usage]

**Maintenance post-livraison :**
- Incluse : [Oui — N mois / Non]
- Conditions : [Qui maintient le design system, à quel coût, selon quelle procédure]

### Livrables contractuels

> ⚠️ Cette section est **contractuelle** — toute demande de livrable non listé ci-dessous est hors contrat.

**Livrables inclus dans le contrat :**
- [ ] Specs fonctionnelles (BA + PO)
- [ ] Maquettes Figma (UX + UI)
- [ ] Prototype React interactif
- [ ] Design system (tokens + composants)
- [ ] Handoff Dev (Figma + documentation)
- [ ] [Autres livrables spécifiques]

**Hors contrat (explicitement exclus) :**
- [ ] Code natif iOS / Android
- [ ] Publication sur les stores
- [ ] [Autres exclusions]

**Format de livraison attendu par le client :**
[Google Drive / Figma / Repository Git / Autre]

### Accessibilité et conformité légale

> ⚠️ Cette section est **contractuelle** — le niveau d'accessibilité requis conditionne les critères de sortie QA.

| Obligation | Niveau requis | Date butoir | Source |
|-----------|--------------|-------------|--------|
| Accessibilité | [RAAM A / AA / WCAG AA / Aucune obligation contractuelle] | [Date si applicable] | [Contrat / Loi / Recommandation] |
| RGPD | [Consentement / Mentions légales / Autre] | [Date] | [Réglementation] |
| [Réglementation sectorielle] | [Obligation] | [Date] | [Source] |

**Impact sur le process :**
- Niveau RAAM/WCAG retenu → critère de sortie bloquant dans `write-qa-report`
- Réglementations → intégrées dans les règles métier (`write-regles-metier`)

### Garanties et maintenance post-livraison

> ⚠️ Cette section est **contractuelle** — définit les obligations après la livraison finale.

| Élément | Durée / Périmètre | Responsable après contrat |
|---------|------------------|--------------------------|
| Garantie corrections | [N jours après livraison] | [Prestataire] |
| Périmètre garantie | [Corrections liées aux specs livrées / Hors évolutions] | — |
| Design system | [Maintenu par prestataire / Transféré au client] | [Client / Prestataire] |
| Figma | [Accès transféré / Conservé / À définir] | [Client] |
| Documentation | [Mise à jour incluse N mois / Non incluse] | — |

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
> ❌ Prérequis manquant — vérifier les fichiers sources avant de relancer.

## Règles de qualité

- Les 6 questions sont **toujours posées en une seule fois** — jamais une par une
- Une réponse vague est préférable à une absence de réponse — démarrer avec ce qu'on a
- Le cadrage ne produit pas de specs — il produit des inputs pour les skills de production
- Si le besoin vient du `backlog/`, vérifier que l'item est en statut `Validated` avant de démarrer


## Phase de validation — niveau approfondi

Avant de valider ce livrable et de passer à la suite, réponds à ces questions dans l'ordre :

1. Les 10 questions ont-elles toutes reçu une réponse — même partielle ? (oui / non + questions sans réponse)
2. Le problème métier est-il clairement identifié depuis la réponse Q4 — pas une feature déguisée ? (oui / reformuler)
3. Le contexte mobile est-il précisé — ios, android ou les deux, fréquence, environnement d'usage ? (oui / non)
4. Au moins un KPI mesurable a-t-il été identifié depuis la réponse Q6 ? (oui / non + proposer un KPI)
5. ✅ NOUVEAU [2] — La stack technique est-elle définie — type d'application (native, cross-platform, hybride), plateformes et versions minimum ? (oui / non — bloquant pour les étapes UI)
6. ✅ NOUVEAU [contractuel] — Le cadre des itérations client est-il défini — nombre de cycles, délai de réponse, procédure en cas de dépassement ? (oui / non — **bloquant**)
7. ✅ NOUVEAU [contractuel] — Les livrables contractuels sont-ils listés — inclus ET exclus explicitement ? (oui / non — **bloquant** : risque de litige si non défini)
8. ✅ NOUVEAU [contractuel] — Le niveau d'accessibilité requis contractuellement est-il identifié — RAAM A, AA, WCAG, ou aucune obligation ? (oui / non — **bloquant** : conditionne les critères QA)
9. ✅ NOUVEAU [contractuel] — La durée et le périmètre de garantie post-livraison sont-ils définis ? (oui / non — **bloquant** : évite les ambiguïtés après livraison)

### Gate de validation humaine — obligatoire
Le résumé de cadrage doit être relu et validé par le **Product Designer** avant de lancer les étapes suivantes.

> "Product Designer — confirmes-tu le résumé de cadrage ci-dessus, en particulier les contraintes contractuelles (livrables, itérations, accessibilité, garantie) et la stack technique ?"

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
