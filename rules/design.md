# Conventions pour les Spécifications de Design

<!-- ✅ MODIFIÉ [A] : structure de dossiers alignée sur Option B — design-system/ séparé au niveau racine -->
## Structure des dossiers
```
design/
├── README.md                  Index de tous les designs
└── [feature-slug]/
    ├── DESIGN-BRIEF.md        Brief pour le designer
    ├── SCREENS-MAP.md         Carte de navigation des écrans
    ├── COMPONENTS.md          Composants à créer / réutiliser
    ├── screens/
    │   └── S-XX-[nom].md      Spec détaillée de chaque écran
    └── REVIEW-[date].md       Compte-rendu de revue design

design-system/                 ← Transversal — géré par le Design System Manager
├── tokens/
├── components/
└── guidelines/
```

> `design-system/` n'est pas un sous-dossier de `design/`. Il est transversal à toutes les features et géré exclusivement par l'agent `design-system-manager`.

## Numérotation des écrans
- `S-01`, `S-02`... par feature (remis à zéro pour chaque feature)
- Nom descriptif : `S-01-login`, `S-02-home`, `S-03-profile`

## Langue
- Les specs design sont en **français**
- Les noms de tokens sont en **anglais** (`color.content.primary`)
- Les noms de fichiers sont en **kebab-case anglais**
- Les statuts sont en **anglais** <!-- ✅ MODIFIÉ [D] -->

## Référencement des specs fonctionnelles
Chaque fichier de design doit référencer les US correspondantes :
```markdown
## User Stories couvertes
- [US-001](../../specs/authentication/stories/US-001-login.md) — Connexion email
- [US-002](../../specs/authentication/stories/US-002-biometric.md) — Connexion biométrique
```

## Tokens de design — règles d'usage
- **Toujours** utiliser les tokens sémantiques (`color.content.primary`)
- **Jamais** utiliser les primitives directement (`palette.gray.900`)
- **Toujours** vérifier la compatibilité dark mode

## Accessibilité — checklist minimale
Chaque spec d'écran doit documenter :
- Labels VoiceOver (iOS) / Content Description (Android) pour chaque élément interactif
- Ordre de navigation au clavier/switch
- Comportement en texte agrandi (Dynamic Type / Large Font)

## ✅ NOUVEAU [AE] — Annotations Figma accessibilité

Chaque frame Figma d'un écran spécifié doit contenir les annotations suivantes avant transmission au Dev :

| Annotation | Obligatoire | Description |
|------------|------------|-------------|
| Labels accessibles | Oui — RAAM 6.1 | Texte lu par VoiceOver/TalkBack sur chaque élément interactif |
| Ordre de focus | Oui — RAAM 7.1 | Numérotation de 1 à N sur le frame |
| Rôles des éléments | Oui — RAAM 6.1 | Bouton / Lien / Champ / Image / Décoration |
| Zones tactiles | Oui — RAAM 13.1 | Overlay 44x44pt iOS / 48x48dp Android |
| Ratios de contraste | AA — RAAM 2.2 | Ratio affiché sur textes et composants interactifs |
| Alt text images | Oui — RAAM 1.1 | Texte alternatif pour chaque image informative |

### Convention de nommage des layers Figma
```
[rôle] — [nom] — [label accessible]
Ex : Button — Connexion — "Se connecter à votre compte"
Ex : Image — Avatar — "Photo de profil de Sophie"
Ex : Input — Email — "Adresse email, champ obligatoire"
Ex : Decoration — Fond — (ignoré par VoiceOver)
```

## ✅ NOUVEAU [AH] — Liaison documentation ↔ Figma

Chaque document fonctionnel référence l'URL Figma correspondante. Chaque frame Figma référence le document fonctionnel source. Ce lien bidirectionnel est obligatoire avant le passage en `Ready for dev`.

### Docs → Figma (dans les fichiers .md)

```markdown
## Références Figma
- Frame : [URL directe — ex: https://figma.com/file/xxx?node-id=yyy]
- Statut Figma : [To Do / In Progress / Ready for dev]
- US couvertes dans ce frame : [US-###, US-###]
```

Obligatoire dans : `US-###.md`, `S-XX-[nom].md`, `SCREENS-MAP.md`

### Figma → Docs (dans la description du frame Figma)

```
Spec : S-XX-[nom] | US-###, US-### | FLUX-###
Statut : [To Do / In Progress / Ready for dev]
```

### Convention de statut Figma

| Statut | Signification | Équivalent statut doc |
|--------|--------------|----------------------|
| `To Do` | Frame créé, non designé | `Draft` |
| `In Progress` | Design en cours | `In Progress` |
| `In Review` | Prêt pour relecture PO | `In Review` |
| `Ready for dev` | Validé, handoff déclenché | `Approved` → `Dev Ready` |

## Transitions — conventions de nommage
- `push` : navigation vers un sous-niveau (→)
- `pop` : retour (←)
- `modal` : présentation modale (↑)
- `dismiss` : fermeture modale (↓)
- `replace` : remplacement de l'écran courant
- `fade` : transition en fondu

