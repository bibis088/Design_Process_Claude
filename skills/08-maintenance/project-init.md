---
name: project-init
description: "Point d'entrée de tout nouveau projet — le Product Designer définit quelles étapes il veut réaliser manuellement vs déléguer aux agents, avant de démarrer. Redemande entre chaque groupe d'étapes si les choix initiaux doivent être adaptés. Use when user says 'nouveau projet', 'démarrer le project', 'on démarre', 'lancer le process', 'initialise le projet', 'start project', or 'commence'."
argument-hint: "[project-slug]"
agent: business-analyst
---

## Rôle
Poser les questions d'initialisation une seule fois — nom du projet, choix des étapes manuelles vs automatiques — puis orchestrer le process en respectant ces choix, avec un checkpoint entre chaque groupe d'étapes pour permettre l'adaptation.

## Prérequis
Aucun — c'est le point de départ absolu.
---
- [ ] Brief initial ou description du projet disponible
- [ ] Accès web · MCP Figma connecté

## Étape 1 — Collecter les informations de base

```
Bonjour. Avant de démarrer le process, j'ai besoin de trois informations.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

QUESTION 1 — Nom et contexte du projet

  - Nom du projet : [...]
  - Secteur d'activité : [...]
  - Type de livrable : [Application mobile iOS / Android / Les deux / Autre]
  - Client ou usage interne : [Client externe / Usage interne]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

QUESTION 2 — Périmètre du process

Quels groupes et étapes souhaitez-vous inclure dans ce projet ?
Indiquez ✅ Inclus ou ⏭️ Ignoré pour chaque ligne.

┌─────────────────────────────────────────────────────────────────┐
│ GROUPE 1 — Research                    [ ✅ Inclus / ⏭️ Ignoré ]│
│   1a. Research stratégique             [ ✅ / ⏭️ ]              │
│   1b. Research utilisateur (interviews)[ ✅ / ⏭️ ]              │
│   1c. Discovery interviews             [ ✅ / ⏭️ ]              │
├─────────────────────────────────────────────────────────────────┤
│ GROUPE 2 — Cadrage et Spécification    [ ✅ Inclus / ⏭️ Ignoré ]│
│   2a. Cadrage + Personas               [ ✅ / ⏭️ ]              │
│   2b. Brief fonctionnel + Flux         [ ✅ / ⏭️ ]              │
│   2c. User Stories                     [ ✅ / ⏭️ ]              │
├─────────────────────────────────────────────────────────────────┤
│ GROUPE 3 — Setup Figma                 [ ✅ Inclus / ⏭️ Ignoré ]│
├─────────────────────────────────────────────────────────────────┤
│ GROUPE 4 — UX Design                   [ ✅ Inclus / ⏭️ Ignoré ]│
│   4a. Navigation map + User flows      [ ✅ / ⏭️ ]              │
│   4b. Specs écrans                     [ ✅ / ⏭️ ]              │
│   4c. Accessibilité RAAM               [ ✅ / ⏭️ ]              │
├─────────────────────────────────────────────────────────────────┤
│ GROUPE 5 — UI Design                   [ ✅ Inclus / ⏭️ Ignoré ]│
│   5a. Composants + Guidelines          [ ✅ / ⏭️ ]              │
│   5b. Direction artistique (toujours ✅)[ ✅ ]                   │
│   5c. Prototype React                  [ ✅ / ⏭️ ]              │
│   5d. Écrans secondaires + états       [ ✅ / ⏭️ ]              │
├─────────────────────────────────────────────────────────────────┤
│ GROUPE 6 — Validation et Livraison     [ ✅ Inclus / ⏭️ Ignoré ]│
│   6a. Tests utilisateur UI finale      [ ✅ / ⏭️ ]              │
│   6b. Plan de tagage                   [ ✅ / ⏭️ ]              │
│   6c. Documentation fonctionnelle      [ ✅ / ⏭️ ]              │
│   6d. Déclinaison OS secondaire        [ ✅ / ⏭️ ]              │
│   6e. Handoff Dev                      [ ✅ / ⏭️ ]              │
│   6f. QA                               [ ✅ / ⏭️ ]              │
│   6g. Assets store                     [ ✅ / ⏭️ ]              │
└─────────────────────────────────────────────────────────────────┘

Note : marquer un groupe ⏭️ ignore toutes ses étapes.
Les étapes ignorées peuvent être réactivées à n'importe quel checkpoint.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

QUESTION 2b — Découpage par sprint

Le process peut être exécuté pour l'ensemble du projet ou sprint par sprint.

  PROJET COMPLET — exécuter tout le process d'un bloc
  PAR SPRINT     — exécuter le process pour chaque sprint séparément
                   (chaque sprint a son propre scope US, ses propres gates,
                    sa propre revue client)

Si PAR SPRINT : combien de sprints sont prévus ?  [N sprints]
→ Le process sera initialisé sprint par sprint avec `/project-init [slug]-sprint-[N]`

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

QUESTION 3 — Mode d'exécution pour les groupes inclus

Pour chaque groupe ✅ Inclus, indiquez :
  🤖 Auto     — les agents réalisent et vous validez
  ✋ Manuel   — vous réalisez, les agents vérifient la DoD
  🔀 Mixte    — vous choisissez étape par étape au démarrage du groupe

┌─────────────────────────────────────────────────────────────────┐
│ GROUPE 1 — Research        [ 🤖 Auto / ✋ Manuel / 🔀 Mixte ]  │
│ GROUPE 2 — Cadrage         [ 🤖 Auto / ✋ Manuel / 🔀 Mixte ]  │
│ GROUPE 3 — Setup Figma     [ 🤖 Auto / ✋ Manuel / 🔀 Mixte ]  │
│ GROUPE 4 — UX Design       [ 🤖 Auto / ✋ Manuel / 🔀 Mixte ]  │
│ GROUPE 5 — UI Design       [ 🤖 Auto / ✋ Manuel / 🔀 Mixte ]  │
│ GROUPE 6 — Validation      [ 🤖 Auto / ✋ Manuel / 🔀 Mixte ]  │
└─────────────────────────────────────────────────────────────────┘

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Étape 2 — Enregistrer les choix

Sauvegarder dans `specs/[project-slug]/project-config.md` :

```markdown
# Configuration projet — [NomProjet] — [YYYY-MM-DD]

