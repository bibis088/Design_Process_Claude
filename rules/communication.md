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
Antoine (décision finale)
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
