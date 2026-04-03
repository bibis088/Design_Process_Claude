---
name: setup-figma-project
description: "Crée la structure de pages des fichiers Figma Projet et Design System selon le scope fonctionnel (EPIC et US). Exécuté par le design-system-manager via `use_figma`."
argument-hint: "[epic-slug]"
agent: design-system-manager
---

## Rôle
Initialiser la structure des deux fichiers Figma du projet — Design System et Projet — avec les pages, la nomenclature et l'organisation définis dans `rules/figma.md`.

## Agents consommateurs
Design System Manager  · UX Designer  · UI Designer 

## Prérequis
- [ ] Brief Fonctionnel `specs/$ARGUMENTS/EPIC.md` disponible et en statut `Approved`
- [ ] User stories `specs/$ARGUMENTS/stories/` disponibles
- [ ] `rules/figma.md` consulté avant de démarrer
- [ ] `/figma-read-design` exécuté — structure et tokens existants identifiés
- [ ] MCP Figma connecté et authentifié
- [ ] `cadrage.md` disponible — OS de base, stack et périmètre V1 définis
- [ ] `FLUX-###` disponibles — sections user flows créées par flux
- [ ] `specs/[epic-slug]/figma-urls.md` à créer à la fin de ce skill
- [ ] Gate 2 validée — `US-###` en statut Approved

## Gestion des erreurs
> ❌ Brief Fonctionnel manquant — lancer d'abord `/write-brief-fonctionnel $ARGUMENTS`.
> ⚠️ MCP Figma indisponible — produire les instructions manuelles de création de structure à transmettre au designer.

## Processus de génération

### Étape 0 — Analyser les fichiers existants (si fournis)

Si des URLs Figma ont été fournis dans `setup-figma-init`, analyser chaque fichier avant de créer quoi que ce soit.

**Fichier Projet existant :**
```
get_metadata [URL Projet]
→ Inventaire des pages existantes vs structure attendue
→ Vérification nomenclature des pages (snake_case / noms corrects)
→ Liste des sections et frames dans Work In Progress UI
→ Vérification nomenclature frames (US_###_nom_os_état)
→ Score de conformité structure /10
```

**Fichier Design System existant :**
```
get_metadata [URL DS]
→ Inventaire des pages (foundations / components / patterns / documentation)
→ Vérification nomenclature des pages
get_variable_defs [URL DS]
→ Liste exhaustive des tokens existants (couleurs, spacing, radius, typo)
→ Vérification modes light/dark
search_design_system [URL DS]
→ Liste exhaustive de tous les composants publiés avec leurs variants
→ Vérification nomenclature composants (snake_case + /)
```

Produire le rapport d'analyse avant de continuer :
```markdown
## Analyse fichiers existants — [YYYY-MM-DD]

### Fichier Projet
Pages : [N] trouvées / [N] attendues
Conformité pages : ✅ / ⚠️ / ❌ par page
Frames : nomenclature [N]% conforme

### Fichier Design System
Pages : [N] trouvées / [N] attendues
Tokens : [N] variables — light ✅/❌ · dark ✅/❌
Composants : [N] composants publiés
| Composant | Variants | Nomenclature |
|-----------|---------|-------------|
| [nom] | [N] | ✅/⚠️ |
```

### Étape 1 — Lire le scope fonctionnel
Depuis `specs/$ARGUMENTS/EPIC.md` et `specs/$ARGUMENTS/stories/`, extraire :
- Le nom du projet et de l'epic
- La liste des `US-###` et leurs titres
- Les plateformes cibles (iOS / Android / Les deux)

### Étape 2 — Créer le fichier Design System via `use_figma`

Utiliser MCP Figma pour créer ou vérifier l'existence du fichier `[nom_projet]_design_system`.

Pages à créer dans l'ordre — structure inspirée du standard Neow DS :