## Contexte
- Nom : [NomProjet]
- Slug : [project-slug]
- Secteur : [Secteur]
- Type : [iOS / Android / Les deux]
- Client : [Client externe / Usage interne]

## Périmètre du process

| Groupe | Étape | Inclus |
|--------|-------|--------|
| 1 | 1a. Research stratégique | ✅ / ⏭️ |
| 1 | 1b. Research utilisateur | ✅ / ⏭️ |
| 1 | 1c. Discovery interviews | ✅ / ⏭️ |
| 2 | 2a. Cadrage + Personas | ✅ / ⏭️ |
| 2 | 2b. Brief fonctionnel + Flux | ✅ / ⏭️ |
| 2 | 2c. User Stories | ✅ / ⏭️ |
| 3 | Setup Figma complet | ✅ / ⏭️ |
| 4 | 4a. Navigation map + Flows | ✅ / ⏭️ |
| 4 | 4b. Specs écrans | ✅ / ⏭️ |
| 4 | 4c. Accessibilité RAAM | ✅ / ⏭️ |
| 5 | 5a. Composants + Guidelines | ✅ / ⏭️ |
| 5 | 5b. Direction artistique | ✅ (toujours) |
| 5 | 5c. Prototype React | ✅ / ⏭️ |
| 5 | 5d. Écrans secondaires + états | ✅ / ⏭️ |
| 6 | 6a. Tests utilisateur UI | ✅ / ⏭️ |
| 6 | 6b. Plan de tagage | ✅ / ⏭️ |
| 6 | 6c. Documentation fonctionnelle | ✅ / ⏭️ |
| 6 | 6d. Déclinaison OS secondaire | ✅ / ⏭️ |
| 6 | 6e. Handoff Dev | ✅ / ⏭️ |
| 6 | 6f. QA | ✅ / ⏭️ |
| 6 | 6g. Assets store | ✅ / ⏭️ |

