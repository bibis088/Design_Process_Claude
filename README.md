# AI_Assisted_Design_Process — Guide du projet

Copie modifiée et enrichie de `Agent_skills_Joris`.
Chaque fichier modifié est annoté avec `✅ MODIFIÉ [X]` ou `✅ NOUVEAU [X]` — la lettre renvoie au tableau de référence en bas de ce document.

---

## 🚀 Guide de démarrage rapide

### Nouveau projet — par où commencer ?

```
─── RESEARCH STRATÉGIQUE (UX Researcher — auto + humain) ────

Étape 0a — Analyse concurrentielle
    /write-competitive-analysis [project-slug]
    → Collecte auto : concurrents, forces/faiblesses, opportunités
    → Produit : specs/[project-slug]/research/competitive-analysis.md

Étape 0b — Veille sectorielle + réglementations
    /write-sector-watch [project-slug]
    → Collecte auto : tendances, actus 6 mois, contraintes légales
    → Produit : specs/[project-slug]/research/sector-watch.md

Étape 0c — Benchmark UX/UI
    /write-ux-benchmark [project-slug]
    → Collecte auto : patterns secteur, conventions, différenciation
    → Produit : specs/[project-slug]/research/ux-benchmark.md

⚠️ Gate -1 — Product Designer valide le research stratégique avant cadrage

─── CADRAGE + SPÉCIFICATIONS (BA) ───────────────────────────

Étape 1 — Cadrage (BA)
    /write-cadrage [epic-slug]
    → Produit : specs/[epic-slug]/cadrage.md

Étape 2 — Personas (BA)
    /write-persona [epic-slug]/[persona-slug]
    → Produit : specs/[epic-slug]/personas/PERSONA-###-[slug].md

Étape 3 — Brief Fonctionnel (BA)
    /write-brief-fonctionnel [epic-slug]
    → Produit : specs/[epic-slug]/EPIC.md

Étape 4 — Flux fonctionnels (BA)
    /write-flux-fonctionnel [epic-slug]/[flux-slug]
    → Produit : specs/[epic-slug]/FLUX-###-[slug].md

Étape 5 — Règles métier (BA)
    /write-regles-metier [epic-slug]
    → Produit : specs/[epic-slug]/regles-metier.md

Étape 6 — User Stories (PO)
    /write-feature-ticket [epic-slug]/[feature-slug]
    /write-user-story [epic-slug]/[story-slug]
    → Produit : specs/[epic-slug]/features/ + stories/

─── FIGMA SETUP (DSM) ────────────────────────────────────────

Étape 7 — Structure fichiers Figma (DSM)
    /setup-figma-project [epic-slug]
    → Crée les fichiers Figma Projet + Design System

Étape 8 — Tokens Figma (DSM)
    /setup-figma-tokens [epic-slug]
    → Couleurs, typographies, spacing dans Figma

Étape 9 — Grilles Figma (DSM)
    /setup-figma-grid [epic-slug]
    → Frames de base iOS et Android avec grilles

─── RESEARCH UTILISATEUR (UX Researcher — Product Designer guidé) ─────

Étape 9a — Plan de recherche
    /write-research-plan [epic-slug]
    → Guide de recherche : méthodes, profils, calendrier
    → Produit : specs/[epic-slug]/research/research-plan.md

Étape 9b — Guide d'entretien
    /write-interview-guide [epic-slug]
    → Questions ouvertes, scénarios, protocole
    → Produit : specs/[epic-slug]/research/interview-guide.md

Étape 9c — [MANUEL — Product Designer] Sessions d'interviews
    → Le Product Designer recrute et conduit les interviews
    → Notes brutes transmises au UX Researcher

Étape 9d — Insights + personas validés
    /write-research-insights [epic-slug]
    → Synthèse insights, personas mis à jour, recommandations
    → Produit : specs/[epic-slug]/research/insights.md

⚠️ Gate 4a — Product Designer valide les insights avant de démarrer le UX Design

─── UX ───────────────────────────────────────────────────────

Étape 10 — Navigation map (UX)
    /write-navigation-map [feature-slug]
    → Produit : design/[feature-slug]/SCREENS-MAP.md

Étape 11 — User flows textuels (UX)
    /write-user-flow [feature-slug]/[flux-slug]
    → Produit : design/[feature-slug]/ux/user-flow-FLUX-###.md

Étape 12 — User flows visuels Figma (UX)
    /write-figma-userflow [feature-slug]/[flux-slug]
    → Figma Projet → page 🗺️ User Flows

Étape 13 — Specs écrans (UX)
    /write-screen-spec [feature-slug]/[screen-id]-[screen-slug]
    → Produit : design/[feature-slug]/ux/screens/S-XX-[nom].md

Étape 13b — Spec accessibilité RAAM (UX)
    /write-accessibility-spec [feature-slug]
    → Produit : design/[feature-slug]/ux/accessibility-spec.md

Étape 13c — Annotations accessibilité (UX)
    /write-accessibility-annotations [feature-slug]/[screen-id]
    → Produit : design/[feature-slug]/ux/screens/S-XX-annotations.md

─── UI ───────────────────────────────────────────────────────

Étape 14 — Lecture Figma avant écriture (UI)
    /figma-read-design [feature-slug]
    → Inventorie frames, tokens et composants existants

Étape 15 — Frames vides nommées (UI)
    /setup-figma-frames [feature-slug]
    → Figma Projet → page 📱 Screens — frames selon US + états

Étape 16 — Composants de base (UI)
    /create-figma-component [feature-slug]/[component-slug]
    → Composants récurrents : boutons, inputs, cards, navigation...
    → Basés sur les tokens du design system

Étape 17 — Conformité guidelines composants (UI)
    /check-guidelines-compliance [feature-slug]/[component-slug]
    → Rapport HIG + Material 3 sur les composants
    → Corriger avant de passer aux écrans

─── DIRECTION ARTISTIQUE (humain) ───────────────────────────

Étape 18 — Direction artistique [MANUELLE — Antoine]
    Réalisée par le designer humain directement dans Figma.
    Aucun agent n'intervient à cette étape.

    Ce que le designer fait :
    - Définit l'identité visuelle : typographie, couleurs, ton
    - Applique les choix stylistiques sur les composants existants
    - Crée les écrans principaux (happy path) sur base des specs UX
    - Valide la cohérence avec la direction de marque

    Gate de sortie : les écrans principaux sont créés et validés
    visuellement par Antoine avant de passer à l'étape suivante.

─── REVUE ACCESSIBILITÉ #1 + INJECTION CONTENU ──────────────

Étape 19 — Revue accessibilité écrans principaux (UI)
    ⚠️ Intégrée comme critère de sortie dans create-figma-component
    → Vérification RAAM niveau A sur chaque écran principal créé
    → Labels, ordre de focus, zones tactiles — bloque si non conforme

Étape 20 — Injection du contenu réel (UI — si URL disponible)
    /fetch-content-for-frames [feature-slug]
    → Contenu depuis URL web → frames Figma
    → Remplace les placeholders par du contenu réel

─── ÉCRANS SECONDAIRES + CAS DÉGRADÉS ──────────────────────

Étape 21 — Écrans secondaires et états dégradés (UI)
    /setup-figma-frames [feature-slug] --scope secondary
    → Loading, Empty, Error sur chaque écran
    → Flux alternatifs et cas d'erreur
    → Basés sur les composants du design system déjà créés

─── REVUE ACCESSIBILITÉ #2 ───────────────────────────────────

Étape 22 — Revue accessibilité écrans secondaires (UI)
    ⚠️ Intégrée comme critère de sortie dans setup-figma-frames
    → Vérification RAAM niveau A sur les états dégradés
    → Même rigueur que les écrans principaux

─── PROTOTYPE REACT ──────────────────────────────────────────

Étape 22a — Prototype cliquable (après écrans principaux)
    /write-prototype-react [feature-slug]/[flow-slug]
    → Niveau 1 : navigation entre écrans, contenu réel
    → Partageable au client via fichier HTML standalone
    → Produit : design/[feature-slug]/prototype/prototype-[flow]-v1.html

Étape 22b — Prototype interactif (après écrans secondaires)
    /write-prototype-react [feature-slug]/[flow-slug]
    → Niveau 2 : transitions + états dégradés + micro-interactions
    → Produit : design/[feature-slug]/prototype/prototype-[flow]-v2.html

─── DOCUMENTATION FONCTIONNELLE ─────────────────────────────

Étape 22c — Documentation fonctionnelle complète (PO)
    /write-functional-doc [epic-slug]
    → Matrice EPIC→FEAT→US→écran Figma + flux + règles + personas
    → Document unique client + Dev
    → Produit : specs/[epic-slug]/functional-doc-v[X].md

─── LIVRABLES CLIENT ─────────────────────────────────────────

Étape 22d — Livrable client prototype (PO)
    /write-client-deliverable prototype [epic-slug]
    → Google Doc avec liens prototypes + guide d'utilisation
    → Section "Ce qu'on vous demande de valider"

─── VALIDATION ───────────────────────────────────────────────

Étape 23 — Code Connect (UI)
    /figma-code-connect [feature-slug]
    → Connexion composants Figma ↔ SwiftUI/Compose

Étape 24 — Review scope Figma (PO)
    /review-figma-scope [epic-slug]/[feature-slug]
    → Conformité EPIC + US + CA dans les frames

Étape 25 — Handoff Dev (UI)
    /write-figma-handoff [feature-slug]
    → Figma Projet → page 🚀 Handoff prête

─── QA ───────────────────────────────────────────────────────

Étape 26 — Rapport QA (QA Engineer)
    /write-qa-report [epic-slug]/[us-slug]
    → Produit : specs/[epic-slug]/qa/QA-###-[slug].md

─── STORE (si publication) ───────────────────────────────────

Étape 27 — Assets store (UI)
    /write-store-assets [epic-slug]
    → Figma Projet → page 📦 Store Assets
```