```
Get started            ← page d'accueil du DS (purpose, usage, contacts)
Change log             ← historique des versions

---                    ← séparateur visuel

🧱 Foundations         ← en-tête de section (pas une page de contenu)
     Colors            ← tokens couleurs primitifs + sémantiques + dark mode
     Typography        ← type scale iOS + Android + web si applicable
     Icons             ← bibliothèque d'icônes + règles d'usage
     Grid & spacing    ← grilles iOS/Android, spacing tokens
     Logo              ← assets logo + règles d'usage
     Shadows & Blur    ← élévations, ombres, blur
     Images            ← règles d'usage images, placeholders, formats
     Animations        ← tokens motion, durées, courbes easing

---                    ← séparateur visuel

⚛️ Components          ← en-tête de section
     Actions           ← boutons, FAB, liens
     Navigation        ← tab bar, nav bar, back button, drawer
     Content           ← cards, listes, tableaux, media
     TextField         ← inputs, search, textarea
     Forms             ← formulaires complets, validation
     Controls          ← checkbox, radio, toggle, switch
     Lists             ← list items, list groups
     Tabs              ← tab components, segmented control
     Avatar            ← avatars, user badges
     Feedbacks         ← toasts, snackbars, alerts, banners
     Accordion         ← collapse, expand
     Indicators        ← badges, chips, tags, progress
     Cards             ← card variants
     Chart             ← graphiques, data viz
     Slider            ← sliders, range
     Pattern           ← assemblages réutilisables (ex-patterns)
     Modals & Pop-up   ← modales, bottom sheets, popovers
     Loader            ← spinners, skeletons, progress bar
     Mock up           ← device frames pour présentations
     Native Elements   ← status bar, keyboard, system UI

---                    ← séparateur visuel

🗃️ Unused Components   ← composants archivés (ne pas supprimer — référence)
🛠️ Utilities           ← grilles, annotations, outils de design
🎇 Cover               ← thumbnail du fichier DS
```

Sur chaque page, créer un frame de titre avec :
```
[Nom de la page]
Version : 0.1.0 — Draft
Dernière mise à jour : [YYYY-MM-DD]
```

### Étape 3 — Créer le fichier Projet via `use_figma`

Créer le fichier `[NomProjet] — [EPIC-###] [NomEpic]`.

Structure complète des pages à créer dans l'ordre :

```
✏️ Design
   ├── Work In Progress UI     ← écrans UI en cours de production
   ├── Validated UI            ← écrans validés par le Product Designer
   ├── Ready for dev           ← écrans prêts pour l'implémentation (tag </> dev)
   └── Accessibility           ← annotations RAAM sur les écrans validés

🔬 Research & UX
   ├── Userflow                ← flows visuels FLUX-###
   ├── Work In Progress Wireframes ← wireframes en cours
   └── Validated wireframe     ← wireframes validés (avant direction artistique)

📚 Ressources
   ├── Moodboard               ← références visuelles et inspiration
   ├── Benchmark               ← captures et analyse UI concurrentielle
   └── Client documentation    ← docs client (brief, charte, assets fournis)

📦 Deliverables
   ├── App Icon                ← icône app (toutes tailles)
   └── Store screen            ← screenshots store iOS + Android

🗄️ Archives                   ← écrans ou versions obsolètes
🧪 Sandbox                    ← explorations libres et tests visuels
🖼️ Cover                      ← thumbnail du fichier (écran le plus représentatif)
```

**Organisation des frames dans `Work In Progress UI`**

Créer pour chaque `US-###` une section nommée :
```
US_###_[nom_us]
```

Chaque frame dans la section suit la convention snake_case :
```
US_###_[nom]_ios_default
US_###_[nom]_ios_loading
US_###_[nom]_ios_error
US_###_[nom]_android_default
```

**Flux des frames selon leur avancement**

```
Work In Progress UI  →  Validated UI  →  Ready for dev
     (en cours)           (validé PD)        (handoff)
         ↓
    Accessibility (annotations ajoutées sur les frames Validated)
```

> ⚠️ Une frame ne passe jamais de WIP directement à Ready for dev.
> Elle doit d'abord être dans Validated UI.

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

## Pages créées — Fichier Projet
| Section | Page | Statut |
|---------|------|--------|
| ✏️ Design | Work In Progress UI | ✅ / ❌ |
| ✏️ Design | Validated UI | ✅ / ❌ |
| ✏️ Design | Ready for dev | ✅ / ❌ |
| ✏️ Design | Accessibility | ✅ / ❌ |
| 🔬 Research & UX | Userflow | ✅ / ❌ |
| 🔬 Research & UX | Work In Progress Wireframes | ✅ / ❌ |
| 🔬 Research & UX | Validated wireframe | ✅ / ❌ |
| 📚 Ressources | Moodboard | ✅ / ❌ |
| 📚 Ressources | Benchmark | ✅ / ❌ |
| 📚 Ressources | Client documentation | ✅ / ❌ |
| 📦 Deliverables | App Icon | ✅ / ❌ |
| 📦 Deliverables | Store screen | ✅ / ❌ |
| — | Archives | ✅ / ❌ |
| — | Sandbox | ✅ / ❌ |
| — | Cover | ✅ / ❌ |

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

## Étape 5 — Connexion bibliothèque DS → fichier Projet

### Étape 1 — Vérifier les fichiers via MCP

```
get_metadata [URL fichier Design System]
→ Vérifier : variables publiées, styles publiés, composants publiés

get_metadata [URL fichier Projet]
→ Vérifier : bibliothèques connectées actuellement
```

