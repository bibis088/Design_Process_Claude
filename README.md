# AI Assisted Design Process

> Un système de conception produit mobile piloté par des agents IA — du brief client jusqu'au handoff développeur.

---

## Qu'est-ce que c'est ?

Ce dossier est un **process de conception agentic** pour applications mobiles iOS (SwiftUI) et Android (Jetpack Compose). Il orchestre 7 agents IA spécialisés qui travaillent en séquence pour produire tous les livrables d'un projet de design produit — specs fonctionnelles, maquettes Figma, prototypes, documentation, handoff Dev.

**Ce que les agents font à ta place :**
- Analyser la concurrence, les tendances du secteur et conduire le research utilisateur
- Poser les 13 questions de cadrage, structurer les specs fonctionnelles et les user stories
- Analyser les fichiers Design System et Projet Figma existants selon la stack cible (SwiftUI / Jetpack Compose)
- Structurer le fichier Projet Figma selon les specs définies lors du cadrage (sections US, pages, nomenclature)
- Documenter chaque composant en 5 specs structurées dans Figma (Anatomy, API, Color, Structure, Screen Reader)
- Concevoir les écrans dégradés et secondaires en s'appuyant sur les composants UI du Design System et le UI des écrans principaux existants
- Générer les prototypes interactifs et préparer le handoff Dev

**Ce que tu fais toujours toi-même :**
- Conduire les interviews et les sessions de recherche utilisateur (les agents préparent le guide et synthétisent les insights)
- Valider le scope, les specs et la répartition en sprint — avec ou sans l'aide du skill `/suggest-sprint-plan`
- Mise en place initiale de l'environnement Figma (si mode Manuel ou Mixte sur le Groupe 3)
- Créer les écrans et flux principaux dans Figma (Gate 6 — direction artistique, étape 100% manuelle)
- Valider à chaque gate avant de continuer
- Décider des changements de scope

---

## Installation et mise en place

### Prérequis

Avant de démarrer, tu as besoin de :

1. **Claude Code** — l'interface agent en ligne de commande d'Anthropic
2. **MCP Figma connecté** — obligatoire pour toutes les étapes Figma
   → Suivre la doc : https://developers.figma.com/docs/figma-mcp-server/
3. **Accès web** — pour le research automatique (web_search + web_fetch)

### Copier le dossier dans ton projet

```bash
# Copier le dossier dans le répertoire de ton projet
cp -r AI_Assisted_Design_Process/ ~/mon-projet/

# Renommer selon ton projet si souhaité
mv ~/mon-projet/AI_Assisted_Design_Process ~/mon-projet/design-process
```

### Structure du dossier

```
AI_Assisted_Design_Process/
│
├── agents/          → Les 7 agents IA et leurs rôles
├── rules/           → Les règles globales appliquées par tous les agents
│                      (figma.md, design.md, personas.md, accessibility.md...)
├── skills/          → Les 44 skills invocables — les commandes du process
│   ├── 01-research/     research stratégique + utilisateur + tests
│   ├── 02-cadrage/      cadrage, personas, specs, user stories
│   ├── 03-figma-setup/  structure Figma, tokens, grilles, bibliothèque
│   ├── 04-ux-design/    navigation map, flows, specs écrans, RAAM
│   ├── 05-ui-design/    composants, écrans, prototypes
│   ├── 06-validation/   tests, tracking, handoff, QA
│   ├── 07-design-system/ tokens, composants DS, changelog
│   ├── 08-maintenance/  initialisation, audit, veille
│   └── figma-mcp/   → Skills Plugin API Figma officiel
│                      (figma-use · figma-generate-design · figma-implement-design)
├── flows/           → Séquences complètes pas à pas
│   ├── new-project/FLOW.md
│   └── existing-project/FLOW.md
├── specs/           → Livrables générés pendant le process
│   └── [epic-slug]/     cadrage · personas · specs · US · research
├── design/          → Specs UX et UI générées
│   └── [feature-slug]/  navigation map · flows · specs écrans
├── design-system/   → Tokens, composants, documentation DS
├── PREREQUISITES.md → Prérequis obligatoires par skill
├── PRODUCT_DESIGNER_ROLE.md → Ce que tu fais vs délègues
└── CHANGELOG.md     → Historique des évolutions du process
```

---

## Démarrage d'un nouveau projet

### Commande unique

Ouvre Claude Code dans le dossier du process et tape :

```
/project-init [nom-de-ton-projet]
```

Claude te pose **3 questions** avant de démarrer :

**Q1 — Contexte** : nom du projet, secteur d'activité, type d'app (iOS / Android / les deux), client externe ou usage interne.

**Q2 — Périmètre** : pour chacune des 21 étapes réparties en 6 groupes, tu indiques ✅ Inclus ou ⏭️ Ignoré.
> Exemple : si tu as déjà fait le research, tu ignores le Groupe 1. Si tu n'as pas besoin de QA formalisé, tu ignores l'étape 6f.
> Les étapes ignorées peuvent être réactivées à n'importe quel checkpoint.

