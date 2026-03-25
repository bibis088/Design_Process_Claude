# Conventions de communication entre agents

## Principe fondamental

Les agents ne communiquent pas librement entre eux — ils échangent des **documents structurés** via des chemins de fichiers définis. Un agent ne peut pas demander à un autre agent de produire quelque chose sans que le document source soit disponible dans le bon dossier.

```
Un agent lit → produit → dépose dans le bon dossier → notifie l'agent aval
```

---

## Chaîne de communication standard

```
backlog/ (client)
    ↓ [BACKLOG-### Validated]
business-analyst
    ↓ EPIC.md + FLUX-###.md + PERSONA-###.md → specs/[epic-slug]/
product-owner
    ↓ FEAT-###.md + US-###.md → specs/[epic-slug]/features/ + stories/
ux-designer
    ↓ SCREENS-MAP.md + user-flows + S-XX.md → design/[feature-slug]/ux/
ui-designer
    ↓ composants + écrans → design/[feature-slug]/ui/
design-system-manager
    ↓ tokens + composants + guidelines → design-system/
qa
    ↓ rapports de test → specs/[epic-slug]/qa/
```

---

## ✅ NOUVEAU [AH] — Figma comme canal de transmission

Figma est un canal de communication bidirectionnel entre les agents design et les agents Dev/QA. Il ne remplace pas les fichiers `.md` — il les complète.

### Règles de transmission via Figma

| Règle | Description |
|-------|-------------|
| Lien bidirectionnel obligatoire | Chaque spec `.md` référence l'URL Figma — chaque frame Figma référence la spec |
| Statut synchronisé | Le statut Figma (`To Do / In Progress / Ready for dev`) doit être cohérent avec le statut du document |
| Annotations avant handoff | Les annotations RAAM (labels, ordre de focus, zones tactiles) doivent être complètes avant `Ready for dev` |
| Jamais de Figma sans spec | Un frame Figma ne peut pas être en `Ready for dev` si la spec `.md` correspondante n'est pas en `Approved` |

### Format de description obligatoire dans chaque frame Figma

```
Spec : S-XX-[nom] | US-###, US-### | FLUX-###
Statut : [To Do / In Progress / Ready for dev]
Annotations RAAM : [To Do / Done]
```

### Notifications via Figma

Quand un frame passe en `Ready for dev` dans Figma :
1. L'UI Designer est notifié pour l'implémentation
2. Le QA Engineer est notifié pour préparer le plan de test
3. Le PO est notifié pour vérification finale

---

## Format de transmission entre agents

Quand un agent termine un livrable et le transmet à l'agent aval, il produit une **note de transmission** dans le fichier concerné :

```markdown
## Transmission

- De : [agent-source]
- À : [agent-destinataire]
- Date : [YYYY-MM-DD]
- Documents transmis :
  - [chemin/vers/fichier.md] — [description courte]
- Statut : [Approved / In Review]
- Points d'attention :
  - [Point spécifique que l'agent aval doit prendre en compte]
```

---

## Règles de communication

### 1. Pas de transmission sans statut `Approved`
Un agent ne peut pas transmettre un document à l'agent aval si ce document n'est pas en statut `Approved`. Exception : transmission explicite pour relecture (`In Review`), clairement indiquée comme telle.

### 2. Jamais de modification sans notification
Si un agent modifie un document produit par un autre agent (ex: le PO enrichit une règle métier du BA), il doit :
- Marquer la modification avec un tag `<!-- MODIFIÉ PAR [agent] — [YYYY-MM-DD] -->`
- Notifier l'agent source dans la note de transmission

### 3. Les questions ouvertes ont un destinataire
Toute question ouverte dans un document doit être adressée à un agent ou une personne spécifique :

```markdown
## Questions ouvertes
| # | Question | Destinataire | Deadline | Statut |
|---|----------|-------------|---------|--------|
| 1 | [Question] | [agent ou Antoine] | [YYYY-MM-DD] | [Open / Resolved] |
```