### Gates de validation humaine — 11 gates en lot par phase

Le Product Designer valide **en lot à la fin de chaque phase** — pas livrable par livrable. Les agents produisent tous les livrables d'une phase, puis s'arrêtent avec un résumé et une question de validation explicite.

```
/review-dod [type-livrable]/[id-livrable]   ← vérification DoD interne avant chaque gate
```

| # | Phase | Lot à valider | Question posée au Product Designer |
|---|-------|--------------|--------------------------|
| **-1** | Research stratégique | Analyse concurrentielle + Veille sectorielle + Benchmark UX | "Les analyses stratégiques donnent-elles une base suffisante pour démarrer le cadrage ?" |
| **0** | Initialisation | Cadrage + Personas | "Le cadrage et les personas reflètent-ils bien le contexte du projet ?" |
| **1** | Spécification fonctionnelle | Brief + Flux + Règles + Glossaire | "Les specs fonctionnelles sont-elles complètes et fidèles au besoin métier ?" |
| **2** | User stories | FEAT-### + US-### × N | "Les stories couvrent-elles le périmètre attendu ? Les CA sont-ils testables ?" |
| **3** | Setup Figma | Structure fichiers + Tokens + Grilles | "La structure Figma est-elle prête pour que l'équipe design démarre ?" |
| **4a** | Research utilisateur | Plan de recherche + Insights + Personas validés | "Les insights confirment-ils les hypothèses de personas et de flux ?" |
| **4** | UX Design | Navigation map + User flows + Specs écrans + Accessibilité RAAM | "Les parcours UX et specs d'écran sont-ils conformes à l'intention fonctionnelle ?" |
| **5** | Composants UI | Composants Figma + Rapport conformité HIG/M3 | "Les composants sont-ils visuellement cohérents et conformes aux guidelines ?" |
| **6** | Direction artistique | *(manuelle — pas de lot)* | Validation implicite : écrans principaux créés dans Figma |
| **7** | Écrans principaux | Frames Default iOS/Android + RAAM accessibilité #1 | "Les écrans principaux sont-ils conformes aux specs UX et à la direction artistique ?" |
| **8** | Écrans secondaires | Frames dégradées + Contenu injecté + RAAM accessibilité #2 | "Les états dégradés sont-ils traités correctement ? Le contenu est-il réaliste ?" |
| **9** | Validation fonctionnelle Figma | Rapport review scope (`review-figma-scope`) | "Les frames couvrent-elles l'intégralité du scope EPIC/US/CA ?" |
| **10** | Handoff | Code Connect + Fichier Handoff + Document handoff | "Le handoff est-il complet et transmissible à l'équipe Dev ?" |
| **11** | QA | Rapports QA × N + Verdict global | "Les résultats QA sont-ils acceptables pour passer en production ?" |

