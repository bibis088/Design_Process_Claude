---
name: write-functional-doc
description: Génère le document fonctionnel complet d'un EPIC — matrice de traçabilité EPIC→FEAT→US→écran Figma, flux fonctionnels avec liens, règles métier, personas validés, glossaire, changelog. Document unique client + Dev. Exécuté par le product-owner après la Gate 9.
argument-hint: [epic-slug]
disable-model-invocation: false
context: fork
agent: product-owner
---

## Rôle
Produire le document fonctionnel de référence d'un EPIC — relie toutes les dimensions du projet (fonctionnel, design, Figma, prototype) en un seul document Google Doc exploitable par le client et l'équipe Dev.

## Agents consommateurs
- Product Owner (pilote — synthétise)
- Product Designer (partage au client)
- Dev iOS / Android (référence d'implémentation)

## Prérequis
- [ ] Gate 9 validée — review scope Figma approuvée
- [ ] Tous les `US-###` en statut `Approved`
- [ ] Frames Figma en statut `Ready for dev`
- [ ] Prototype React disponible (`write-prototype-react`)
- [ ] Plan de tagage révisé disponible (`review-tracking-plan` — v finale après Gate 8)
- [ ] Insights de recherche disponibles (`write-research-insights`)

## Gestion des erreurs

Si des US ne sont pas encore en statut `Approved` :
> ⚠️ Document partiel — documenter explicitement les US en attente.

## Processus de génération

### Étape 1 — Collecter tous les inputs

Depuis les dossiers du projet, extraire :
- `specs/[epic-slug]/EPIC.md` — brief fonctionnel
- `specs/[epic-slug]/stories/US-###.md` × N — user stories
- `specs/[epic-slug]/features/FEAT-###.md` × N — features
- `specs/[epic-slug]/regles-metier.md` — règles métier
- `specs/[epic-slug]/glossaire.md` — glossaire
- `specs/[epic-slug]/personas/PERSONA-###.md` × N — personas validés
- `specs/[epic-slug]/research/insights.md` — insights research
- `design/[feature-slug]/SCREENS-MAP.md` — navigation map
- URLs Figma depuis les specs d'écran
- URL(s) prototype React

### Étape 2 — Générer le document

```markdown
# Documentation Fonctionnelle — [EPIC-###] [NomProjet]
**Version :** [MAJOR.MINOR] — [YYYY-MM-DD]
**Statut :** [Draft / In Review / Approved]
**Auteur :** Product Owner

---

## Résumé exécutif

### Contexte
[2-3 phrases — problème résolu, contexte business, opportunité]

### Objectifs du projet
| Objectif | KPI | Cible |
|----------|-----|-------|
| [Objectif 1] | [KPI] | [Valeur cible] |

### Stack technique
- Type : [Native iOS / Android / Cross-platform / Hybride]
- Plateformes : [iOS XX+ / Android XX+]
- Contraintes : [Biométrie, offline, notifications...]

### Périmètre V1
**Dans le scope :**
- [Fonctionnalité 1]
- [Fonctionnalité 2]

**Hors scope (V2+) :**
- [Fonctionnalité reportée]

---

## Personas

### [PERSONA-001] [Prénom]
- **Rôle :** [Rôle]
- **Profil :** [Âge, maturité tech, contexte d'usage]
- **Objectif principal :** [Ce qu'il veut accomplir]
- **Frustration principale :** [Pain point résolu par ce produit]
- **Citation :** "[Citation représentative]"
- **Statut :** [Hypothétique / Validé par research]

[Répéter pour chaque persona]

---

## Matrice de traçabilité

| EPIC | FEAT | US | Titre US | Écran Figma | URL Figma | Prototype | Statut |
|------|------|----|----------|------------|-----------|-----------|--------|
| [EPIC-###] | [FEAT-###] | [US-###] | [Titre] | S-XX | [URL] | [URL proto] | Ready for dev |

---

## Features et User Stories

### [FEAT-###] — [Titre de la feature]
**Description :** [Description courte]
**Priorité MoSCoW :** [Must / Should / Could / Won't]

#### [US-###] — [Titre]
**En tant que** [persona],
**Je veux** [action],
**Afin de** [bénéfice].

**Critères d'acceptance :**
- [ ] CA-01 : [Critère testable]
- [ ] CA-02 : [Critère testable]

**Références design :**
- Écran principal : [S-XX] — [URL Figma directe]
- États : [Loading / Empty / Error — URLs]

**Dépendances :**
- Bloqué par : [US-### si applicable]

---

[Répéter pour chaque FEAT et US]

---

## Flux fonctionnels

### [FLUX-###] — [Nom du flux]
**Description :** [Ce que ce flux accomplit]
**Persona(s) concerné(s) :** [PERSONA-###]
**User flow Figma :** [URL page 🗺️ user_flows]
**Prototype :** [URL fichier HTML]

**Flux principal :**
```
[Point d'entrée] → [Étape 1] → [Étape 2] → [Point de sortie]
```

**Flux alternatifs :**
- ALT-01 : [Description — URL frame correspondante]

**Cas d'erreur :**
- ERR-01 : [Description — URL frame correspondante]

---

[Répéter pour chaque FLUX-###]

---

## Règles métier

| ID | Règle | Catégorie | US concernées |
|----|-------|-----------|--------------|
| RB-001 | [Règle] | [Validation / Sécurité / Métier] | [US-###] |

---

## Glossaire

| Terme | Définition | Contexte d'usage |
|-------|-----------|-----------------|
| [Terme] | [Définition] | [US-###, écran S-XX] |

---

## Liens et ressources

### Figma
- Fichier Projet : [URL]
- Fichier Design System : [URL]
- Page Handoff : [URL]

### Prototypes
| Flow | Version | URL | Date |
|------|---------|-----|------|
| [FLUX-###] | v1 — Cliquable | [URL HTML] | [Date] |
| [FLUX-###] | v2 — Interactif | [URL HTML] | [Date] |

### Research
- Analyse concurrentielle : `specs/[epic-slug]/research/competitive-analysis.md`
- Insights utilisateurs : `specs/[epic-slug]/research/insights.md`

---

## Changelog

| Version | Date | Auteur | Changement |
|---------|------|--------|-----------|
| [MAJOR.MINOR] | [YYYY-MM-DD] | [Agent / Product Designer] | [Description] |
```

## Phase de validation — niveau approfondi

Avant de valider ce livrable et de passer à la suite, réponds à ces questions dans l'ordre :

1. La matrice de traçabilité couvre-t-elle toutes les US en statut `Approved` — aucune manquante ? (oui / non + US manquantes)
2. Chaque US référence-t-elle l'URL Figma directe de son écran principal ? (oui / non + US sans URL)
3. Les prototypes React sont-ils référencés pour chaque flux ? (oui / non + flux sans prototype)
4. Le changelog reflète-t-il l'historique réel des versions du document ? (oui / non)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-functional-doc "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/functional-doc-v[MAJOR.MINOR].md

Prochaines étapes :
→ /write-client-deliverable $ARGUMENTS/functional-doc (version client)
→ /write-figma-handoff $ARGUMENTS (handoff Dev)
→ Partager au client pour validation finale
```
