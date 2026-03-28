# Flow — Nouveau Projet

Ce document décrit la séquence complète pour démarrer un projet from scratch avec le process AI Assisted Design.

---

## Point d'entrée — obligatoire

```
/project-init [project-slug]
```

Le Product Designer définit dès le départ :
- Contexte du projet (nom, secteur, type, client)
- Mode d'exécution pour chacun des 6 groupes d'étapes (Auto / Manuel / Mixte)

Un **checkpoint** est affiché entre chaque groupe pour adapter les choix si nécessaire.
En mode Manuel, le skill fournit le brief complet : liste des livrables, éléments indispensables, objectifs et DoD.

Produit : `specs/[project-slug]/project-config.md`

---

## Pré-requis avant de démarrer
- [ ] Nom du projet et secteur d'activité connus
- [ ] Client ou brief initial disponible
- [ ] MCP Figma connecté
- [ ] Accès web disponible (web_search + web_fetch)

---

## Phase Discovery — Interviews de découverte (en parallèle)
**Agent :** `ux-researcher` | **Automatisation :** Semi-auto
**Peut démarrer en même temps que le research stratégique**

```
/write-discovery-interview [project-slug]
```

**Livrables :**
- `specs/[project-slug]/research/discovery-interviews.md`
- Vocabulaire → BA (glossaire) | Opportunités V2+ → PO (backlog)

---

## Phase -1 — Research Stratégique
**Agent :** `ux-researcher` | **Automatisation :** Auto
**Gate de sortie :** Gate -1 — Product Designer valide en lot

```
/write-competitive-analysis [project-slug]
/write-sector-watch [project-slug]
/write-ux-benchmark [project-slug]
```

**Livrables :**
- `specs/[project-slug]/research/competitive-analysis.md`
- `specs/[project-slug]/research/sector-watch.md`
- `specs/[project-slug]/research/ux-benchmark.md`

---

## Phase 0 — Cadrage + Personas
**Agent :** `business-analyst` | **Automatisation :** Semi-auto
**Gate de sortie :** Gate 0 — Product Designer valide en lot

```
/write-cadrage [epic-slug]
/write-persona [epic-slug]  ← génère les 3 personas en un seul appel
```

**Livrables :**
- `specs/[epic-slug]/cadrage.md`
- `specs/[epic-slug]/personas/PERSONA-001-regular-user.md`
- `specs/[epic-slug]/personas/PERSONA-002-new-user.md`
- `specs/[epic-slug]/personas/PERSONA-003-edge-user.md`

**⚠️ Product Designer :** Valider la stack technique et le périmètre V1

---

## Phase 1 — Spécification Fonctionnelle
**Agent :** `business-analyst` | **Automatisation :** Auto
**Gate de sortie :** Gate 1 — Product Designer valide en lot

```
/write-brief-fonctionnel [epic-slug]
/write-flux-fonctionnel [epic-slug]/[flux-slug]   ← × N
/write-regles-metier [epic-slug]
/write-glossaire [epic-slug]
```

**Livrables :**
- `specs/[epic-slug]/EPIC.md`
- `specs/[epic-slug]/FLUX-###-[slug].md` × N
- `specs/[epic-slug]/regles-metier.md`
- `specs/[epic-slug]/glossaire.md`

---

## Phase 2 — User Stories
**Agent :** `product-owner` | **Automatisation :** Auto
**Gate de sortie :** Gate 2 — Product Designer valide en lot

```
/write-feature-ticket [epic-slug]/[feature-slug]   ← × N
/write-user-story [epic-slug]/[story-slug]         ← × N
```

**Livrables :**
- `specs/[epic-slug]/features/FEAT-###.md` × N
- `specs/[epic-slug]/stories/US-###.md` × N

**📤 Livrable client :** `/write-client-deliverable user-stories [epic-slug]`

---

## Phase 3 — Setup Figma
**Agent :** `design-system-manager` | **Automatisation :** Auto via MCP
**Gate de sortie :** Gate 3 — Product Designer valide en lot

```
/figma-read-design [project-slug]
/setup-figma-project [epic-slug]
/setup-figma-tokens [epic-slug]
/setup-figma-grid [epic-slug]
```

**Livrables :**
- Fichier Figma Projet : `[NomProjet] — [EPIC-###].fig`
- Fichier Figma Design System : `[nom_projet]_design_system.fig`

---

## Phase 4a — Research Utilisateur
**Agent :** `ux-researcher` | **Automatisation :** Semi-auto (Product Designer conduit les sessions)
**Gate de sortie :** Gate 4a — Product Designer valide les insights

```
/write-research-plan [epic-slug]
/write-interview-guide [epic-slug]
← Product Designer conduit les interviews →
/write-research-insights [epic-slug]
```

**Livrables :**
- `specs/[epic-slug]/research/research-plan.md`
- `specs/[epic-slug]/research/interview-guide.md`
- `specs/[epic-slug]/research/insights.md`
- `specs/[epic-slug]/personas/` mis à jour

---

## Phase 4 — UX Design
**Agent :** `ux-designer` | **Automatisation :** Auto
**Gate de sortie :** Gate 4 — Product Designer valide en lot

```
/write-navigation-map [feature-slug]
/write-user-flow [feature-slug]/[flux-slug]         ← × N
/write-figma-userflow [feature-slug]/[flux-slug]    ← × N
/write-screen-spec [feature-slug]/[screen-id]       ← × N
/write-accessibility-spec [feature-slug]
```