**Gate hors-phase — Changement de scope**
Tout changement de périmètre en cours de projet ou sprint passe obligatoirement par validation humaine, quelle que soit la phase en cours. Voir template dans `rules/communication.md`.

### Nouvelle feature sur projet existant

Si Figma est déjà configuré (tokens, grilles), démarrer directement à la phase 2 :
```
/write-feature-ticket [epic-slug]/[feature-slug]
```

### Intégrer le process dans un projet existant

Si un projet Figma et des specs existent déjà, démarrer par les audits :
```
Étape A — Audit Figma complet (DSM)
    /audit-figma-existing [figma-url]
    → Score /100 sur 8 dimensions
    → Plan de correction P1/P2/P3
    → Produit : specs/[project-slug]/audit/figma-audit.md

Étape B — Audit global projet (BA)
    /audit-existing-project [project-slug]
    → Specs + Figma + Design system + Code
    → Plan de migration consolidé
    → Produit : specs/[project-slug]/audit/audit-migration.md

⚠️ Validation Humaine requise
    "Le plan de migration est-il validé avant de démarrer les corrections ?"

Étape C — Appliquer les corrections P1 selon le plan
    → Chaque action P1 utilise le skill identifié dans le rapport
    → Puis reprendre le process à l'étape correspondante
```

