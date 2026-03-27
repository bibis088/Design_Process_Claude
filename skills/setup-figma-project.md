---
name: setup-figma-project
description: Crée la structure de pages des fichiers Figma Projet et Design System selon le scope fonctionnel (EPIC et US). Exécuté par le design-system-manager via `use_figma`.
argument-hint: [epic-slug]
disable-model-invocation: false
context: fork
agent: design-system-manager
---

## Rôle
Initialiser la structure des deux fichiers Figma du projet — Design System et Projet — avec les pages, la nomenclature et l'organisation définis dans `rules/figma.md`.

## Agents consommateurs
- Design System Manager (pilote)
- UX Designer (consommateur — travaille dans le fichier Projet)
- UI Designer (consommateur — travaille dans les deux fichiers)

## Prérequis
- [ ] Brief Fonctionnel `specs/$ARGUMENTS/EPIC.md` disponible et en statut `Approved`
- [ ] User stories `specs/$ARGUMENTS/stories/` disponibles
- [ ] `rules/figma.md` consulté avant de démarrer
- [ ] `/figma-read-design` exécuté — structure et tokens existants identifiés
- [ ] MCP Figma connecté et authentifié

## Gestion des erreurs

Si les prérequis ne sont pas remplis :
> ❌ Brief Fonctionnel manquant — lancer d'abord `/write-brief-fonctionnel $ARGUMENTS`.

Si MCP Figma n'est pas accessible :
> ⚠️ MCP Figma indisponible — produire les instructions manuelles de création de structure à transmettre au designer.

## Processus de génération

### Étape 1 — Lire le scope fonctionnel
Depuis `specs/$ARGUMENTS/EPIC.md` et `specs/$ARGUMENTS/stories/`, extraire :
- Le nom du projet et de l'epic
- La liste des `US-###` et leurs titres
- Les plateformes cibles (iOS / Android / Les deux)

### Étape 2 — Créer le fichier Design System via `use_figma`

Utiliser MCP Figma pour créer ou vérifier l'existence du fichier `[nom_projet]_design_system`.

Pages à créer dans l'ordre :
1. `🎨 foundations` — tokens primitifs et sémantiques
2. `🧩 components` — bibliothèque de composants
3. `📐 patterns` — assemblages réutilisables
4. `📖 documentation` — changelog et guidelines

Sur chaque page, créer un frame de titre avec :
```
[Nom de la page]
Version : 0.1.0 — Draft
Dernière mise à jour : [YYYY-MM-DD]
```

### Étape 3 — Créer le fichier Projet via `use_figma`

Créer le fichier `[NomProjet] — [EPIC-###] [NomEpic]`.

Pages à créer dans l'ordre :
1. `🗺️ user_flows` — une section par `FLUX-###` identifié dans l'epic
2. `📱 screens` — une section par `US-###` avec le titre de la story
3. `🔧 components` — composants locaux à la feature
4. `🚀 handoff` — frames prêtes pour dev (vide au démarrage)
5. `📦 store_assets` — assets publication (vide au démarrage)

Sur la page `📱 screens`, créer pour chaque `US-###` une section nommée :
```
[US-###] [Titre de la story]
```

### Étape 4 — Générer le rapport de structure

```markdown
# Rapport Setup Figma — [EPIC-###] [NomEpic] — [YYYY-MM-DD]

## Fichiers créés
- [ ] [nom_projet]_design_system : [URL Figma]
- [ ] [NomProjet] — [EPIC-###] [NomEpic] : [URL Figma]

## Pages créées — Design System
| Page | Statut | Notes |
|------|--------|-------|
| 🎨 foundations | ✅ / ❌ | |
| 🧩 components | ✅ / ❌ | |
| 📐 patterns | ✅ / ❌ | |
| 📖 documentation | ✅ / ❌ | |

## Pages créées — Projet
| Page | Sections créées | Statut |
|------|----------------|--------|
| 🗺️ user_flows | [N] flux | ✅ / ❌ |
| 📱 screens | [N] sections US | ✅ / ❌ |
| 🔧 components | — | ✅ / ❌ |
| 🚀 handoff | — | ✅ / ❌ |
| 📦 store_assets | — | ✅ / ❌ |

## Sections Screens créées
| US | Titre | Section Figma |
|----|-------|--------------|
| US-### | [Titre] | ✅ / ❌ |
```

## Phase de validation — niveau approfondi

Avant de valider ce livrable et de passer à la suite, réponds à ces questions dans l'ordre :

1. Les deux fichiers Figma sont-ils accessibles et correctement nommés selon la convention `rules/figma.md` ? (oui / non + corrections)
2. Toutes les `US-###` de l'epic ont-elles une section dédiée dans la page `📱 screens` ? (oui / non + US manquantes)
3. Les URLs des deux fichiers sont-elles renseignées dans le rapport et à disposition des agents UX et UI ? (oui / non)
4. La nomenclature des pages est-elle exacte — emojis et libellés conformes à `rules/figma.md` ? (oui / non + corrections)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ setup-figma-project "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/figma-setup.md (rapport)

Prochaines étapes :
→ /setup-figma-tokens $ARGUMENTS
→ /setup-figma-grid $ARGUMENTS
→ Partager les URLs Figma avec les agents UX et UI
```