### 4. Les conflits se résolvent par escalade
Si deux agents produisent des documents contradictoires (ex: règle métier BA vs critère d'acceptance PO), la résolution suit cette hiérarchie :

```
Humain (décision finale)
    ↑
PO (arbitrage fonctionnel)
    ↑
BA (arbitrage métier)
    ↑
Agents spécialisés (UX, UI, DSM, QA)
```

### 5. Un document par responsable
Chaque document a un **agent responsable unique**. Les autres agents peuvent enrichir mais pas remplacer le responsable.

| Document | Agent responsable |
|----------|------------------|
| `EPIC.md`, `FLUX-###`, `PERSONA-###`, `RB-###` | Business Analyst |
| `FEAT-###`, `US-###` | Product Owner |
| `SCREENS-MAP.md`, `S-XX.md`, user flows | UX Designer |
| Composants UI, écrans UI | UI Designer |
| Tokens, composants DS, guidelines | Design System Manager |
| Rapports de test, bugs | QA |

---

## Conventions de nommage des échanges

### Demande d'ajout au design system
Quand un agent UI ou UX identifie un token ou composant manquant :

```markdown
## Demande design system — [YYYY-MM-DD]

- Demandeur : [agent]
- Type : [Token / Composant]
- Nom proposé : [nom.semantique.propose]
- Usage : [description précise du contexte d'utilisation]
- Valeur suggérée : [si connue]
- Feature concernée : [FEAT-###]
- Urgence : [Bloquant / Non bloquant]
```

### Signalement de conflit entre documents
Quand un agent détecte une incohérence entre deux documents :

```markdown
## Conflit détecté — [YYYY-MM-DD]

- Détecté par : [agent]
- Document A : [chemin] — [description du contenu]
- Document B : [chemin] — [description du contenu contradictoire]
- Impact : [ce que ça bloque]
- Proposition de résolution : [si l'agent a une suggestion]
- Escalade à : [PO / Antoine]
```

---

## Statuts de communication

| Statut | Signification |
|--------|--------------|
| `Pending` | Document produit, en attente de transmission |
| `Transmitted` | Document transmis à l'agent aval |
| `Acknowledged` | Agent aval a accusé réception |
| `In Review` | Agent aval en cours de relecture |
| `Approved` | Document validé, peut être consommé |
| `Rejected` | Document refusé — retour à l'agent source avec motif |
| `Conflict` | Contradiction détectée — escalade requise |

---

## ✅ NOUVEAU [5] — Gates de validation humaine

Le **Humain** est le valideur final à chaque phase du process. Son rôle est de vérifier et valider l'ensemble des décisions — il ne produit pas de documents, il approuve ou bloque.

### Définition du rôle Humain
- Valide les livrables **en lot à la fin de chaque phase** — pas livrable par livrable
- Arbitre les conflits entre agents
- Décide des changements de scope en cours de projet ou sprint
- Réalise la direction artistique (étape manuelle dans le process UI)

---

### 11 Gates — détail par phase

#### Gate 0 — Initialisation
**Déclenchée après :** `/write-cadrage` + `/write-persona` × N
**Lot soumis :**
- Résumé de cadrage (problème, stack technique, périmètre V1)
- Personas (min. 2 — utilisateur régulier + nouvel utilisateur)

**Question :** "Le cadrage et les personas reflètent-ils bien le contexte du projet ?"
**Bloque :** le Brief Fonctionnel ne démarre pas sans validation

---

#### Gate 1 — Spécification fonctionnelle
**Déclenchée après :** `/write-brief-fonctionnel` + `/write-flux-fonctionnel` × N + `/write-regles-metier` + `/write-glossaire`
**Lot soumis :**
- Brief Fonctionnel (`EPIC.md`)
- Flux fonctionnels (`FLUX-###` × N)
- Matrice de règles métier (`RB-###`)
- Glossaire

**Question :** "Les specs fonctionnelles sont-elles complètes et fidèles au besoin métier ?"
**Bloque :** les user stories ne démarrent pas sans validation

---

#### Gate 2 — User stories
**Déclenchée après :** `/write-feature-ticket` + `/write-user-story` × N (toutes les US d'une feature)
**Lot soumis :**
- Feature ticket (`FEAT-###`)
- Toutes les user stories (`US-###` × N) avec leurs critères d'acceptance

**Question :** "Les stories couvrent-elles le périmètre attendu ? Les critères d'acceptance sont-ils testables et non ambigus ?"
**Bloque :** le process UX ne démarre pas sans validation

---

#### Gate 3 — Setup Figma
**Déclenchée après :** `/setup-figma-project` + `/setup-figma-tokens` + `/setup-figma-grid`
**Lot soumis :**
- URLs des fichiers Figma Projet et Design System
- Tokens configurés (couleurs, typo, spacing) — lien vers la page Foundations
- Frames de base iOS/Android avec grilles actives

**Question :** "La structure Figma est-elle prête pour que l'équipe design démarre ?"
**Bloque :** le travail UX dans Figma ne démarre pas sans validation

---

#### Gate 4 — UX Design
**Déclenchée après :** `/write-navigation-map` + `/write-user-flow` × N + `/write-figma-userflow` × N + `/write-screen-spec` × N + `/write-accessibility-spec`
**Lot soumis :**
- Navigation map (`SCREENS-MAP.md`)
- User flows visuels dans Figma (page 🗺️ User Flows)
- Specs de tous les écrans (`S-XX` × N)
- Spec accessibilité RAAM de la feature

**Question :** "Les parcours UX et les specs d'écran sont-ils conformes à l'intention fonctionnelle ?"
**Bloque :** la création des composants UI ne démarre pas sans validation

---

#### Gate 5 — Composants UI
**Déclenchée après :** `/create-figma-component` × N + `/check-guidelines-compliance` × N
**Lot soumis :**
- Tous les composants Figma avec variants, tokens et auto layout
- Rapport de conformité HIG/M3 (sans non-conformité bloquante)

**Question :** "Les composants sont-ils visuellement cohérents et conformes aux guidelines HIG/Material 3 ?"
**Bloque :** la direction artistique ne démarre pas sans validation

---

#### Gate 6 — Direction artistique *(manuelle)*
**Pas de lot** — le Humain intervient directement dans Figma.
**Ce qu'il fait :** définit l'identité visuelle, crée les écrans principaux, valide la cohérence de marque.
**Validation implicite :** les écrans principaux sont créés et visibles dans Figma.
**Bloque :** la création des frames vides automatiques ne démarre pas sans que cette étape soit réalisée

---

#### Gate 7 — Écrans principaux
**Déclenchée après :** création des frames Default iOS/Android pour tous les écrans happy path + revue accessibilité RAAM #1
**Lot soumis :**
- Frames Default iOS + Android pour toutes les US (happy path)
- Résultat revue accessibilité RAAM niveau A sur les écrans principaux

**Question :** "Les écrans principaux sont-ils conformes aux specs UX et à la direction artistique ?"
**Bloque :** l'injection de contenu et les écrans secondaires ne démarrent pas sans validation

---

#### Gate 8 — Écrans secondaires
**Déclenchée après :** `/fetch-content-for-frames` + `/setup-figma-frames` (écrans secondaires) + revue accessibilité RAAM #2
**Lot soumis :**
- Frames Loading / Empty / Error pour tous les écrans
- Rapport d'injection de contenu (contenu réel vs placeholders restants)
- Résultat revue accessibilité RAAM niveau A sur les états dégradés

**Question :** "Les états dégradés sont-ils traités correctement ? Le contenu est-il réaliste ?"
**Bloque :** la validation fonctionnelle Figma ne démarre pas sans validation

---

#### Gate 9 — Validation fonctionnelle Figma
**Déclenchée après :** `/review-figma-scope`
**Lot soumis :**
- Rapport review scope avec verdict Pass/Fail

**Question :** "Les frames couvrent-elles l'intégralité du scope EPIC/US/CA défini ?"
**Bloque :** le handoff ne démarre pas sans validation

---

#### Gate 10 — Handoff
**Déclenchée après :** `/figma-code-connect` + `/write-figma-handoff`
**Lot soumis :**
- Rapport Code Connect (composants connectés ↔ code)
- Fichier Figma page `🚀 Handoff` prête
- Document handoff avec URLs, composants utilisés, notes Dev

**Question :** "Le handoff est-il complet et transmissible à l'équipe Dev sans ambiguïté ?"
**Bloque :** la transmission aux Dev ne se fait pas sans validation

---

#### Gate 11 — QA
**Déclenchée après :** `/write-qa-report` × N (toutes les US de la feature)
**Lot soumis :**
- Rapports QA pour toutes les US (`QA-###` × N)
- Verdict global : Pass / Fail / Approuvé avec réserves
- Liste des bugs bloquants si applicable

**Question :** "Les résultats QA sont-ils acceptables pour passer en production ?"
**Bloque :** le passage en `Done` et la soumission store ne se font pas sans validation

---

### Format de demande de validation humaine en lot

À chaque gate, l'agent produit ce résumé structuré :

```markdown
## ⚠️ Validation Humaine requise — Gate [#] [NomPhase] — [YYYY-MM-DD]

### Livrables produits dans cette phase
| Livrable | ID | Statut | Lien |
|----------|----|---------|----- |
| [Type] | [ID] | In Review | [URL ou chemin] |

### Résumé de la phase
[3-5 phrases — ce qui a été produit, les décisions prises, les points d'attention]

### Points nécessitant ton regard
- [Point spécifique 1]
- [Point spécifique 2 — décision ouverte si applicable]

### Question de validation
"[Question définie pour cette gate]"

Réponse attendue :
- ✅ "Oui" → tous les livrables passent en `Approved`, phase suivante déclenchée
- ❌ "Non" + corrections → livrables concernés retournent en `Draft`, agents relancés
- ⏳ "Reporter" → livrables mis en attente, raison documentée dans le changelog
```

---

#### Gate hors-phase — Changement de scope
Déclenché à tout moment, quelle que soit la phase en cours.
Voir template complet dans la section suivante.

---

## ✅ NOUVEAU [6] — Gestion des changements de scope

Tout changement de périmètre en cours de projet ou de sprint — ajout, suppression ou modification d'une fonctionnalité — passe obligatoirement par validation humaine.

### Template de demande de changement de scope

```markdown
## Demande de changement de scope — [YYYY-MM-DD]

### Demandeur
[Agent ou source ayant identifié le besoin de changement]

### Changement proposé
- Type : [Ajout / Suppression / Modification]
- Scope actuel : [Description de ce qui existe]
- Scope proposé : [Description de ce qui changerait]
- Raison : [Pourquoi ce changement est nécessaire]

### Impact estimé
| Dimension | Impact | Détail |
|-----------|--------|--------|
| US concernées | [N US] | [US-###, US-###] |
| Sprint en cours | [Oui / Non] | [Si oui : impact sur les engagements] |
| Design System | [Oui / Non] | [Si oui : tokens ou composants à modifier] |
| Figma | [Oui / Non] | [Frames à retravailler] |
| Version document | [MINOR / MAJOR] | [Selon l'ampleur] |

### Décision Humaine requise
"Approuves-tu ce changement de scope ?"

- ✅ Approuvé → mettre à jour les documents concernés (version MAJOR ou MINOR)
- ❌ Refusé → documenter la raison + reporter en backlog si pertinent
- ⏳ À planifier → créer un item backlog pour la prochaine itération
```
