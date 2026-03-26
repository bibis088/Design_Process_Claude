# Conventions pour les Spécifications Fonctionnelles

<!-- ✅ MODIFIÉ [E] : numérotation des épics alignée sur la convention inter-agents EPIC-### -->
## Structure des dossiers
```
specs/
├── README.md                      Index de tous les épics
├── [epic-slug]/                   Ex : EPIC-001-authentication/
│   ├── EPIC.md                    Brief fonctionnel de l'épic
│   ├── personas/
│   │   └── PERSONA-###-[slug].md
│   ├── features/
│   │   └── FEAT-###-[slug].md
│   ├── stories/
│   │   └── US-###-[slug].md
│   └── qa/                        Rapports de test — QA Engineer
│       └── QA-###-[slug].md
└── backlog/                       Zone de staging client (voir section dédiée)
    └── US-XXX-[slug].md
```

## Numérotation
<!-- ✅ MODIFIÉ [E] : épics numérotés EPIC-### pour cohérence avec la convention inter-agents -->
- Épics : `EPIC-###` + slug descriptif (`EPIC-001-authentication`)
- Features : `FEAT-###` (global, pas par épic)
- User Stories : `US-###` (global, pas par feature)

> Convention partagée avec tous les agents (BA, PO, UX, UI, QA). Voir `agents/business-analyst.md` section Conventions de nommage.

## Langue
- Les specs sont rédigées en **français**
- Les noms de fichiers et slugs sont en **kebab-case anglais**
- Le code dans les specs est en **anglais**
- Les statuts sont en **anglais** <!-- ✅ MODIFIÉ [G] : uniformisation confirmée -->

## Critères d'acceptance
Un critère d'acceptance valide :
- ✅ "L'utilisateur voit un message d'erreur si le champ email est vide"
- ✅ "Le bouton est désactivé pendant le chargement"
- ❌ "Le formulaire fonctionne bien" (trop vague)
- ❌ "L'API retourne 200" (niveau technique, pas fonctionnel)

## Estimation T-shirt
| Taille | Points Scrum | Description |
|--------|-------------|-------------|
| XS | 1 | < 2h de dev, aucune inconnue |
| S | 2 | Demi-journée, peu de complexité |
| M | 3-5 | 1-2 jours, quelques inconnues |
| L | 8 | 3-5 jours, complexité notable |
| XL | 13+ | > 1 semaine, à redécouper si possible |

## Priorités MoSCoW
- **Must** : bloquant MVP, sans ça le produit ne peut pas sortir
- **Should** : important, prévu pour V1 si temps disponible
- **Could** : nice-to-have, backlog V2
- **Won't** : explicitement hors scope, documenté pour éviter les discussions

## Statuts des documents
`Draft` → `In Review` → `Approved` → `In Development` → `Done`

<!-- ✅ NOUVEAU [F] : documentation du dossier backlog/ comme zone de staging client -->
## Backlog — zone de staging client

Le dossier `backlog/` est une **zone de staging** qui accueille les besoins fonctionnels exprimés par le client avant qu'ils soient transformés en épics et user stories formels.

### Objectif
Capturer les besoins bruts dans un format simplifié pour validation client, avant de les structurer en `EPIC-###` / `US-###`.

### Format d'un item backlog

```markdown
# [NEED-###] Titre du besoin

## Exprimé par
- Client / Stakeholder : [Nom]
- Date : [Date]

## Besoin
[Description du besoin en langage naturel, tel qu'exprimé par le client]

## Valeur attendue
[Pourquoi ce besoin est important pour le client]

## Critères de validation client
- [ ] [Ce que le client veut voir pour considérer ce besoin satisfait]
- [ ] [Autre critère]

## Statut
`To Validate` → `Validated` → `Converted` → `Rejected`

## Conversion
- Épic créé : [EPIC-###] (une fois validé)
- US créées : [US-###, US-###] (une fois découpé)
```

### Règles
- Un item `backlog/` n'est **jamais** directement développé — il doit d'abord être converti en `EPIC-###` + `US-###`
- La conversion est déclenchée par l'agent `business-analyst` après validation client
- Une fois converti, l'item est marqué `Converted` et référence l'épic créé — il n'est pas supprimé
- Un item `Rejected` reste dans le backlog avec la raison du rejet documentée

---

## ✅ NOUVEAU [3] — Versioning des documents fonctionnels

Les documents fonctionnels (EPIC, US, FEAT) sont versionnés pour tracer les évolutions en cours de projet.

### Convention de version

```
MAJOR.MINOR
```

| Type | Quand incrémenter | Exemple |
|------|------------------|---------|
| `MAJOR` | Changement de scope, refonte du flux, suppression de critères | `1.0 → 2.0` |
| `MINOR` | Ajout de critères d'acceptance, clarification, correction | `1.0 → 1.1` |

### Format dans chaque document

Ajouter dans les métadonnées de chaque EPIC, US et FEAT :

```markdown
## Métadonnées
- Version : [MAJOR.MINOR]
- Créé le : [YYYY-MM-DD]
- Dernière mise à jour : [YYYY-MM-DD]
- Modifié par : [agent ou Senior Lead Designer]
- Changements : [description courte du dernier changement]
```

### Changelog fonctionnel

Chaque EPIC dispose d'un mini-changelog dans son fichier `EPIC.md` :

```markdown
## Changelog

| Version | Date | Auteur | Changement |
|---------|------|--------|-----------|
| 1.1 | YYYY-MM-DD | PO | Ajout CA-04 sur US-042 |
| 1.0 | YYYY-MM-DD | BA | Création initiale |
```

### Règle de validation humaine sur les changements de version MAJOR

Tout passage de version MAJOR sur un EPIC ou une US nécessite une validation explicite du **Senior Lead Designer** avant d'être appliqué — c'est un changement de scope.

---

## ✅ NOUVEAU [4] — Dépendances inter-features

Plusieurs features peuvent partager des composants ou dépendre les unes des autres.

### Déclaration dans le template EPIC

```markdown
## Dépendances inter-features

### Cette feature dépend de
| EPIC | Feature | Livrable requis | Statut |
|------|---------|----------------|--------|
| EPIC-001 | FEAT-003 | Composant `Button/Primary` | ✅ Disponible |
| EPIC-001 | FEAT-005 | API authentification | ⏳ En cours |

### Cette feature est requise par
| EPIC | Feature | Livrable fourni | Statut |
|------|---------|----------------|--------|
| EPIC-003 | FEAT-008 | Composant `Card/Product` | 🔒 Bloquant |
```

### Règles de gestion des dépendances

- Une feature ne peut pas démarrer si ses dépendances bloquantes ne sont pas en statut `Done`
- Une dépendance non bloquante peut démarrer avec un mock ou placeholder — documenter explicitement
- Si une dépendance change de scope (version MAJOR), toutes les features qui en dépendent doivent être réévaluées
- Le **Senior Lead Designer** arbitre les conflits de dépendances inter-features

### Conventions de statut des dépendances

| Statut | Signification |
|--------|--------------|
| ✅ Disponible | Livrable prêt et disponible |
| ⏳ En cours | En développement — date estimée connue |
| 🔒 Bloquant | Feature bloquée — ne peut pas démarrer |
| ⚠️ Mock | Livrable remplacé par un mock temporaire |
| ❌ Absent | Livrable non démarré — à planifier |
