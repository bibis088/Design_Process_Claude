# Prérequis par phase — AI Assisted Design Process

---

## Prérequis transversaux (toujours requis)

| Élément | Requis par |
|---------|-----------|
| **MCP Figma connecté** | Tous les skills Figma (setup, UI, validation, handoff) |
| **Accès web** | Research stratégique · fetch-content · process-watch · project-init |
| **`cadrage.md` validé** | Personas · Specs · Setup Figma · Frames · Tracking plan · Livraison client |
| **OS de base défini** | Setup Figma · Frames · os-decline |
| **`EPIC.md` + `US-###` Approved** | Setup Figma · Frames · Composants · Validation · Handoff |
| **`figma-read-design` exécuté** | setup-figma-project · setup-figma-tokens · figma-use-wrapper |
| **Tokens DS disponibles** | write-component · write-screen · write-component-ui |

---

## Phase 0 — Initialisation

| Skill | Prérequis obligatoires |
|-------|----------------------|
| `/project-init` | Brief initial disponible · accès web · MCP Figma connecté |

---

## Groupe 1 — Research

| Skill | Prérequis obligatoires |
|-------|----------------------|
| `/write-strategic-research` | Accès web · nom du projet et secteur connus |
| `/write-user-research` | Secteur et cible identifiés · Gate -1 research stratégique en cours · participants potentiels identifiés |
| `/write-discovery-interview` | Secteur et cible identifiés (même partiellement) · participants potentiels identifiés |
| `/write-usability-test` | Maquettes UX Figma disponibles · prototype navigable · insights research · participants recrutés |
| `/run-user-tests` | Gate 8 validée · `US-###` disponibles · MCP Figma · participants recrutés · consentements signés · outil d'enregistrement configuré |

**Gate -1** — Research complet (stratégique + utilisateur + discovery) validé avant cadrage.

---

## Groupe 2 — Cadrage et Spécification

| Skill | Prérequis obligatoires |
|-------|----------------------|
| `/write-cadrage` | Gate -1 validée · research disponible · brief initial ou backlog disponible |
| `/write-persona` | `cadrage.md` Q1 répondue · contexte d'usage connu |
| `/write-specs` | Gate 0 validée · `cadrage.md` + `PERSONA-###` Approved |
| `/write-user-story` | Gate 1 validée · `EPIC.md` + `FLUX-###` Approved · features à créer identifiées |

**Gate 0** — Cadrage · Personas · Stack technique · OS de base
**Gate 1** — Brief · Flux · Règles · Glossaire
**Gate 2** — Features · US · CA

---

## Groupe 3 — Setup Figma

| Skill | Prérequis obligatoires |
|-------|----------------------|
| `/setup-figma-init` | `project-init` exécuté · OS de base dans `cadrage.md` · `write-cadrage` validé · MCP Figma · `US-###` disponibles |
| `/setup-figma-project` | `EPIC.md` Approved · `US-###` disponibles · `cadrage.md` disponible · MCP Figma · `figma-read-design` exécuté |
| `/setup-figma-tokens` | Fichier DS créé · `cadrage.md` disponible (direction stylistique + OS de base) · tokens dans `design-system/tokens/` · MCP Figma |
| `/figma-read-design` | URL Figma ou feature-slug fourni · MCP Figma connecté |
| `/figma-use-wrapper` | MCP Figma connecté et authentifié · `figma-read-design` exécuté |

**Gate 3** — Structure Figma complète (DS + Projet + tokens + grilles + bibliothèque + permissions)

---

## Groupe 4 — UX Design

| Skill | Prérequis obligatoires |
|-------|----------------------|
| `/write-navigation-map` | `FLUX-###` Approved · `US-###` connues · specs fonctionnelles disponibles |
| `/write-user-flow` | `FLUX-###` Approved · `PERSONA-###` identifiés · `SCREENS-MAP.md` disponible (`write-navigation-map` exécuté) |
| `/write-accessibility-annotations` | Spec écran `S-XX.md` disponible · spec RAAM feature disponible · URL frame Figma renseignée · `US-###` source identifiée |
| `/write-ux-content` | `US-###` disponibles · contexte d'usage identifié · specs d'écrans en cours ou disponibles |

**Gate 4** — Navigation map · Flows · Specs · RAAM validés

---

## Groupe 5 — UI Design