## Mode d'exécution

| Groupe | Mode initial | Modifié le | Mode actuel |
|--------|-------------|-----------|------------|
| 1 — Research | [Auto/Manuel/Mixte] | — | [idem] |
| 2 — Cadrage | [Auto/Manuel/Mixte] | — | [idem] |
| 3 — Setup Figma | [Auto/Manuel/Mixte] | — | [idem] |
| 4 — UX Design | [Auto/Manuel/Mixte] | — | [idem] |
| 5 — UI Design | [Auto/Manuel/Mixte] | — | [idem] |
| 6 — Validation | [Auto/Manuel/Mixte] | — | [idem] |

## Historique des adaptations

| Date | Checkpoint | Groupe | Étape | Changement |
|------|-----------|--------|-------|-----------|
| — | — | — | — | — |
```

---

## Étape 3 — Démarrer le premier groupe

Démarrer le Groupe 1 selon le mode choisi (voir section "Orchestration par groupe" ci-dessous).

---

## Orchestration par groupe

### Principe général

Avant chaque groupe :
1. Afficher le **checkpoint** (voir template ci-dessous)
2. Demander si les choix initiaux sont confirmés ou à adapter
3. Si adaptation → mettre à jour `project-config.md`
4. Exécuter le groupe selon le mode final

---

### GROUPE 1 — Research et Discovery

**Étapes :**
```
/write-discovery-interview [project-slug]   ← en parallèle
/write-competitive-analysis [project-slug]
/write-sector-watch [project-slug]
/write-ux-benchmark [project-slug]
```

**Si mode Manuel — brief pour le Product Designer :**

```markdown
## 📋 Groupe 1 — Research et Discovery [Manuel]

### Ce que vous devez produire

**Research stratégique (auto)**
- Analyse concurrentielle des apps existantes dans le secteur
- Veille sectorielle et réglementaire
- Benchmark UX/UI des meilleures pratiques du domaine

**Research utilisateur (semi-auto — vous conduisez les sessions)**
- Plan de recherche + guide d'entretien
- Sessions d'interviews avec de vrais utilisateurs (minimum 5 participants)
- Synthèse des insights

**Discovery interviews (semi-auto — vous conduisez les sessions)**
- Interviews ouvertes sur les besoins larges au-delà du MVP
- Vocabulaire utilisateur réel

> ⚠️ Tout ce research doit être terminé AVANT de passer au Groupe 2 (cadrage).
> Le cadrage sera directement nourri par les insights terrain.

### Éléments indispensables
- Minimum 5 participants interviewés (research utilisateur)
- Minimum 5 interviews de découverte (3 profils différents)
- 3 à 5 concurrents analysés avec captures et observations
- Points de conformité légale identifiés
- 3 patterns UX retenus comme référence pour ce projet

### Objectifs de cette phase
1. Comprendre les besoins réels avant toute décision de scope
2. Valider ou invalider les hypothèses du brief client
3. Produire des personas basés sur la réalité terrain (pas des hypothèses)
4. Identifier les opportunités V2+ pour la roadmap

### DoD — Critères de sortie

Pour passer au Groupe 2 (cadrage), les éléments suivants doivent être disponibles :
- [ ] `specs/[project-slug]/research/competitive-analysis.md` — analyse 3+ concurrents
- [ ] `specs/[project-slug]/research/sector-watch.md` — réglementations et tendances
- [ ] `specs/[project-slug]/research/ux-benchmark.md` — patterns de référence
- [ ] `specs/[project-slug]/research/research-plan.md` + `interview-guide.md`
- [ ] `specs/[project-slug]/research/insights.md` — synthèse interviews utilisateurs
- [ ] `specs/[project-slug]/research/discovery-interviews.md` — besoins larges + vocabulaire
- [ ] Vocabulaire utilisateur transmis pour le glossaire
- [ ] Opportunités V2+ identifiées pour la roadmap