<!-- ✅ MODIFIÉ [B] : agents assignés à chaque transition de statut -->
<!-- ✅ MODIFIÉ [D] : statuts passés en anglais -->
## Statuts des designs

| Statut | Responsable | Condition de passage |
|--------|-------------|---------------------|
| `To Do` | — | Fichier créé, non démarré |
| `In Progress` | UX Designer / UI Designer | Travail en cours |
| `In Review` | UX Designer / UI Designer | Prêt à être relu |
| `Approved` | PO | Cohérence avec les critères d'acceptance validée |
| `Dev Ready` | Tech Lead | Faisabilité technique confirmée, prêt pour handoff |
| `Handed Off` | UI Designer | Fichiers transmis à l'équipe dev |

> Le passage à `Approved` requiert la validation du PO. Le passage à `Dev Ready` requiert la validation du Tech Lead.

---

## ✅ NOUVEAU [AA] — Distinction flux fonctionnel / user flow / navigation map

Ces trois documents couvrent le même parcours utilisateur à trois niveaux de lecture différents. Ils ne sont pas interchangeables.

| Document | Produit par | Niveau | Ce qu'il décrit |
|----------|------------|--------|----------------|
| Flux fonctionnel (`FLUX-###`) | Business Analyst | Logique métier | Acteurs, actions, règles métier, préconditions, postconditions — sans référence à l'interface |
| User flow | UX Designer | Séquence d'écrans | Traduction du flux fonctionnel en enchaînement d'écrans, états et transitions |
| Navigation map (`SCREENS-MAP.md`) | UX Designer | Vue globale | Carte de tous les écrans d'une feature et leurs connexions — pas un flux séquentiel |

**Règle de séquence obligatoire :**
```
BA produit FLUX-###
    → UX Designer traduit en User flow (parcours séquentiel)
    → UX Designer consolide en Navigation map (vue d'ensemble)
```

Un user flow ne peut pas être produit sans `FLUX-###` existant. Une navigation map ne peut pas être produite sans au moins un user flow validé.

---

## ✅ NOUVEAU [Z] — Template `SCREENS-MAP.md`

La navigation map est une vue d'ensemble de tous les écrans d'une feature et de leurs connexions. Ce n'est pas un flux séquentiel — c'est une carte structurelle.

```markdown
# Navigation Map — [NomFeature]

## Métadonnées
- Feature : [FEAT-###]
- UX Designer : [nom]
- Version : [x.x]
- Statut : [To Do / In Progress / In Review / Approved]

## Points d'entrée
- [Écran ou trigger qui initie la navigation dans cette feature]
- [ex: Notification push, Tab bar, Deep link `app://feature`]

## Carte des écrans

| ID | Nom de l'écran | Type | Écrans accessibles depuis ici | Transitions |
|----|---------------|------|------------------------------|-------------|
| S-01 | [Nom] | [Root / Child / Modal / Overlay] | S-02, S-03 | push, modal |
| S-02 | [Nom] | [Root / Child / Modal / Overlay] | S-01 | pop |
| S-03 | [Nom] | [Root / Child / Modal / Overlay] | S-01, S-04 | pop, push |

## Points de sortie
- [Écran ou action qui fait quitter cette feature]
- [ex: Retour au Tab bar, Deep link externe, Déconnexion]

## Écrans partagés (hors feature)
- [Écran utilisé mais appartenant à une autre feature]
- [ex: S-00-profile — appartient à FEAT-###-profile]

## User flows couverts
- [FLUX-###] — [Nom du flux] — [US-### associées]
- [FLUX-###] — [Nom du flux] — [US-### associées]

## Notes
- [Contraintes de navigation spécifiques iOS ou Android]
- [Comportements conditionnels selon le rôle utilisateur]
```

---

<!-- ✅ NOUVEAU [C] : template structuré pour REVIEW-[date].md -->
## Template — `REVIEW-[date].md`

```markdown
# Design Review — [Feature] — [Date]

## Participants
| Rôle | Nom |
|------|-----|
| PO | ... |
| UX Designer | ... |
| UI Designer | ... |
| Tech Lead | ... |

## Écrans reviewés
| Écran | Statut avant | Statut après | Commentaire |
|-------|-------------|-------------|-------------|
| S-01-login | In Review | Approved | — |
| S-02-home | In Review | In Progress | Voir points bloquants |

## Décisions prises
- [Décision 1] — validée par [qui]
- [Décision 2] — validée par [qui]

## Points bloquants
| # | Description | Impact | Responsable | Deadline |
|---|-------------|--------|-------------|---------|
| 1 | [Description du blocage] | [Écrans concernés] | [Agent/personne] | [Date] |

## Actions
| # | Action | Responsable | Deadline |
|---|--------|-------------|---------|
| 1 | [Action à réaliser] | [Agent/personne] | [Date] |

## Prochaine review
- Date : [Date]
- Écrans concernés : [Liste]
```