### Besoin client non formalisé (backlog)

```
1. Créer specs/[epic-slug]/backlog/BACKLOG-###-[slug].md
2. Faire valider par le client (statut : Validated)
3. Lancer /write-cadrage [epic-slug]  ← démarre phase 0
```

---

## ⚙️ Mode d'exécution — Manuel, Semi-auto ou Automatique

Ce process peut être exécuté à trois niveaux selon le contexte et la confiance accordée aux agents.

---

### Niveau 1 — Manuel (une étape à la fois)

Chaque skill est invoqué individuellement. L'humain lit le résultat, valide, puis déclenche l'étape suivante.

**Quand l'utiliser :**
- Premier projet avec ce process
- Étapes nouvelles ou complexes (cadrage, direction artistique)
- Quand le contexte est incertain ou le scope peu clair

**Comment :**
```
/write-cadrage authentication
→ Lire le résumé de cadrage
→ Valider ou corriger
→ /write-persona authentication/regular-user
→ Lire le persona
→ Valider ou corriger
→ /write-brief-fonctionnel authentication
→ ...
```

Chaque étape attend une réponse explicite avant de continuer.

---

### Niveau 2 — Semi-automatique (séquence jusqu'à la prochaine gate humaine)

Plusieurs étapes sont enchaînées automatiquement jusqu'à atteindre un point de validation humaine obligatoire. L'agent s'arrête, présente les livrables produits, et attend la validation.

**Quand l'utiliser :**
- Process bien maîtrisé
- Scope clair et cadrage validé
- Phases techniques sans décision stratégique (UX → UI sur un flux connu)