Prévenez-moi quand ces fichiers sont prêts pour passer au Groupe 2.
```

**Gate de sortie Groupe 1 :**
→ Validation Product Designer → **Checkpoint 1**

---

### CHECKPOINT entre chaque groupe

Template de checkpoint à afficher systématiquement avant de démarrer le groupe suivant :

```markdown
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ GROUPE [N] terminé — CHECKPOINT [N]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Prochaine étape : GROUPE [N+1] — [Nom du groupe]

Étapes prévues :
  [liste des étapes ✅ incluses]

Étapes ignorées (⏭️) :
  [liste des étapes ignorées au démarrage — peuvent être réactivées maintenant]

Mode configuré : [Auto / Manuel / Mixte]

Souhaitez-vous adapter avant de continuer ?

  ✅ Continuer tel quel
  🔄 Changer le mode :
     → 🤖 Auto / ✋ Manuel / 🔀 Mixte
  ➕ Réactiver une étape ignorée
  ⏭️ Ignorer une étape supplémentaire

[Attendez votre réponse avant de continuer]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

Toute adaptation est enregistrée dans `project-config.md` — tableau `Historique des adaptations`.

---

### GROUPE 2 — Cadrage et Spécification

**Étapes :**
```
/write-cadrage [epic-slug]
/write-persona [epic-slug]
/write-flux-fonctionnel [epic-slug]/[flux]
/write-regles-metier [epic-slug]
/write-glossaire [epic-slug]
/write-brief-fonctionnel [epic-slug]
/write-feature-ticket [epic-slug]/[feature]
/write-user-story [epic-slug]/[story]
```

**Si mode Manuel — brief pour le Product Designer :**

```markdown
## 📋 Groupe 2 — Cadrage et Spécification [Manuel]

### Ce que vous devez produire
- Réponses aux 10 questions de cadrage (6 fonctionnelles + 4 contractuelles)
- 3 personas (regular-user, new-user, edge-user)
- Flux fonctionnels détaillés
- Règles métier et glossaire
- Brief fonctionnel EPIC
- Features et user stories avec critères d'acceptance

### Éléments indispensables
- Cadre contractuel des itérations défini (bloquant)
- Stack technique identifiée (iOS/Android/cross-platform)
- Au moins 1 KPI mesurable par objectif
- Chaque user story couvre un seul besoin utilisateur
- Les CA sont testables et observables

### Objectifs
1. Transformer le besoin brut en spécifications exploitables
2. Aligner équipe et client sur le périmètre V1
3. Créer la base de référence pour toutes les phases suivantes

### DoD — Critères de sortie

- [ ] `specs/[epic-slug]/cadrage.md` — 10 questions répondues
- [ ] `specs/[epic-slug]/personas/PERSONA-00[1-3]-*.md` — 3 personas Draft
- [ ] `specs/[epic-slug]/FLUX-###-*.md` × N — tous les flux identifiés
- [ ] `specs/[epic-slug]/regles-metier.md`
- [ ] `specs/[epic-slug]/glossaire.md`
- [ ] `specs/[epic-slug]/EPIC.md` — brief fonctionnel complet
- [ ] `specs/[epic-slug]/features/FEAT-###.md` × N
- [ ] `specs/[epic-slug]/stories/US-###.md` × N — CA définis

Prévenez-moi quand ces fichiers sont prêts pour passer au Groupe 3.
```

---

### GROUPE 3 — Setup Figma

Déléguer à `setup-figma-init` avec le mode choisi (A/B/C correspondent aux modes Auto/Mixte/Manuel).

```
/setup-figma-init [epic-slug]
→ Mode Auto = Mode A (automatique)
→ Mode Mixte = Mode B (mixte)
→ Mode Manuel = Mode C (humain + review)
```

**Si mode Manuel — brief pour le Product Designer :**

```markdown
## 📋 Groupe 3 — Setup Figma [Manuel]

### Ce que vous devez créer dans Figma