| Skill | Prérequis obligatoires |
|-------|----------------------|
| `/setup-figma-frames` | OS de base défini · `US-###` disponibles · `SCREENS-MAP.md` disponible · `setup-figma-tokens` exécuté · MCP Figma |
| `/write-component` | MCP Figma connecté · `figma-read-design` exécuté · tokens DS disponibles · `US-###` et specs UX disponibles |
| `/write-screen` | `US-###` source Approved · Gate 6 validée (direction artistique) · frames créées · composants DS disponibles |
| `/fetch-content-for-frames` | Frames créées (`setup-figma-frames`) · URLs de référence fournies · `US-###` disponibles · MCP Figma · accès web |
| `/setup-figma-prototype` | Frames principaux créées · snake_case · user flows disponibles · `US-###` disponibles · MCP Figma |
| `/write-prototype-react` | Gate 7 (v1) ou Gate 8 (v2) · frames Figma validées · MCP Figma (`get_design_context`) · niveau choisi |
| `/write-component-ui` | Spec composant UX disponible · tokens DS vérifiés · composant non existant dans DS · `US-###` sources identifiées |

**Gate 5** — Composants validés
**Gate 6** — Direction artistique (Product Designer) — écrans principaux
**Gate 7** — Prototype v1 + annotations validés
**Gate 8** — Tous les écrans + Prototype v2 validés

---

## Groupe 6 — Validation et Livraison

| Skill | Prérequis obligatoires |
|-------|----------------------|
| `/tracking-plan` | `cadrage.md` disponible (KPIs) · specs écrans disponibles · MCP Figma (Phase B) · Gate 9 en cours (A) · Gate 8 validée (B) |
| `/review-figma-scope` | Frames créées et peuplées · composants conformes · `EPIC.md` disponible · `US-###` + CA disponibles · MCP Figma |
| `/write-functional-doc` | Gate 9 validée · `EPIC.md` Approved · tous `US-###` Approved · frames `Ready for dev` · prototype React · plan de tagage révisé · rapport tests utilisateur |
| `/write-client-deliverable` | `cadrage.md` disponible (itérations contractuelles) · document source Approved ou In Review · gate correspondante atteinte |
| `/os-decline` | Gate 9 validée · frames OS de base dans `Validated UI` / `Ready for dev` · OS base + secondaire définis · `US-###` disponibles · MCP Figma |
| `/write-handoff` | Gate 10 · MCP Figma · frames `Ready for dev` · `US-###` Approved et CA disponibles |
| `/write-store-assets` | Handoff validé · frames `Ready for dev` · icône app designée (1024×1024pt sans transparence) · MCP Figma |
| `/write-qa-report` | US/FEAT `Dev Ready` · CA disponibles · spec écran disponible · annotations RAAM (`write-accessibility-annotations`) exécutées · URLs Figma renseignées |
| `/write-release-notes` | `US-###` Done · rapport QA Approved · version définie (ex: 1.2.0) |
| `/review-dod` | Livrables de la phase disponibles · `US-###` et CA de la feature disponibles |

**Gate 9** — Tests · Tracking · Doc fonctionnelle · Scope complet validés
**Gate 10** — Handoff Dev validé
**Gate 11** — QA validé → Publication

---

## Groupe 7 — Design System

| Skill | Prérequis obligatoires |
|-------|----------------------|
| `/write-token` | Demande soumise par un agent · palette primitives consultée · token non existant dans DS · usage clairement défini |
| `/write-component-ds` | Composant créé dans Figma DS (`write-component`) · `US-###` sources identifiées · spec comportementale rédigée |
| `/write-changelog` | Changement DS effectué et validé · version précédente documentée |

---

## Groupe 8 — Maintenance

| Skill | Prérequis obligatoires |
|-------|----------------------|
| `/audit-project` | URLs Figma existants · MCP Figma connecté · specs existantes accessibles |
| `/process-watch` | Accès web disponible |
| `/project-init` | Brief initial ou description du projet · accès web · MCP Figma connecté |

---

## Checklist pré-lancement

Avant de démarrer `/project-init`, vérifier :

- [ ] Brief client ou description du projet disponible
- [ ] MCP Figma configuré et connecté dans Claude Code
- [ ] Accès web disponible (web_search + web_fetch)
- [ ] Nom du projet et secteur d'activité définis
- [ ] Stack technique connue (iOS / Android / les deux)
- [ ] Client ou usage interne identifié