**Comment :**

Demander explicitement de chaîner les étapes :

```
"Lance le process de spécification fonctionnelle pour l'epic authentication.
Enchaîne automatiquement : cadrage → personas → brief → flux → règles métier.
Arrête-toi avant de produire les user stories et présente-moi un résumé
de tout ce qui a été produit."
```

L'agent exécute les 5 skills dans l'ordre, applique les phases de validation internes, et s'arrête à la gate humaine pour présenter le bilan.

**Séquences semi-auto recommandées par phase (alignées sur les 11 gates) :**

| Phase | Gate | Séquence automatisable | Arrêt |
|-------|------|----------------------|-------|
| 0 — Initialisation | Gate 0 | `write-cadrage` → `write-persona` × N | Validation humaine cadrage + personas |
| 1 — Spécification | Gate 1 | `write-brief-fonctionnel` → `write-flux-fonctionnel` × N → `write-regles-metier` → `write-glossaire` | Validation humaine specs |
| 2 — User stories | Gate 2 | `write-feature-ticket` → `write-user-story` × N | Validation humaine US |
| 3 — Setup Figma | Gate 3 | `figma-read-design` → `setup-figma-project` → `setup-figma-tokens` → `setup-figma-grid` | Validation humaine structure Figma |
| 4 — UX Design | Gate 4 | `write-navigation-map` → `write-user-flow` × N → `write-figma-userflow` × N → `write-screen-spec` × N → `write-accessibility-spec` | Validation humaine specs UX |
| 5 — Composants UI | Gate 5 | `create-figma-component` × N → `check-guidelines-compliance` × N | Validation humaine composants |
| 7 — Écrans principaux | Gate 7 | `setup-figma-frames` → `write-accessibility-annotations` × N | Validation humaine écrans principaux |
| 8 — Écrans secondaires | Gate 8 | `fetch-content-for-frames` → `setup-figma-frames` (secondaires) → `write-accessibility-annotations` × N | Validation humaine états dégradés |
| 9 — Validation Figma | Gate 9 | `review-figma-scope` | Validation humaine scope |
| 10 — Handoff | Gate 10 | `figma-code-connect` → `write-figma-handoff` | Validation humaine handoff |
| 11 — QA | Gate 11 | `write-qa-report` × N → `review-dod` × N | Validation humaine finale |

---

### Niveau 3 — Automatique (phase complète sans intervention)

Une phase entière s'exécute de bout en bout. L'agent prend toutes les décisions techniques et s'arrête uniquement aux gates humaines obligatoires définies dans `rules/communication.md`.

**Quand l'utiliser :**
- Phases purement techniques (setup Figma, tokens, grilles)
- Tâches répétitives sur un process maîtrisé (génération de N user stories depuis un brief approuvé)
- Veille et rapports automatiques (`process-watch`)

**Comment :**

```
"Lance la phase de setup Figma complète pour l'epic authentication.
Enchaîne tout automatiquement jusqu'à ce que le setup soit prêt
pour que l'UX Designer commence à travailler.
Présente-moi un rapport final avec les URLs des fichiers créés."
```

ou

```
"À partir du brief fonctionnel EPIC-001-authentication approuvé,
génère automatiquement toutes les user stories sans intervention.
Applique les DoD internes et arrête-toi quand toutes les stories
sont en statut In Review, prêtes pour ma validation."
```

**Phases entièrement automatisables :**

| Phase | Condition d'automatisation | Ce qui se passe |
|-------|--------------------------|----------------|
| Setup Figma complet | Brief approuvé + MCP Figma connecté | `figma-read-design` → `setup-figma-project` → `setup-figma-tokens` → `setup-figma-grid` |
| Génération de N US | Brief + flux approuvés | `write-user-story` × N avec DoD interne |
| Audit projet existant | Accès sources confirmé | `audit-existing-project` complet |
| Veille hebdomadaire | Déclenchée chaque semaine | `process-watch` → rapport |
| Assets store | Handoff validé | `write-store-assets` complet |

