---
name: process-watch
description: Veille hebdomadaire automatique sur les mises à jour des référentiels — RAAM, Apple HIG, Material Design 3, MCP Figma, Claude/Anthropic. Identifie l'impact sur les agents, rules et skills. Exécuté par le business-analyst.
argument-hint: [YYYY-MM-DD]
disable-model-invocation: false
context: fork
agent: business-analyst
---

## Rôle
Surveiller les mises à jour des référentiels externes et produire un rapport hebdomadaire d'impact sur le process — quels agents, rules ou skills doivent être mis à jour suite aux évolutions détectées.

## Fréquence
Hebdomadaire — à déclencher chaque semaine.

## Sources surveillées

| Source | URL | Ce qu'on surveille |
|--------|-----|-------------------|
| RAAM | https://accessibilite.numerique.gouv.fr/raam/ | Nouveaux critères, modifications de niveau A/AA |
| Apple HIG | https://developer.apple.com/design/human-interface-guidelines/ | Nouvelles guidelines, composants dépréciés |
| Material Design 3 | https://m3.material.io/whats-new | Nouveaux composants, tokens, patterns |
| MCP Figma changelog | https://github.com/figma/mcp-server-guide | Nouveaux outils, skills officilels, breaking changes |
| Claude / Anthropic | https://www.anthropic.com/news | Nouvelles versions de modèles, nouvelles capacités |

## Gestion des erreurs

Si une source est inaccessible :
> ⚠️ Source inaccessible — documenter dans le rapport et réessayer la semaine suivante.

## Processus de veille

### Étape 1 — Récupérer les dernières mises à jour

Pour chaque source, utiliser `web_search` + `web_fetch` :

```
web_search: "RAAM accessibilité mobile nouveautés [année en cours]"
web_search: "Apple HIG updates [mois année]"
web_search: "Material Design 3 whats new [mois année]"
web_search: "figma mcp-server-guide changelog [mois année]"
web_search: "Anthropic Claude new model release [mois année]"
```

### Étape 2 — Analyser l'impact sur le process

Pour chaque mise à jour détectée, identifier :
- Quel fichier du process est impacté (agent, rule, skill)
- Quelle section précisément
- Niveau d'urgence (Bloquant / Important / Informatif)

### Étape 3 — Produire le rapport

```markdown
# Rapport Veille Process — Semaine du [YYYY-MM-DD]

## Résumé
[N] mises à jour détectées — [N] bloquantes, [N] importantes, [N] informatives

---

## RAAM
| Mise à jour | Impact | Fichiers concernés | Urgence |
|------------|--------|-------------------|---------|
| [Description] | [Impact sur nos critères] | `rules/accessibility.md` | [Bloquant/Important/Info] |

## Apple HIG
| Mise à jour | Impact | Fichiers concernés | Urgence |
|------------|--------|-------------------|---------|
| [Description] | [Impact sur guidelines] | `rules/figma.md`, `skills/check-guidelines-compliance.md` | [Niveau] |

## Material Design 3
| Mise à jour | Impact | Fichiers concernés | Urgence |
|------------|--------|-------------------|---------|
| [Description] | [Impact sur guidelines] | `rules/figma.md` | [Niveau] |

## MCP Figma
| Mise à jour | Impact | Fichiers concernés | Urgence |
|------------|--------|-------------------|---------|
| [Nouvel outil / skill officiel] | [Impact sur nos skills Figma] | `rules/figma.md`, `skills/figma-use-wrapper.md` | [Niveau] |

## Claude / Anthropic
| Mise à jour | Impact | Fichiers concernés | Urgence |
|------------|--------|-------------------|---------|
| [Nouveau modèle / capacité] | [Impact sur les agents] | `agents/*.md` (model: field) | [Niveau] |

---

## Actions recommandées

### 🔴 Bloquant — à traiter cette semaine
| # | Action | Fichier | Skill de mise à jour |
|---|--------|---------|---------------------|
| 1 | [Mise à jour requise] | [Fichier] | [Skill à utiliser ou modification manuelle] |

### 🟡 Important — à planifier dans les 2 semaines
| # | Action | Fichier | Note |
|---|--------|---------|------|
| 1 | [Mise à jour recommandée] | [Fichier] | [Détail] |

### 🟢 Informatif — à garder en tête
| # | Info | Source | Note |
|---|------|--------|------|
| 1 | [Information utile] | [Source] | [Contexte] |

---

## Historique des rapports
- [Semaine précédente] : [N] mises à jour, [N] actions réalisées
```

### Étape 4 — Demande de validation humaine si action bloquante

Si au moins une action bloquante est détectée :

```
"⚠️ Mise à jour critique détectée cette semaine.

Source : [nom]
Changement : [description]
Impact : [fichiers concernés]

Confirmes-tu que je mette à jour [fichier] pour intégrer ce changement ?"
```

## Phase de validation — niveau léger

Avant de clore ce skill, confirme ce point :

1. Toutes les sources ont-elles été consultées cette semaine — aucune inaccessible sans note dans le rapport ? (oui / non + sources non consultées)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ process-watch "[YYYY-MM-DD]" terminé
📁 specs/process-watch/rapport-[YYYY-MM-DD].md

Prochaines étapes :
→ Traiter les actions bloquantes (validation Product Designer requise)
→ Planifier les actions importantes
→ Prochain rapport dans 7 jours
```