**Q3 — Mode d'exécution** : pour chaque groupe inclus, tu choisis :
- 🤖 **Auto** — les agents produisent tout, tu valides à la fin du groupe
- ✋ **Manuel** — tu produis toi-même, les agents te donnent un brief détaillé (livrables attendus, éléments indispensables, DoD) et vérifient ton travail
- 🔀 **Mixte** — tu choisis étape par étape au démarrage du groupe

Un **checkpoint** apparaît avant chaque groupe pour ajuster tes choix si le contexte a évolué.

### Mode projet complet vs mode sprint

Le process peut tourner de deux façons :

**Projet complet** — une seule instance du process pour tout le périmètre V1.

**Par sprint** — une instance par sprint. Le cadrage, le setup Figma et le research sont exécutés **une seule fois** au démarrage et partagés entre tous les sprints.

```bash
# Sprint 1
/project-init mon-projet-sprint-1

# Sprint 2 (Figma existant, cadrage existant — agents le détectent)
/project-init mon-projet-sprint-2
```

---

## Comment fonctionne le process

### Les 6 groupes et les 13 gates

Le process est séquentiel. Chaque groupe produit des livrables qui alimentent le suivant. Une **gate** entre chaque groupe valide les livrables avant d'autoriser la suite — rien ne démarre sans ta validation explicite.

```
GROUPE 1 — Research
  Agents : ux-researcher
  → Analyse concurrentielle · veille sectorielle · benchmark UX
  → Plan de recherche · guide d'entretien · [toi : interviews] · insights
  → Discovery interviews — besoins larges, roadmap V2+
  ↓ GATE -1 : "Le research est-il suffisant pour cadrer avec des bases solides ?"

GROUPE 2 — Cadrage et Spécification
  Agents : business-analyst · product-owner
  → 13 questions cadrage (6 fonctionnelles + 4 contractuelles + 3 planification)
  → Personas enrichis par les insights terrain
  → Brief fonctionnel · flux · règles métier · glossaire
  → User stories avec CA · gestion des erreurs · accessibilité · advisory
  ↓ GATE 0 : cadrage + personas + stack + OS de base + planning sprints
  ↓ GATE 1 : brief + flux + règles
  ↓ GATE 2 : features + US + CA

GROUPE 3 — Setup Figma
  Agent : design-system-manager
  → Question : setup automatisé ou manuel ?
  → Si fichiers existants : analyse qualité /10 + liste des corrections
  → Structure pages DS (standard Neow — 38 pages)
  → Structure pages Projet (Design · Research & UX · Ressources · Deliverables...)
  → Tokens couleurs light/dark · text styles iOS/Android · grilles · bibliothèque
  → Composant section/feat (3000×1500px · auto layout) + frame all_feat_sections
  ↓ GATE 3 : DoD 45 critères (fichiers · pages · tokens · grilles · bibliothèque)

GROUPE 4 — UX Design
  Agent : ux-designer
  → Navigation map (architecture de l'app)
  → User flows textuels depuis les flux fonctionnels
  → Specs écrans — contenu, comportements, accessibilité RAAM
  → Annotations accessibilité dans Figma
  ↓ GATE 4 : navigation · flows · specs · RAAM

GROUPE 5 — UI Design
  Agents : ui-designer · design-system-manager · [toi : direction artistique]
  → Analyse DS et fichier Projet selon la stack (SwiftUI / Jetpack Compose)
  → Frames OS de base créées automatiquement depuis les US (nomenclature snake_case)
  → Composants avec variants, tokens et 5 specs Documentation UX/UI dans Figma
    (Anatomy · API · Color · Structure · Screen Reader)
  → [TOI] Direction artistique — identité visuelle, écrans principaux dans Figma
  → Écrans secondaires et états dégradés (loading · empty · error)
    conçus à partir des composants DS et du UI des écrans principaux
  → Prototype React interactif (v1 navigation · v2 interactions)
  → Tests utilisateurs sur UI finale
  ↓ GATE 5 : composants
  ↓ GATE 6 : direction artistique (toi)
  ↓ GATE 7 : prototype v1
  ↓ GATE 8 : tous les écrans + proto v2

GROUPE 6 — Validation et Livraison
  Agents : product-owner · ux-researcher · ui-designer · qa-engineer
  → Plan de tagage KPIs → événements analytics
  → Documentation fonctionnelle complète
  → Déclinaison OS secondaire (agents : structure · toi : composants spécifiques)
  → Handoff Dev (Code Connect + package Figma)
  → Assets store
  → Rapports QA par US
  ↓ GATE 9 : tests · tracking · scope · doc
  ↓ GATE 10 : handoff
  ↓ GATE 11 : QA → Publication
```

### Les agents et leurs rôles

