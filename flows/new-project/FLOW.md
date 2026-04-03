# Flow — Nouveau Projet

---

## Démarrage

```
/project-init [project-slug]
```

Choisir le mode pour chacun des 6 groupes (Auto / Manuel / Mixte).
Checkpoint entre chaque groupe — possibilité d'adapter les choix.
Produit : `specs/[project-slug]/project-config.md`

**Pré-requis :** brief initial disponible · MCP Figma connecté · accès web (web_search)

---

## Groupe 1 — Research
**Agent :** `ux-researcher` | **Tout le research se fait avant le cadrage**

```
/write-competitive-analysis [project-slug]   ← auto
/write-sector-watch [project-slug]           ← auto
/write-ux-benchmark [project-slug]           ← auto

/write-research-plan [project-slug]          ← semi-auto
/write-interview-guide [project-slug]        ← semi-auto
← Product Designer conduit les interviews →
/write-research-insights [project-slug]      ← semi-auto

/write-discovery-interview [project-slug]    ← semi-auto (en parallèle)
```

**Livrables :** `research/` — competitive · sector · benchmark · research-plan · interview-guide · insights · discovery-interviews

⚠️ **Gate -1** — "Le research est-il suffisant pour cadrer avec des bases solides ?"
Bloque : le cadrage ne démarre pas sans validation

---

## Groupe 2 — Cadrage et Spécification
**Agents :** `business-analyst` + `product-owner`

```
/write-cadrage [epic-slug]              ← 10 questions (6 fonc. + 4 contractuelles)
/write-persona [epic-slug]              ← 3 personas en 1 appel (enrichis par le research)
```
⚠️ **Gate 0** — Valider cadrage · personas · stack technique · OS de base

```
/write-brief-fonctionnel [epic-slug]
/write-flux-fonctionnel [epic-slug]/[flux-slug]   ← × N
/write-regles-metier [epic-slug]
/write-glossaire [epic-slug]
```
⚠️ **Gate 1** — Valider brief · flux · règles · glossaire

```
/write-feature-ticket [epic-slug]/[feature-slug]  ← × N
/write-user-story [epic-slug]/[story-slug]        ← × N
```
⚠️ **Gate 2** — Valider features · US · CA

---

## Groupe 3 — Setup Figma
**Agent :** `design-system-manager`

```
/setup-figma-init [epic-slug]
```
Pose 2 questions : automatisé ? + URLs Figma existants.
Si non-auto → audit qualité /10 + corrections automatisables sur demande.
Orchestre selon le mode :
```
/setup-figma-project [epic-slug]     ← structure pages DS + Projet
/setup-figma-tokens [epic-slug]      ← tokens + light/dark + Text Styles
/setup-figma-grid [epic-slug]        ← grilles iOS/Android + safe areas
/setup-figma-library [epic-slug]     ← connexion DS → fichier Projet
/setup-figma-permissions [epic-slug] ← matrice accès (action manuelle PD)
```

**Livrables :** fichiers Figma DS + Projet · `specs/[epic-slug]/figma-urls.md`

⚠️ **Gate 3** — "La structure Figma est-elle prête ?"

---

## Groupe 4 — UX Design
**Agent :** `ux-designer`

```
/write-navigation-map [feature-slug]
/write-user-flow [feature-slug]/[flux-slug]         ← × N
/write-screen-spec [feature-slug]/[screen-id]       ← × N
```

**Livrables :** `SCREENS-MAP.md` · `user-flow-FLUX-###.md` × N · `S-XX-[nom].md` × N · `accessibility-spec.md`

⚠️ **Gate 4** — "Les specs UX et flows sont-ils conformes à l'intention fonctionnelle ?"

---

## Groupe 5 — UI Design
**Agent :** `ui-designer` (+ Product Designer pour la direction artistique)

```
/figma-read-design [feature-slug]
/write-screen-spec [feature-slug]/[screen-id]       ← × N (specs pendant la phase UI)
/setup-figma-frames [feature-slug]              ← frames OS de base depuis US-###.md
/create-figma-component [feature-slug]/[slug]   ← × N
/check-guidelines-compliance [feature-slug]/[slug]
/write-ds-documentation [component-slug]        ← × N
```
⚠️ **Gate 5** — Valider composants

```
⚠️ DIRECTION ARTISTIQUE — Product Designer (étape 100% manuelle dans Figma)
   Identité visuelle · écrans principaux (happy path)
```
⚠️ **Gate 6** — Écrans principaux validés visuellement

```
/write-accessibility-annotations [feature-slug]/[screen-id]  ← × N
/setup-figma-prototype [feature-slug]/[flow-slug]            ← prototype natif Figma
/write-prototype-react [feature-slug]/[flow-slug]            ← prototype HTML v1
```
⚠️ **Gate 7** — Valider + partage client (après tests utilisateur)

```
/fetch-content-for-frames [feature-slug]         ← injection contenu réel
/setup-figma-frames [feature-slug]               ← écrans secondaires + états dégradés
/write-accessibility-annotations [feature-slug]/[screen-id]  ← × N
/write-prototype-react [feature-slug]/[flow-slug]            ← prototype HTML v2
```
⚠️ **Gate 8** — Valider tous les écrans

---

## Groupe 6 — Validation et Livraison
**Agents :** `ux-researcher` + `product-owner` + `ui-designer` + `qa-engineer`

```
/write-user-test-scenarios [epic-slug]/[feature-slug]
← Product Designer conduit les sessions (enregistrements audio/vidéo) →
/run-user-tests [epic-slug]/[feature-slug]

/write-tracking-plan [epic-slug]/[feature-slug]
/review-tracking-plan [epic-slug]/[feature-slug]

/figma-code-connect [feature-slug]
/review-figma-scope [epic-slug]/[feature-slug]
/write-functional-doc [epic-slug]
→ Client : /write-client-deliverable functional-doc [epic-slug]
```
⚠️ **Gate 9** — "Tests · tracking · scope · doc fonctionnelle validés ?"

```
/os-decline [epic-slug]/[feature-slug]
→ Agents : duplication frames · grilles · composants système · text styles
→ Product Designer : ajustements composants spécifiques

/write-figma-handoff [feature-slug]
/write-store-assets [epic-slug]
```
⚠️ **Gate 10** — Valider handoff

```
/write-qa-report [epic-slug]/[us-slug]   ← × N
/review-dod [epic-slug]
```
⚠️ **Gate 11** — Valider QA → Publication