---

### Règles d'automatisation

**Ce qui peut toujours être automatisé :**
- Tout skill de production documentaire (write-*)
- Setup et configuration (setup-figma-*)
- Lecture Figma (figma-read-design)
- Rapports et veille (process-watch, write-qa-report)

**Ce qui nécessite toujours une intervention humaine :**
- Validation du cadrage et de la stack technique
- Direction artistique
- Tout changement de scope
- Validation finale avant `Done`
- Arbitrage de conflits entre agents

**Ce qui peut être automatisé mais doit être relu :**
- Génération de user stories (vérifier la pertinence des CA)
- Création de personas (vérifier la représentativité)
- Review scope Figma (le PO peut valider en lot)

---

### Enchaîner plusieurs phases d'un coup

Pour les projets bien cadrés, il est possible de décrire l'objectif final et laisser l'agent planifier et exécuter l'ensemble :

```
"On a un brief fonctionnel validé pour EPIC-001-authentication.
Objectif : avoir toutes les specs UX prêtes pour que l'UI Designer
puisse démarrer lundi.

Enchaîne automatiquement :
1. Génère tous les flux fonctionnels depuis le brief
2. Génère toutes les user stories
3. Génère la navigation map
4. Génère les user flows textuels
5. Génère les user flows visuels dans Figma
6. Génère les specs de tous les écrans

Arrête-toi avant la direction artistique et présente-moi
un bilan complet de tout ce qui a été produit."
```

L'agent exécute dans l'ordre, applique les phases de validation internes (questions de fin de skill), et s'arrête à la gate humaine définie.

---

### Bonnes pratiques

- **Commencer au niveau 1** sur un nouveau projet — passer au niveau 2 dès que le process est familier
- **Toujours nommer explicitement la gate d'arrêt** dans ta demande : "arrête-toi avant X"
- **Les phases de validation internes des skills** (questions 1, 2, 3...) s'appliquent même en mode automatique — l'agent les traite lui-même et bloque si un critère échoue
- **Si un critère de validation échoue en mode auto**, l'agent s'arrête, signale le problème et attend l'instruction humaine avant de continuer
- **`process-watch`** est le seul skill conçu pour tourner de façon entièrement autonome et récurrente

## Arborescence

```
AI_Assisted_Design_Process/
├── agents/
│   ├── business-analyst.md
│   ├── product-owner.md
│   ├── ux-designer.md
│   ├── design-system-manager.md
│   ├── ui-designer.md
│   └── qa-engineer.md
│
├── rules/
│   ├── specs.md
│   ├── design.md
│   ├── personas.md
│   ├── qa.md
│   └── communication.md
│
├── skills/                         ← 19 skills invocables
│
├── specs/
│   └── [epic-slug]/
│       ├── EPIC.md
│       ├── cadrage.md
│       ├── regles-metier.md
│       ├── glossaire.md
│       ├── personas/
│       ├── features/
│       ├── stories/
│       ├── qa/
│       └── backlog/
│
├── design/
│   └── [feature-slug]/
│       ├── SCREENS-MAP.md
│       ├── DESIGN-BRIEF.md
│       ├── COMPONENTS.md
│       ├── ux/
│       └── ui/
│
└── design-system/
    ├── tokens/
    ├── components/
    └── guidelines/
```

---

## Chaîne des agents