**Livrables :**
- `design/[feature-slug]/SCREENS-MAP.md`
- `design/[feature-slug]/ux/user-flow-FLUX-###.md` × N
- `design/[feature-slug]/ux/screens/S-XX-[nom].md` × N
- `design/[feature-slug]/ux/accessibility-spec.md`
- Figma → page `🗺️ user_flows`

---

## Phase 5 — Composants UI
**Agent :** `ui-designer` | **Automatisation :** Auto via MCP
**Gate de sortie :** Gate 5 — Product Designer valide en lot

```
/figma-read-design [feature-slug]
/setup-figma-frames [feature-slug]
/create-figma-component [feature-slug]/[slug]   ← × N
/check-guidelines-compliance [feature-slug]/[slug]  ← × N
/write-ds-documentation [component-slug]        ← × N
```

---

## Phase 6 — Direction Artistique
**⚠️ ÉTAPE MANUELLE — Product Designer**
Aucun agent n'intervient.

Le Product Designer :
1. Ouvre Figma
2. Définit l'identité visuelle (couleurs, typo, ton)
3. Crée les écrans principaux (happy path)
4. Valide la cohérence de marque

**Gate de sortie :** Écrans principaux visibles dans Figma

---

## Phase 7 — Prototype Cliquable (v1)
**Agent :** `ui-designer` | **Automatisation :** Auto
**Gate de sortie :** Gate 7 — Product Designer valide + partage au client

```
/write-accessibility-annotations [feature-slug]/[screen-id]  ← × N
/setup-figma-prototype [feature-slug]/[flow-slug]    ← connexions prototype natif Figma
/write-prototype-react [feature-slug]/[flow-slug]    ← prototype HTML standalone (Niveau 1)
```

**📤 Livrable client :** `/write-client-deliverable prototype [epic-slug]`

> ⚠️ Partager UNIQUEMENT après tests utilisateur (Phase 8b) + corrections critiques.
> Maximum 2 cycles de retours client. Si cycle 2 insuffisant :
> PO + Product Designer décident — appliquer / reporter V2 / rejeter.

---

## Phase 8 — Écrans Secondaires + Prototype Interactif (v2)
**Agent :** `ui-designer` | **Automatisation :** Auto via MCP
**Gate de sortie :** Gate 8 — Product Designer valide en lot

```
/fetch-content-for-frames [feature-slug]
/setup-figma-frames [feature-slug]          ← écrans secondaires
/write-accessibility-annotations [feature-slug]/[screen-id]  ← × N
/write-prototype-react [feature-slug]/[flow-slug]    ← Niveau 2
```

---

## Phase 8b — Tests utilisateur sur UI finale
**Agent :** `ux-researcher` + Product Designer | **Automatisation :** Mixte
**Déclenchée après :** Gate 8

```
/write-user-test-scenarios [epic-slug]/[feature-slug]
→ Scénarios depuis flux + frames Figma finalisées

← Product Designer conduit les sessions avec participants →
← Enregistrements audio/vidéo sur Drive →

/run-user-tests [epic-slug]/[feature-slug]
→ Partie auto : métriques prototype + analytics
→ Synthèse : rapport consolidé + timecodes enregistrements
```

**Livrables :**
- `specs/[epic-slug]/[feature-slug]/research/user-test-scenarios.md`
- `specs/[epic-slug]/[feature-slug]/research/user-test-report.md`
- `sessions/[feature-slug]/[date]/*.mp4` (Drive)

---

## Phase 8c — Révision plan de tagage
**Agent :** `product-owner` | **Automatisation :** Auto via MCP Figma
**Déclenchée après :** Phase 8b

```
/review-tracking-plan [epic-slug]/[feature-slug]
→ Couverture événements vs frames finalisées
→ Adaptation déclencheurs + nouveaux événements
→ Plan de tagage v[X.Y] mis à jour
```

---

## Phase 9 — Documentation Fonctionnelle + Validation
**Agent :** `product-owner` + `ui-designer` | **Automatisation :** Auto
**Gate de sortie :** Gate 9 — Product Designer valide en lot

```
/figma-code-connect [feature-slug]
/review-figma-scope [epic-slug]/[feature-slug]
/write-functional-doc [epic-slug]
```

**📤 Livrable client :** `/write-client-deliverable functional-doc [epic-slug]`

---

## Phase 10 — Handoff Dev
**Agent :** `ui-designer` | **Automatisation :** Auto via MCP
**Gate de sortie :** Gate 10 — Product Designer valide

```
/write-figma-handoff [feature-slug]
/write-store-assets [epic-slug]     ← si publication store
```

---

## Phase 11 — QA
**Agent :** `qa-engineer` | **Automatisation :** Auto
**Gate de sortie :** Gate 11 — Product Designer valide

```
/write-qa-report [epic-slug]/[us-slug]   ← × N
/review-dod [epic-slug]
```

**📤 Livrable client :** `/write-client-deliverable qa [epic-slug]`
**📤 Livrable client :** `/write-client-deliverable release-notes [version] [epic-slug]`

---

## Commande semi-auto complète (exemple)

```
"Lance le process complet pour un nouveau projet [project-slug].
Démarre par le research stratégique et enchaîne automatiquement
jusqu'à la Gate 0. Présente-moi le résumé du research pour validation."
```