**Fichier Design System**
- Pages : 🎨 foundations / 🧩 components / 📐 patterns / 📖 documentation
- Variables : tokens couleurs (light + dark), spacing, radius, motion
- Text Styles : ios/* et android/* complets
- Effect Styles : ombres

**Fichier Projet**
- Pages : Userflow / Work In Progress UI / Sandbox / Ready for dev / Store screen
- Sections dans screens : une par US (nommée US_### en snake_case)
- Bibliothèque DS connectée

### Éléments indispensables
- Nomenclature snake_case stricte (aucun nom auto Figma)
- Modes light ET dark configurés sur les tokens sémantiques
- Grilles iOS (4 col, 16pt) et Android (4 col, 16dp) avec safe areas
- Bibliothèque DS publiée et connectée au fichier Projet

### Objectifs
1. Avoir une base de travail structurée, nommée et prête pour la production
2. Garantir que les tokens et styles sont partagés entre les fichiers
3. Éviter toute dette technique Figma dès le départ

### DoD — Critères de sortie

- [ ] Fichier DS publié avec variables, styles et composants de base
- [ ] Fichier Projet créé avec toutes les pages et sections US
- [ ] Bibliothèque DS connectée — variables accessibles dans le Projet
- [ ] Grilles iOS et Android actives sur les frames principales
- [ ] Nomenclature snake_case vérifiée — 0 nom auto Figma
- [ ] URLs des fichiers disponibles dans `specs/[epic-slug]/figma-urls.md`

Partagez les URLs des fichiers créés, je lancerai l'audit qualité automatiquement.
```

---

### GROUPE 4 — UX Design

**Étapes :**
```
/write-navigation-map [feature-slug]
/write-user-flow [feature-slug]/[flux]
/write-figma-userflow [feature-slug]/[flux]
/write-screen-spec [feature-slug]/[screen]
/write-accessibility-spec [feature-slug]
```

**Si mode Manuel — brief pour le Product Designer :**

```markdown
## 📋 Groupe 4 — UX Design [Manuel]

### Ce que vous devez produire
- Navigation map — architecture de l'app
- User flows détaillés pour chaque FLUX-###
- Flows visuels dans Figma (page `Userflow`)
- Specs textuelles de chaque écran
- Spec d'accessibilité RAAM

### Éléments indispensables
- Chaque US-### a au moins un écran spécifié
- Les états dégradés sont documentés (loading, empty, error)
- Les flows couvrent les chemins alternatifs et d'erreur
- La navigation map est cohérente avec les flux fonctionnels

### Objectifs
1. Définir l'architecture de navigation avant de toucher à l'UI
2. Documenter l'intention UX de chaque écran
3. Préparer la base pour la direction artistique

### DoD — Critères de sortie

- [ ] `design/[feature-slug]/SCREENS-MAP.md` — navigation map
- [ ] `design/[feature-slug]/ux/user-flow-FLUX-###.md` × N
- [ ] Flows visuels dans Figma page `Userflow`
- [ ] `design/[feature-slug]/ux/screens/S-XX-*.md` × N — specs écrans
- [ ] `design/[feature-slug]/ux/accessibility-spec.md` — RAAM

Prévenez-moi quand ces fichiers sont prêts pour passer au Groupe 5.
```

---

### GROUPE 5 — UI Design

**Note :** La direction artistique est **toujours manuelle** — signalée explicitement.

**Étapes :**
```
/setup-figma-frames [feature-slug]          ← frames vides
/create-figma-component [feature-slug]/[c]  ← composants × N
/check-guidelines-compliance [feature-slug] ← vérification
[MANUEL — Direction artistique]             ← toujours humain
/write-accessibility-annotations [feature]  ← annotations
/setup-figma-prototype [feature-slug]/[f]   ← prototype natif
/write-prototype-react [feature-slug]/[f]   ← prototype HTML
```

**Si mode Manuel — brief pour le Product Designer :**

```markdown
## 📋 Groupe 5 — UI Design [Manuel]

### Ce que vous devez créer dans Figma

**Phase composants**
- Tous les composants nécessaires aux US (depuis les specs écrans)
- Chaque composant avec ses variants et états (default, hover, disabled, loading, error)
- Conformité HIG (iOS) et Material 3 (Android) vérifiée

**Direction artistique** *(toujours manuelle)*
- Identité visuelle : couleurs, typographie, ton, ambiance
- Écrans principaux du happy path créés depuis zéro
- Cohérence avec la direction de marque du client

**Écrans secondaires et états dégradés**
- États loading, empty, error pour chaque écran principal
- Écrans de gestion d'erreur globaux
- Contenu réel injecté (pas de Lorem ipsum)

### Éléments indispensables
- Nomenclature snake_case sur tous les layers et frames
- Aucune valeur en dur — tokens DS uniquement
- Auto Layout activé sur tous les composants
- Zones tactiles ≥ 44pt iOS / 48dp Android

### Objectifs
1. Produire des écrans fidèles aux specs UX avec l'identité visuelle du projet
2. Couvrir tous les états et flux définis dans les US
3. Préparer un handoff Dev de qualité

### DoD — Critères de sortie

- [ ] Tous les composants créés dans Figma DS
- [ ] Écrans principaux (happy path) créés par le Product Designer
- [ ] États dégradés (loading, empty, error) pour chaque écran
- [ ] Annotations RAAM sur tous les éléments interactifs
- [ ] Statut des frames : `ready_for_dev` ou `in_progress`
- [ ] Prototype Figma natif connecté (setup-figma-prototype)
- [ ] Prototype React généré (write-prototype-react)
- [ ] 0 valeur hex en dur dans les composants
- [ ] 0 layer nommé automatiquement (Rectangle, Group, Frame + chiffre)

Prévenez-moi quand c'est terminé, je lance la vérification guidelines et la revue de scope.
```

---

### GROUPE 6 — Validation et Livraison

**Étapes :**
```
/write-user-test-scenarios [epic-slug]/[feature]
/run-user-tests [epic-slug]/[feature]
/write-tracking-plan [epic-slug]/[feature]
/review-tracking-plan [epic-slug]/[feature]
/review-figma-scope [epic-slug]/[feature]
/write-functional-doc [epic-slug]
/write-figma-handoff [feature-slug]
/write-store-assets [epic-slug]
/write-qa-report [epic-slug]/[us]
```

**Si mode Manuel — brief pour le Product Designer :**

```markdown
## 📋 Groupe 6 — Validation et Livraison [Manuel]

### Ce que vous devez réaliser

**Tests utilisateur**
- Scénarios de test définis depuis les flux et frames Figma
- Sessions modérées avec 11 participants (5 regular / 3 new / 3 edge)
- Enregistrements audio/vidéo uploadés sur Drive
- Synthèse des résultats

**Plan de tagage**
- Événements définis pour chaque KPI
- Vérification que chaque événement a un élément visuel correspondant
- Adaptation si les choix UI ont modifié des déclencheurs

**Documentation et handoff**
- Documentation fonctionnelle complète
- Handoff Dev avec annotations, specs et tokens
- Assets store si publication

### Éléments indispensables
- Chaque KPI a au moins 1 événement de tracking
- Chaque événement est mappé sur un écran Figma avec URL directe
- Le rapport de tests couvre au moins 1 friction avec citation + donnée chiffrée

### DoD — Critères de sortie

- [ ] `specs/[epic-slug]/research/user-test-report.md`
- [ ] `specs/[epic-slug]/[feature]/tracking-plan.md` — révisé post-UI
- [ ] `specs/[epic-slug]/functional-doc-v[X].md`
- [ ] `design/[feature-slug]/handoff/` — complet
- [ ] Rapport QA disponible
- [ ] Livrable client préparé (write-client-deliverable)

Prévenez-moi quand tout est prêt pour la Gate 9.
```

---

## Gestion des adaptations (checkpoint)

Quand le Product Designer demande à changer un mode en cours de route :

```markdown
## Adaptation enregistrée — Checkpoint [N]

- Groupe : [N — Nom]
- Mode précédent : [Auto / Manuel / Mixte]
- Nouveau mode : [Auto / Manuel / Mixte]
- Raison (si précisée) : [...]
- Date : [YYYY-MM-DD]

Mise à jour de `specs/[project-slug]/project-config.md` ✅
```

## Résumé de fin d'exécution

```
✅ project-init "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/project-config.md

Groupe 1 démarré — mode : [Auto / Manuel / Mixte]

Note : un checkpoint sera présenté avant chaque groupe
pour adapter les choix si nécessaire.

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```