### Étape 2 — Publier la bibliothèque DS

Via `use_figma` sur le fichier Design System :

```
Action : Publier la bibliothèque
Éléments à publier :
  ✅ Variables (tokens couleurs, spacing, radius, motion)
  ✅ Text Styles (ios/* et android/*)
  ✅ Effect Styles (shadows)
  ✅ Composants (tous les composants de 🧩 components)
```

> ⚠️ La publication nécessite un Figma Team ou Organization plan.
> En plan Starter : partager le fichier DS en lien et copier les composants manuellement.

### Étape 3 — Connecter la bibliothèque dans le fichier Projet

Via `use_figma` sur le fichier Projet :

```
Action : Activer la bibliothèque
Source : [NomProjet] — Design System
Résultat attendu :
  → Variables DS disponibles dans le panneau Variables
  → Text Styles DS disponibles dans les options de texte
  → Composants DS disponibles dans Assets
```

### Étape 4 — Vérifier la connexion

```
get_variable_defs [URL fichier Projet]
→ Les variables du DS doivent apparaître comme "external"
→ Vérifier : color/interactive/primary, spacing/md, radius/lg

search_design_system [URL fichier Projet] "button"
→ Les composants du DS doivent être trouvables
```

### Étape 5 — Documenter les URLs dans le rapport

```markdown
## Rapport connexion bibliothèque — [EPIC-###] — [YYYY-MM-DD]

### Fichiers connectés
- Design System : [URL Figma DS]
- Fichier Projet : [URL Figma Projet]

### Éléments publiés et connectés
| Élément | Publié | Accessible dans le Projet |
|---------|--------|--------------------------|
| Variables couleurs | ✅ | ✅ |
| Variables spacing | ✅ | ✅ |
| Text Styles iOS | ✅ | ✅ |
| Text Styles Android | ✅ | ✅ |
| Effect Styles | ✅ | ✅ |
| Composants | ✅ | ✅ |

### Plan Figma
- Plan actuel : [Starter / Professional / Organization]
- Publication bibliothèque : [Active / Manuel — copie des composants]
```

## Phase de validation — niveau standard

1. Les variables du DS sont-elles accessibles dans le fichier Projet — apparaissent-elles comme "external" dans le panneau Variables ? (oui / non)
2. Les Text Styles iOS et Android sont-ils disponibles dans le fichier Projet ? (oui / non)
3. Les composants publiés sont-ils trouvables dans Assets du fichier Projet ? (oui / non)

> Si tout validé, le skill se termine. Si non, reprendre depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ setup-figma-library "$ARGUMENTS" terminé
🔗 DS → Projet connectés
  Variables : ✅ | Text Styles : ✅ | Composants : ✅

Prochaines étapes :
→ /setup-figma-permissions $ARGUMENTS
→ /write-figma-userflow $ARGUMENTS

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```

---

## Étape 6 — Matrice des permissions

### Étape 1 — Identifier les intervenants depuis le cadrage

Depuis `specs/$ARGUMENTS/cadrage.md`, extraire :
- Équipe interne (Product Designer, BA, PO, Dev)
- Intervenants client (PO client, direction, valideurs)
- Testeurs externes si tests utilisateur prévus

### Étape 2 — Définir la matrice de permissions

```markdown
## Matrice de permissions — [NomProjet]

### Fichier Design System
| Intervenant | Rôle | Accès recommandé |
|-------------|------|-----------------|
| Product Designer | Équipe interne | ✏️ Can edit |
| BA / PO | Équipe interne | 💬 Can comment |
| Dev iOS / Android | Équipe interne | 👁️ Can view |
| Client | Externe | ❌ Pas d'accès direct |

### Fichier Projet (maquettes)
| Intervenant | Rôle | Accès recommandé |
|-------------|------|-----------------|
| Product Designer | Équipe interne | ✏️ Can edit |
| BA / PO | Équipe interne | 💬 Can comment |
| Dev iOS / Android | Équipe interne | 👁️ Can view |
| Client — valideurs | Externe | 💬 Can comment (phases de validation uniquement) |
| Client — direction | Externe | 👁️ Can view |

### Prototype React (fichier HTML)
| Usage | Accès | Mode |
|-------|-------|------|
| Tests utilisateur | Lien direct fichier HTML | Ouvert — aucune auth |
| Partage client | Google Drive — lien avec accès | Lecture |
| Partage public | ❌ Non recommandé | — |

### Lien de partage Figma prototype natif (si utilisé)
| Usage | Mode | Expiration |
|-------|------|-----------|
| Présentation client | Lien Anyone with link — Can view | Après validation |
| Tests utilisateur | Lien Anyone with link — Can view | Durée des tests |
```

### Étape 3 — Produire les instructions pour le Product Designer

```markdown
## Actions de partage à effectuer manuellement