```
ux-researcher ─────→ specs/[epic-slug]/research/
    ↓ competitive-analysis + sector-watch + ux-benchmark
    ↓ Gate -1 (Product Designer)
business-analyst ──→ specs/[epic-slug]/
    ↓ EPIC + FLUX + PERSONA (hypothétiques) + RB
    ↓ Gate 0 + 1 (Product Designer)
product-owner ─────→ specs/[epic-slug]/features/ + stories/
    ↓ FEAT + US
    ↓ Gate 2 (Product Designer)
ux-researcher ─────→ specs/[epic-slug]/research/ (research utilisateur)
    ↓ research-plan + interviews + insights + personas validés
    ↓ Gate 4a (Product Designer)
ux-designer ───────→ design/[feature-slug]/ux/
    ↓ SCREENS-MAP + user-flows + S-XX + accessibility-spec
    ↓ Gate 4 (Product Designer)
ui-designer ───────→ design/[feature-slug]/ui/
    ↓ composants + écrans
    ↓ Gates 5-10 (Product Designer)
qa-engineer ───────→ specs/[epic-slug]/qa/
    ↓ QA-###
    ↓ Gate 11 (Product Designer)
Done
```

```
design-system-manager
    ← consulté par UX Designer (guidelines accessibilité, comportements)
    ← consulté par UI Designer (tokens, composants disponibles)
    ← sollicité par UI/UX (demandes de nouveaux tokens ou composants)
    → valide et documente tout ajout au design-system/
    → notifie les agents consommateurs en cas de breaking change
```

```
Figma (canal transversal)
    ← UX Designer (frames, annotations RAAM, ordre de focus)
    ← UI Designer (handoff, statut Ready for dev)
    → QA Engineer (référence visuelle pour vérification)
    → Dev (implémentation)
    Règle : chaque frame référence sa spec .md | chaque spec référence son URL Figma
```

---

## Référence des changements

### Agents

| Réf | Fichier | Description |
|-----|---------|-------------|
| A | business-analyst | Contexte mobile iOS/Android dans Brief + colonne Plateforme dans flux |
| B | business-analyst | Trigger élargi au raffinage de specs existantes |
| C | business-analyst + product-owner | Conventions de nommage inter-agents |
| D | business-analyst | Cadrage initial — 6 questions posées en une fois |
| E | product-owner | Relation BA → PO formalisée |
| F | product-owner | Personas génériques |
| G | product-owner | DoD adaptable (socle + exigences additionnelles) |
| H | product-owner | Convention dossier specs/ alignée inter-agents |
| I | business-analyst | DoD BA ajoutée |
| J | product-owner | DoD PO refactorisée |
| K | ux-designer | Relation amont BA + PO formalisée |
| L | ux-designer | Périmètre UX vs UI — tokens supprimés |
| M | ux-designer | DoD UX ajoutée |
| N | ux-designer + product-owner | Structure dossiers Option B |
| O | design-system-manager | Position dans la chaîne + rôle de validation |
| P | design-system-manager | Structure dossiers confirmée (transversal) |
| Q | design-system-manager | Versioning sémantique + changelog |
| R | design-system-manager | 4 DoD (token, composant, guideline, breaking change) |
| S | ui-designer | Titre et intro recentrés UI Mobile uniquement |
| T | ui-designer | Relation Design System Manager formalisée |
| U | ui-designer | Relation UX Designer formalisée |
| V | ui-designer | 2 DoD (composant, écran) |
| W | ux-designer | Relation Design System Manager — tag de référence ajouté |
| X | ui-designer | Exemples de code SwiftUI/Compose remplacés par conventions de style |
| AB | ux-designer | Navigation map ajoutée comme livrable explicite |

### Rules

| Réf | Fichier | Description |
|-----|---------|-------------|
| Y | rules/personas | Nouveau — template persona + cycle de vie |
| Z | rules/design | Template SCREENS-MAP.md — navigation map |
| AA | rules/design | Distinction flux fonctionnel / user flow / navigation map |
| A | rules/design | Structure dossiers alignée Option B |
| B | rules/design | Statuts en anglais + agents responsables par transition |
| C | rules/design | Template structuré pour REVIEW-[date].md |
| D | rules/specs | Statuts en anglais + agents responsables |
| E | rules/specs | Numérotation EPIC-### alignée sur convention agents |
| F | rules/specs | Backlog documenté comme zone de staging client |

### Skills (19)