| Agent | Phases | Ce qu'il produit |
|-------|--------|-----------------|
| `ux-researcher` | 1, 6 | Research stratégique · guides interviews · insights · tests utilisateurs |
| `business-analyst` | 2 | Cadrage · specs fonctionnelles · flux · règles métier · personas |
| `product-owner` | 2, 6 | User stories avec CA · tracking plan · doc fonctionnelle · livrables client |
| `design-system-manager` | 3, 5, 7 | Setup Figma · tokens · bibliothèque · documentation DS |
| `ux-designer` | 4 | Navigation map · user flows · specs écrans · annotations RAAM |
| `ui-designer` | 5, 6 | Composants · frames · prototypes · handoff · os-decline |
| `qa-engineer` | 6 | Rapports QA par US · vérification DoD |

---

## Les skills — les commandes du process

Un **skill** est une commande que tu invoques dans Claude Code. Il orchestre un ou plusieurs agents pour produire un livrable précis.

### Syntaxe

```
/[nom-du-skill] [argument]
```

### Exemples concrets

```bash
# Démarrer un nouveau projet
/project-init project_name

# Suggérer une découpe en sprints (optionnel — avant validation Gate 0)
/suggest-sprint-plan project_name

# Lancer le research stratégique (competitive, sector, benchmark en 1 appel)
/write-strategic-research project_name

# Lancer le cadrage — 13 questions en une seule fois
/write-cadrage project_name

# Générer les 3 personas depuis les insights
/write-persona project_name

# Initialiser Figma (automatisé ou analyse de l'existant)
/setup-figma-init project_name

# Créer un composant avec ses 5 specs Documentation UX/UI
/write-component project_name/button-primary

# Créer un écran (spec + implémentation Figma)
/write-screen project_name/US_042

# Lancer les tests utilisateurs
/run-user-tests project_name/authentication

# Décliner pour l'OS secondaire après Gate 9
/os-decline project_name/authentication
```

### Skills optionnels

Certains skills ne sont pas dans la séquence principale mais peuvent être déclenchés à la demande :

| Skill | Quand l'utiliser |
|-------|-----------------|
| `/suggest-sprint-plan [epic-slug]` | Avant Gate 0 — propose une répartition US/sprints basée sur les estimations, dépendances et capacité. Le Product Designer valide ou ajuste avant de confirmer le cadrage. |
| `/audit-project [project-slug]` | Rejoindre un projet existant — audit specs + Figma |
| `/process-watch` | Veille hebdomadaire HIG · Material 3 · RAAM · MCP Figma |

### Les skills Figma MCP (dossier `figma-mcp/`)

Ces skills interagissent directement avec les fichiers Figma via le Plugin API.
**Règle absolue : charger `figma-use` avant tout appel `use_figma`.**

| Skill | Rôle |
|-------|------|
| `figma-use` | Prérequis obligatoire — règles Plugin API critiques (couleurs 0-1, fonts, FILL/HUG, page context, return IDs...) |
| `figma-read-design` | Lire la structure du fichier avant d'écrire (get_metadata + get_variable_defs) |
| `figma-generate-design` | Créer des frames d'écran section par section depuis les specs UX — DS tokens uniquement |
| `figma-implement-design` | Traduire un nœud Figma en code SwiftUI ou Jetpack Compose avec fidélité 1:1 |

---

## Projet existant ou intégration en cours de route

Si tu rejoins un projet déjà démarré, veux auditer un fichier Figma existant ou démarrer le process sur une base existante :

```bash
# Auditer un projet existant (specs + Figma)
/audit-project [project-slug]
```

Pour le setup Figma sur fichier existant :
```
/setup-figma-init [epic-slug]
→ Répondre NON à "automatisé ?"
→ Fournir les URLs Figma existants (DS + Projet + charte client si applicable)
→ L'agent analyse : structure pages · nomenclature · tokens · composants
→ Rapport qualité /10 sur 7 dimensions
→ Tu valides les corrections automatisables → l'agent les applique
```

---

## Nomenclature Figma — référence rapide

Toutes les règles de nommage sont dans `rules/figma.md`. Les essentielles :

```
Frame    US_042_login_ios_default        ← US_###_[nom]_[os]_[état]
Section  US_042_login                   ← US_###_[nom]
Layer    hero_image · icon_search       ← snake_case strict (jamais de nom auto Figma)
Composant button/primary/default        ← cat/nom/variante/état
Token    color/interactive/primary      ← catégorie/rôle
IDs      US_042 · EPIC_001              ← MAJUSCULES pour les identifiants
```

---

## Références

| Document | Contenu |
|----------|---------|
| `PREREQUISITES.md` | Prérequis obligatoires par skill — ce qui doit exister avant chaque commande |
| `PRODUCT_DESIGNER_ROLE.md` | Ce que tu fais vs ce que tu délègues — détail complet |
| `flows/new-project/FLOW.md` | Séquence complète avec toutes les commandes dans l'ordre |
| `flows/existing-project/FLOW.md` | Intégration du process sur un projet déjà démarré |
| `CHANGELOG.md` | Historique des évolutions du process (v1.0.0 → v2.0.0) |
