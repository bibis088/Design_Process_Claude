# Update_Antoine — Guide du projet

Copie modifiée et enrichie de `Agent_skills_Joris`.
Chaque fichier modifié est annoté avec `✅ MODIFIÉ [X]` ou `✅ NOUVEAU [X]` — la lettre renvoie au tableau de référence en bas de ce document.

---

## 🚀 Guide de démarrage rapide

### Nouveau projet — par où commencer ?

```
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

Étape 14 — Frames vides Figma (UI)
    /setup-figma-frames [feature-slug]
    → Figma Projet → page 📱 Screens — frames nommées

Étape 14b — Injection contenu (UI — optionnel si URL disponible)
    /fetch-content-for-frames [feature-slug]
    → Contenu réel depuis URL dans les frames Figma

Étape 15 — Composants Figma (UI)
    /create-figma-component [feature-slug]/[component-slug]
    → Figma → composant avec variants + tokens + auto layout

Étape 16 — Conformité guidelines (UI)
    /check-guidelines-compliance [feature-slug]/[slug]
    → Rapport HIG + Material 3

─── VALIDATION ───────────────────────────────────────────────

Étape 17 — Review scope Figma (PO)
    /review-figma-scope [epic-slug]/[feature-slug]
    → Conformité EPIC + US + CA dans les frames

Étape 18 — Handoff Dev (UI)
    /write-figma-handoff [feature-slug]
    → Figma Projet → page 🚀 Handoff prête

─── QA ───────────────────────────────────────────────────────

Étape 19 — Rapport QA (QA Engineer)
    /write-qa-report [epic-slug]/[us-slug]
    → Produit : specs/[epic-slug]/qa/QA-###-[slug].md

─── STORE (si publication) ───────────────────────────────────

Étape 20 — Assets store (UI)
    /write-store-assets [epic-slug]
    → Figma Projet → page 📦 Store Assets
```

### Gates de validation obligatoires

```
/review-dod [type-livrable]/[id-livrable]
```

| Gate | Qui valide | Condition |
|------|-----------|-----------|
| `Draft` → `In Review` | Agent auteur | DoD interne cochée |
| `In Review` → `Approved` | PO | Cohérence fonctionnelle validée |
| Frames → `Ready for dev` | PO (`review-figma-scope`) | Scope EPIC/US/CA couvert |
| `Approved` → `Dev Ready` | Tech Lead | Faisabilité technique confirmée |
| `Dev Ready` → `Done` | PO + QA | Critères d'acceptance et rapport QA validés |

### Nouvelle feature sur projet existant

Si Figma est déjà configuré (tokens, grilles), démarrer directement à l'étape 6 :
```
/write-feature-ticket [epic-slug]/[feature-slug]
```

### Besoin client non formalisé (backlog)

```
1. Créer specs/[epic-slug]/backlog/BACKLOG-###-[slug].md
2. Faire valider par le client (statut : Validated)
3. Lancer /write-cadrage [epic-slug]
```

---

## Arborescence

```
Update_Antoine/
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
client (backlog)
    ↓
business-analyst ──→ specs/[epic-slug]/
    ↓ EPIC + FLUX + PERSONA + RB
product-owner ─────→ specs/[epic-slug]/features/ + stories/
    ↓ FEAT + US
ux-designer ───────→ design/[feature-slug]/ux/
    ↓ SCREENS-MAP + user-flows + S-XX + accessibility-spec
ui-designer ───────→ design/[feature-slug]/ui/
    ↓ composants + écrans
qa-engineer ───────→ specs/[epic-slug]/qa/
    ↓ QA-###
Done (PO valide)
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
