# Rôle du Product Designer dans le process AI Assisted Design

Ce document définit précisément ce que le Product Designer fait lui-même et ce qu'il délègue aux agents IA — pour qu'un nouveau Product Designer comprenne immédiatement son périmètre.

---

## Principe général

Le Product Designer est le **décideur et valideur** du process. Il ne produit pas les documents — il oriente, valide et décide. Les agents produisent, lui approuve ou corrige.

> "Les agents font le travail de production. Le Product Designer fait le travail de décision."

---

## Ce que le Product Designer fait lui-même

Ces actions ne peuvent pas être déléguées aux agents — elles requièrent son jugement, son expertise ou son autorité.

### Validation et décision
| Action | Quand | Gate |
|--------|-------|------|
| Valider le cadrage et le brief fonctionnel | Après `/write-cadrage` + `/write-brief-fonctionnel` | Gate 0 + 1 |
| Valider les personas | Après `/write-persona` × N | Gate 0 |
| Valider les user stories et leurs CA | Après `/write-user-story` × N | Gate 2 |
| Valider les composants Figma | Après `/create-figma-component` × N | Gate 5 |
| Valider le prototype React avec le client | Après `/write-prototype-react` | Hors gate |
| Décider de tout changement de scope | À tout moment | Gate hors-phase |
| Arbitrer les conflits entre agents | À tout moment | — |

### Direction artistique *(étape manuelle)*
Le Product Designer intervient **directement dans Figma** après la Gate 5 :
- Définit l'identité visuelle du projet (couleurs, typographie, ton, ambiance)
- Applique les choix stylistiques sur les composants existants
- Crée les écrans principaux (happy path) depuis zéro
- Valide la cohérence avec la direction de marque du client
- **Gate de sortie** : les écrans principaux sont créés et visibles dans Figma avant que les agents reprennent

---

## Ce que le Product Designer délègue aux agents

Ces actions sont exécutées automatiquement ou semi-automatiquement. Le Product Designer reçoit le résultat, le relit et valide à la gate correspondante.

### Research (UX Researcher)
| Délégué à | Action | Niveau d'automatisation |
|-----------|--------|------------------------|
| `ux-researcher` | Analyse concurrentielle | ✅ Automatique (web_search) |
| `ux-researcher` | Veille sectorielle + réglementations | ✅ Automatique (web_search) |
| `ux-researcher` | Benchmark UX/UI | ✅ Automatique (web_search) |
| `ux-researcher` | Plan de recherche utilisateur | ⚙️ Semi-auto (structure produite, Product Designer conduit les sessions) |
| `ux-researcher` | Guide d'entretien | ⚙️ Semi-auto |
| `ux-researcher` | Synthèse des insights | ⚙️ Semi-auto (Product Designer fournit les notes brutes) |

### Spécification fonctionnelle (BA + PO)
| Délégué à | Action | Niveau d'automatisation |
|-----------|--------|------------------------|
| `business-analyst` | Cadrage (6 questions) | ⚙️ Semi-auto (Product Designer répond aux questions) |
| `business-analyst` | Brief fonctionnel | ✅ Automatique depuis le cadrage |
| `business-analyst` | Flux fonctionnels | ✅ Automatique depuis le brief |
| `business-analyst` | Règles métier | ✅ Automatique depuis le brief |
| `business-analyst` | Glossaire | ✅ Automatique |
| `business-analyst` | Personas hypothétiques | ✅ Automatique (validés par le Product Designer) |
| `product-owner` | Feature tickets | ✅ Automatique depuis le brief |
| `product-owner` | User stories | ✅ Automatique depuis les features |
| `product-owner` | Priorisation MoSCoW | ✅ Automatique (Product Designer peut ajuster) |

### Setup Figma (Design System Manager)
| Délégué à | Action | Niveau d'automatisation |
|-----------|--------|------------------------|
| `design-system-manager` | Structure fichiers Figma | ✅ Automatique via MCP |
| `design-system-manager` | Tokens couleurs, typo, spacing | ✅ Automatique via MCP |
| `design-system-manager` | Grilles iOS/Android | ✅ Automatique via MCP |
| `design-system-manager` | Documentation design system | ✅ Automatique |