⚠️ Ces actions doivent être réalisées par le Product Designer dans Figma.
Les agents ne peuvent pas modifier les permissions.

### Fichier Design System
1. Ouvrir [URL Fichier DS]
2. Cliquer "Share" en haut à droite
3. Ajouter les membres selon la matrice ci-dessus
4. Ne pas partager avec le client

### Fichier Projet
1. Ouvrir [URL Fichier Projet]
2. Cliquer "Share" en haut à droite
3. Ajouter l'équipe interne
4. Pour le client : partager uniquement pendant les phases de validation
   (Gate 7, Gate 8, Gate 9) puis retirer l'accès après validation

### À faire lors du partage prototype client
1. Générer un lien "Anyone with link — Can view" sur Figma
   OU utiliser le lien du fichier HTML prototype React
2. Inclure ce lien dans `/write-client-deliverable prototype $ARGUMENTS`
3. NE PAS donner accès Can edit au client

### Règle de sécurité
- Jamais de Can edit pour les intervenants externes
- Retirer l'accès client après chaque cycle de validation
- Ne jamais partager le fichier DS avec le client
```

## Phase de validation — niveau standard

1. La matrice de permissions couvre-t-elle tous les intervenants identifiés au cadrage ? (oui / non + intervenants manquants)
2. Le client n'a-t-il accès qu'au fichier Projet (jamais au fichier DS) ? (oui / confirmé)
3. Le Product Designer a-t-il reçu les instructions d'actions manuelles ? (oui / non)

> Ce skill produit des instructions — l'exécution est manuelle par le Product Designer.

## Résumé de fin d'exécution

```
✅ setup-figma-permissions "$ARGUMENTS" terminé
📋 Matrice de permissions : specs/$ARGUMENTS/figma-permissions.md

⚠️ Action manuelle requise — Product Designer
"Appliquer les permissions selon la matrice produite"

Prochaines étapes :
→ /write-figma-userflow $ARGUMENTS
→ /setup-figma-prototype $ARGUMENTS (connexions prototype natif)

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```

---

## Résumé de fin d'exécution

```
✅ setup-figma-project "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/figma-setup.md (rapport)

Prochaines étapes :
→ /setup-figma-tokens $ARGUMENTS (orchestré par setup-figma-init)
→ /setup-figma-grid $ARGUMENTS
→ Partager les URLs Figma avec les agents UX et UI

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```

---

## Composant Section FEAT

Créer un composant `section/feat` dans le fichier Projet via `use_figma` :

### Spécifications du composant

```
Nom          : section/feat
Type         : Composant Figma (pas un simple frame)
Taille min.  : 3000 × 1500 px
Auto Layout  : Vertical — direction top → bottom
Padding      : 80px tous côtés
Gap          : 64px entre les éléments enfants
Fill         : Couleur de fond neutre (surface/secondary depuis tokens DS)
```

### Propriétés du composant

```
Propriété : feat_title     — Text — "FEAT-### Nom de la feature"
Propriété : feat_id        — Text — "FEAT-###"
Propriété : has_ios        — Boolean — True
Propriété : has_android    — Boolean — True
```

### Structure interne (layers)

```
section/feat
├── feat_header (Auto Layout horizontal)
│   ├── feat_id         ← Text · ios/type/headline · token color/content/secondary
│   └── feat_title      ← Text · ios/type/title_1 · token color/content/primary
├── feat_divider        ← Rectangle 1px · color/border/default · fill width
└── feat_content        ← Frame vide · fill width · min-height 1000px
```

### Groupement de toutes les sections FEAT

Après création du composant, créer un groupe global dans `Work In Progress UI` :

```
Frame : all_feat_sections
  Auto Layout : Vertical
  Gap         : 120px entre chaque section
  Padding     : 80px
  Fill        : Transparent
  Contenu     : Instance section/feat × N (une par FEAT-###)
```

Chaque instance est nommée selon le FEAT :
```
section_FEAT_001_[nom_feature]
section_FEAT_002_[nom_feature]
```

### Via `use_figma`

```
Action 1 : Créer le composant section/feat (3000 × 1500, auto layout vertical)
Action 2 : Publier le composant dans la bibliothèque DS
Action 3 : Créer le frame all_feat_sections dans Work In Progress UI
Action 4 : Instancier section/feat × N (une par FEAT-### du scope)
Action 5 : Appliquer auto layout vertical sur all_feat_sections (gap 120px)
Action 6 : Nommer chaque instance section_FEAT_###_[nom]
```