| Priorité | Skill | Agent pilote |
|----------|-------|-------------|
| Haute | `write-cadrage` | BA |
| Haute | `write-brief-fonctionnel` | BA |
| Haute | `write-flux-fonctionnel` | BA |
| Haute | `write-user-story` | PO |
| Haute | `write-screen-spec` | UX |
| Haute | `write-token` | DSM |
| Moyenne | `write-persona` | BA |
| Moyenne | `write-navigation-map` | UX |
| Moyenne | `write-component-ui` | UI |
| Moyenne | `write-cadrage` | BA |
| Basse | `write-user-flow` | UX |
| Basse | `write-regles-metier` | BA |
| Basse | `write-glossaire` | BA |
| Basse | `write-feature-ticket` | PO |
| Basse | `write-component-spec` | UX |
| Basse | `write-screen-ui` | UI |
| Basse | `write-component-ds` | DSM |
| Basse | `write-guideline` | DSM |
| Basse | `write-changelog` | DSM |
| Transversal | `review-dod` | Tous |

### Skills Figma (nouveau)

| Ordre | Skill | Agent pilote | Description |
|-------|-------|-------------|-------------|
| 1 | `setup-figma-project` | DSM | Structure pages fichiers Figma Projet + DS |
| 2 | `setup-figma-tokens` | DSM | Tokens couleurs, typo, spacing dans Figma |
| 3 | `setup-figma-grid` | DSM | Grilles iOS/Android + frames de base |
| 4 | `write-figma-userflow` | UX | Flows visuels dans Figma depuis FLUX-### |
| 5 | `setup-figma-frames` | UI | Frames vides nommées selon US + états |
| 6 | `fetch-content-for-frames` | UI | Injection contenu web → frames Figma |
| 7 | `create-figma-component` | UI | Composant Figma avec variants + tokens + auto layout |
| 8 | `check-guidelines-compliance` | UI | Conformité HIG + Material 3 |
| 9 | `review-figma-scope` | PO | Conformité frames vs EPIC + US + CA |
| 10 | `write-figma-handoff` | UI | Préparation fichier Figma pour Dev |
| 11 | `write-store-assets` | UI | Assets App Store + Play Store |

### Skills MCP Figma fondateurs (nouveaux)

| Skill | Agent | Description |
|-------|-------|-------------|
| `figma-use-wrapper` | UI / DSM | Skill fondateur — wrappe `use_figma` avec conventions projet. Obligatoire avant tout écriture |
| `figma-read-design` | UI / DSM | Pipeline de lecture — `get_metadata` + `get_variable_defs` + `search_design_system` avant écriture |
| `figma-code-connect` | UI | Connecte composants Figma → SwiftUI/Compose via Code Connect |

### Skills prototype et documentation (nouveaux)

| Skill | Agent | Description |
|-------|-------|-------------|
| `write-prototype-react` | UI | Prototype React standalone — cliquable (v1) ou interactif (v2) |
| `write-functional-doc` | PO | Documentation fonctionnelle complète — matrice EPIC→écran Figma |
| `write-client-deliverable` | PO | Livrables client Google Doc/Sheets avec section de validation |

### Skills documentation utilisateur (nouveaux)

| Skill | Agent | Description |
|-------|-------|-------------|
| `write-contextual-help` | UX | Empty states, messages d'erreur, tooltips inline |
| `write-onboarding-spec` | UX | Parcours d'onboarding — walkthroughs, tooltips découverte |
| `write-release-notes` | PO | Release notes App Store, Play Store et in-app |

### Skills maintenance et intégration (nouveaux)

| Skill | Agent | Description |
|-------|-------|-------------|
| `audit-existing-project` | BA | Audit global projet existant — specs, Figma (via audit-figma-existing), DS, code |
| `audit-figma-existing` | DSM | Revue Figma complète — 8 dimensions, score /100, plan P1/P2/P3 |
| `process-watch` | BA | Veille hebdomadaire — RAAM, HIG, M3, MCP Figma, Claude |