### UX Design (UX Designer)
| Délégué à | Action | Niveau d'automatisation |
|-----------|--------|------------------------|
| `ux-designer` | Navigation map | ✅ Automatique depuis les flux |
| `ux-designer` | User flows textuels | ✅ Automatique depuis les flux |
| `ux-designer` | User flows visuels Figma | ✅ Automatique via MCP |
| `ux-designer` | Specs d'écrans | ✅ Automatique depuis les US |
| `ux-designer` | Spec accessibilité RAAM | ✅ Automatique |
| `ux-designer` | Annotations accessibilité | ✅ Automatique via MCP |

### UI Design (UI Designer)
| Délégué à | Action | Niveau d'automatisation |
|-----------|--------|------------------------|
| `ui-designer` | Frames vides nommées | ✅ Automatique via MCP |
| `ui-designer` | Composants de base Figma | ✅ Automatique via MCP |
| `ui-designer` | Conformité HIG/Material 3 | ✅ Automatique |
| `ui-designer` | Injection contenu web | ✅ Automatique via web_fetch |
| `ui-designer` | Écrans secondaires et états dégradés | ✅ Automatique via MCP |
| `ui-designer` | Prototype React | ✅ Automatique |
| `ui-designer` | Code Connect | ✅ Automatique via MCP |
| `ui-designer` | Handoff Dev | ✅ Automatique via MCP |
| `ui-designer` | Assets store | ✅ Automatique via MCP |

### Documentation et livrables (PO)
| Délégué à | Action | Niveau d'automatisation |
|-----------|--------|------------------------|
| `product-owner` | Documentation fonctionnelle | ✅ Automatique depuis les specs |
| `product-owner` | Livrables client Google Doc | ✅ Automatique depuis les docs internes |
| `product-owner` | Plan de tagage (KPIs → événements) | ✅ Automatique depuis le cadrage |
| `ux-designer` | Mapping plan de tagage sur les écrans | ✅ Automatique depuis les specs |
| `product-owner` | Release notes | ✅ Automatique depuis les US Done |

### QA (QA Engineer)
| Délégué à | Action | Niveau d'automatisation |
|-----------|--------|------------------------|
| `qa-engineer` | Rapports de test | ✅ Automatique depuis les specs |
| `qa-engineer` | Conformité RAAM | ✅ Automatique |

---

## Zones grises — ce qui est délégué mais relu attentivement

Ces livrables sont produits par les agents mais nécessitent une relecture approfondie du Product Designer avant validation — pas juste un "oui" rapide.

| Livrable | Pourquoi une relecture approfondie |
|----------|-----------------------------------|
| Personas | Doivent refléter la réalité du terrain — pas juste des archétypes théoriques |
| User stories | Les CA doivent être testables ET couvrir les edge cases métier |
| Specs d'écrans | L'intention UX peut être mal interprétée par l'agent |
| Insights de recherche | L'agent structure — mais le Product Designer valide les interprétations |
| Prototype React | À tester sur mobile avant de le partager au client |

---

## Ce que le Product Designer ne fait jamais

- Il ne code pas les composants Figma (délégué au UI Designer)
- Il ne rédige pas les specs d'écran (délégué au UX Designer)
- Il ne conduit pas les recherches automatisées (délégué au UX Researcher)
- Il ne génère pas les tokens (délégué au Design System Manager)
- Il ne prépare pas les fichiers de handoff (délégué au UI Designer)

---

## Résumé en une phrase par phase

| Phase | Ce que le Product Designer fait |
|-------|--------------------------------|
| Research stratégique | Valide les analyses en lot (Gate -1) |
| Cadrage | Répond aux 6 questions, valide le résumé |
| Spécification | Valide le brief + flux + règles en lot (Gate 1) |
| User stories | Valide les CA et la priorisation en lot (Gate 2) |
| Setup Figma | Valide la structure Figma en lot (Gate 3) |
| UX Design | Valide les specs UX en lot (Gate 4) |
| Composants UI | Valide les composants en lot (Gate 5) |
| **Direction artistique** | **Crée les écrans principaux — étape 100% manuelle** |
| Écrans | Valide les écrans en lot (Gates 7 + 8) |
| Validation Figma | Valide le scope en lot (Gate 9) |
| Handoff | Valide le handoff en lot (Gate 10) |
| QA | Valide les rapports QA en lot (Gate 11) |
