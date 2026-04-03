---
name: setup-figma-init
description: "Point d'entrée obligatoire du setup Figma — demande si la phase doit être automatisée ou non, collecte les URLs Figma existants si manuel, puis orchestre le setup. Use when user says 'setup figma', 'initialise figma', 'démarre le setup figma', 'configure figma', 'phase figma', or 'prépare figma'."
argument-hint: "[epic-slug]"
agent: design-system-manager
---

## Rôle
Poser deux questions dans l'ordre avant tout setup Figma — automatisé ou non, puis URLs existants — et orchestrer la suite selon la réponse.

## Prérequis
- [ ] `project-init` exécuté
- [ ] OS de base défini dans `specs/[epic-slug]/cadrage.md` — section "OS de conception de référence"
> Si le cadrage ne précise pas l'OS de base et que les deux plateformes sont ciblées :
> demander au Product Designer avant de créer les frames.
> La réponse est enregistrée dans `cadrage.md` et `project-config.md`.
 — `specs/[project-slug]/project-config.md` disponible
- [ ] Cadrage validé (`write-cadrage`)
- [ ] Stack technique connue depuis le cadrage
> Si `project-init` a déjà défini le mode du Groupe 3, l'utiliser directement sans redemander.
---
- [ ] MCP Figma connecté
- [ ] `US-###` disponibles dans `specs/[epic-slug]/stories/`
- [ ] `specs/[epic-slug]/figma-urls.md` créé ou à créer pendant ce skill

## Étape 1 — Question : automatisé ou non ?

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SETUP FIGMA — [epic-slug]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Le setup Figma doit-il être automatisé ?

  OUI — les agents créent et configurent les fichiers Figma
        via MCP (structure, tokens, grilles, bibliothèque).
        Aucune intervention manuelle nécessaire.

  NON — vous gérez Figma vous-même.
        Je listerai les fichiers à me partager pour analyse
        et utilisation dans la suite du process.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Étape 2 — Traitement selon la réponse

### Si OUI — automatisé

Poser la question complémentaire sur les fichiers existants :

```
Des fichiers Figma existent-ils déjà pour ce projet ?

  - Fichier Design System    : [URL Figma ou "Aucun"]
  - Fichier Projet           : [URL Figma ou "Aucun"]
  - Charte client / référence : [URL Figma ou "Aucun"]

Si tout est "Aucun" → je crée les fichiers depuis zéro.
```

Pour chaque URL fournie, lire via MCP :
```
get_metadata [URL] → pages existantes, composants, structure
get_variable_defs [URL DS] → tokens existants si DS fourni
```

Puis enchaîner les skills de setup dans l'ordre :
```
/setup-figma-project [epic-slug]
/setup-figma-tokens [epic-slug]
/setup-figma-grid [epic-slug]
/setup-figma-library [epic-slug]
/setup-figma-permissions [epic-slug]
```

---

### Si NON — manuel

Afficher la liste des fichiers à fournir :

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
FICHIERS FIGMA À PARTAGER
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Pour que je puisse analyser votre Figma et l'utiliser
dans la suite du process, partagez les URLs suivants.
Indiquez "Aucun" si un fichier n'existe pas encore.

Obligatoires pour continuer le process :
  1. Fichier Design System
     → URL : ___________________________
     → Contient : tokens, composants, styles

  2. Fichier Projet [EPIC-###]
     → URL : ___________________________
     → Contient : screens, user flows, handoff

Optionnels — utiles si existants :
  3. Charte graphique client
     → URL : ___________________________
     → Utilisée comme référence pour les tokens couleurs/typo

  4. Fichier UI existant (version précédente ou concurrent)
     → URL : ___________________________
     → Utilisé pour l'audit et la migration

  5. Bibliothèque d'icônes ou d'illustrations
     → URL : ___________________________

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Une fois les URLs partagés, j'effectuerai l'analyse
et vous indiquerai comment les utiliser dans le process.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

Dès réception des URLs, lire chaque fichier via MCP :
```
get_metadata [URL DS]        → structure, pages, composants publiés
get_variable_defs [URL DS]   → tokens existants
get_metadata [URL Projet]    → pages, sections, frames nommées
get_screenshot [frames clés] → état visuel actuel
```

Puis produire le rapport d'audit qualité complet :

```markdown
## Rapport audit Figma — [epic-slug] — [YYYY-MM-DD]

---

## Note globale : [N]/10

| Dimension | Note | Poids |
|-----------|------|-------|
| Nomenclature (snake_case) | [N]/10 | 20% |
| Tokens et variables | [N]/10 | 20% |
| Structure des pages | [N]/10 | 15% |
| Composants et variants | [N]/10 | 15% |
| Modes light/dark | [N]/10 | 10% |
| Text Styles | [N]/10 | 10% |
| Grilles et safe areas | [N]/10 | 10% |

**Note globale pondérée : [N]/10**
[Verdict : Excellent (9-10) / Bon (7-8) / Acceptable (5-6) / À corriger (< 5)]

---

## Éléments problématiques

### 🔴 Bloquants — à corriger avant de continuer
| # | Dimension | Problème | Exemple | Impact |
|---|-----------|---------|---------|--------|
| 1 | Nomenclature | Frames nommées automatiquement | `Frame 42`, `Group 7` | Handoff impossible |
| 2 | Tokens | Valeurs couleurs en dur dans les composants | `#FF0000` au lieu de `color/feedback/error` | Incohérence DS |
| 3 | Structure | Page `Work In Progress UI` absente | — | Specs UX impossibles |

### 🟡 Importants — à corriger rapidement
| # | Dimension | Problème | Exemple | Impact |
|---|-----------|---------|---------|--------|
| 1 | Modes | Mode dark absent sur les tokens sémantiques | — | Dark mode non supporté |
| 2 | Text Styles | Styles iOS et Android non créés | — | Typographie non partagée |

### 🟢 Mineurs — à corriger avant le handoff
| # | Dimension | Problème | Exemple | Impact |
|---|-----------|---------|---------|--------|
| 1 | Composants | Variants non nommés en snake_case | `State=Hover` au lieu de `state=hover` | Incohérence |

---

## Corrections suggérées

### 🔴 Bloquants

| # | Correction | Méthode | Automatisable |
|---|-----------|---------|--------------|
| 1 | Renommer toutes les frames selon `US_###_[nom]_[plateforme]_[état]` | `use_figma` — renommage batch | ✅ Oui |
| 2 | Remplacer les valeurs hex en dur par les tokens DS correspondants | `use_figma` — remplacement variables | ✅ Oui |
| 3 | Créer la page `Work In Progress UI` avec sections US | `use_figma` — création page | ✅ Oui |

### 🟡 Importants

| # | Correction | Méthode | Automatisable |
|---|-----------|---------|--------------|
| 1 | Créer le mode `dark` sur la collection sémantique | `use_figma` — ajout mode | ✅ Oui |
| 2 | Créer les Text Styles `ios/*` et `android/*` | `use_figma` — création styles | ✅ Oui |

### 🟢 Mineurs

| # | Correction | Méthode | Automatisable |
|---|-----------|---------|--------------|
| 1 | Renommer les propriétés de variants en snake_case | `use_figma` — renommage | ✅ Oui |

---

## Résumé des corrections automatisables

- **[N] corrections automatisables** via MCP Figma (`use_figma`)
- **[N] corrections manuelles** — nécessitent le Product Designer
  (ex : direction artistique, choix de tokens sémantiques ambigus)

Souhaitez-vous que je lance l'automatisation des corrections ?
  OUI → je corrige dans l'ordre : bloquants → importants → mineurs
  NON → je liste les instructions pour chaque correction manuelle
```

---

### Si OUI — lancer l'automatisation des corrections

Appliquer les corrections dans l'ordre de priorité via `use_figma` :

```
Phase 1 — Corrections bloquantes
  → Renommage frames (snake_case batch)
  → Remplacement valeurs en dur par tokens
  → Création pages/sections manquantes

Phase 2 — Corrections importantes
  → Création mode dark
  → Création Text Styles iOS/Android
  → Publication bibliothèque si absente

Phase 3 — Corrections mineures
  → Renommage variants
  → Nettoyage layers auto-nommés restants
```

Après chaque phase, afficher :
```
✅ Phase [N] terminée — [N] corrections appliquées
   Éléments corrigés : [liste courte]
   Éléments restants : [N]
   Continuer vers la Phase [N+1] ?
```

Produire le rapport final après toutes les corrections :

```markdown
## Rapport corrections — [epic-slug] — [YYYY-MM-DD]

| Phase | Corrections appliquées | Méthode | Statut |
|-------|----------------------|---------|--------|
| Bloquants | [N] | Automatique | ✅ |
| Importants | [N] | Automatique | ✅ |
| Mineurs | [N] | Automatique | ✅ |

Note avant corrections : [N]/10
Note après corrections : [N]/10

Fichiers prêts pour la suite du process.
```

### Si NON — instructions manuelles

Produire pour chaque correction non automatisable une fiche d'instruction :

```markdown
## Correction [#N] — [Titre]

**Problème :** [Description]
**Où :** [Page > Section > Layer concerné]
**Comment :**
  1. [Étape 1]
  2. [Étape 2]
**Résultat attendu :** [Description]
**Prévenez-moi quand c'est fait.**
```

---

## Étape 3 — Sauvegarder les URLs

Quel que soit le mode, sauvegarder dans `specs/[epic-slug]/figma-urls.md` :

```markdown
# URLs Figma — [NomProjet]

| Rôle | URL | Mode setup | Statut |
|------|-----|-----------|--------|
| Design System | [URL] | [Auto/Manuel] | [Créé/Existant] |
| Fichier Projet | [URL] | [Auto/Manuel] | [Créé/Existant] |
```


---

## DoD — Definition of Done — Phase Figma (Gate 3) — 45 critères

La Gate 3 ne peut être validée que si **tous** les critères ci-dessous sont cochés.
Cette DoD est présentée au Product Designer à la fin du setup, quel que soit le mode (Auto / Non).

```markdown
## ✅ Gate 3 — DoD Setup Figma — [NomProjet] — [YYYY-MM-DD]

### 1. Fichiers Figma

- [ ] Fichier Design System créé et accessible : `[NomProjet] — Design System`
- [ ] Fichier Projet créé et accessible : `[NomProjet] — [EPIC-###]`
- [ ] Les deux URLs sont renseignées dans `specs/[epic-slug]/figma-urls.md`
- [ ] Nomenclature des fichiers conforme à la convention du process

### 2. Structure des pages — Fichier Projet

- [ ] ✏️ Design > Work In Progress UI — page créée
- [ ] ✏️ Design > Validated UI — page créée
- [ ] ✏️ Design > Ready for dev — page créée
- [ ] ✏️ Design > Accessibility — page créée
- [ ] 🔬 Research & UX > Userflow — page créée
- [ ] 🔬 Research & UX > Work In Progress Wireframes — page créée
- [ ] 🔬 Research & UX > Validated wireframe — page créée
- [ ] 📚 Ressources > Moodboard — page créée
- [ ] 📚 Ressources > Benchmark — page créée
- [ ] 📚 Ressources > Client documentation — page créée
- [ ] 📦 Deliverables > App Icon — page créée
- [ ] 📦 Deliverables > Store screen — page créée
- [ ] 🗄️ Archives — page créée
- [ ] 🧪 Sandbox — page créée
- [ ] 🖼️ Cover — page créée

### 3. Structure des pages — Fichier Design System (standard Neow DS)

**Pages système**
- [ ] `Get started` — page créée
- [ ] `Change log` — page créée

**Foundations (8 pages)**
- [ ] `🧱 Foundations` · `Colors` · `Typography` · `Icons`
- [ ] `Grid & spacing` · `Logo` · `Shadows & Blur` · `Images` · `Animations`

**Components (20 pages)**
- [ ] `⚛️ Components` · `Actions` · `Navigation` · `Content` · `TextField`
- [ ] `Forms` · `Controls` · `Lists` · `Tabs` · `Avatar`
- [ ] `Feedbacks` · `Accordion` · `Indicators` · `Cards` · `Chart`
- [ ] `Slider` · `Pattern` · `Modals & Pop-up` · `Loader` · `Mock up` · `Native Elements`

**Pages spéciales**
- [ ] `🗃️ Unused Components` · `🛠️ Utilities` · `🎇 Cover`

### 4. Tokens et variables

- [ ] Variables couleurs créées — collection `primitives` (valeurs brutes)
- [ ] Variables couleurs créées — collection `semantic` (rôles)
- [ ] Mode `light` configuré sur la collection sémantique
- [ ] Mode `dark` configuré sur la collection sémantique avec valeurs distinctes
- [ ] Contraste 4.5:1 vérifié en light ET en dark
- [ ] Variables spacing créées — multiples de 4pt/dp
- [ ] Variables radius créées
- [ ] Variables motion créées (durées + easing)
- [ ] Text Styles iOS créés — nommés `ios/[type]/[style]`
- [ ] Text Styles Android créés — nommés `android/[type]/[style]`
- [ ] Effect Styles (ombres) créés
- [ ] Tokens Figma conformes à `design-system/design-tokens.md` — 0 divergence

### 5. Grilles et frames de base

- [ ] Grille iOS créée — 4 col · 16pt gutter · 16pt margin
- [ ] Grille Android créée — 4 col · 16dp gutter · 16dp margin
- [ ] Safe areas iOS configurées (status bar + home indicator)
- [ ] Safe areas Android configurées (status bar + gesture nav)
- [ ] Frames de base iOS disponibles (375pt · 390pt · 430pt)
- [ ] Frames de base Android disponibles (360dp · 412dp)

### 6. Composant section/feat

- [ ] Composant `section/feat` créé dans le fichier Projet
- [ ] Taille minimum 3000 × 1500px respectée
- [ ] Auto Layout vertical configuré — padding 80px · gap 64px
- [ ] Propriétés `feat_title` · `feat_id` · `has_ios` · `has_android` configurées
- [ ] Frame globale `all_feat_sections` créée avec Auto Layout vertical (gap 120px)
- [ ] Une instance `section/feat` créée par FEAT-### du scope

### 7. Bibliothèque et connexions

- [ ] Bibliothèque DS publiée (variables + Text Styles + Effect Styles + composants)
- [ ] Bibliothèque DS connectée au fichier Projet
- [ ] Variables DS apparaissent comme "external" dans le panneau Variables du fichier Projet
- [ ] Text Styles disponibles dans le fichier Projet
- [ ] Composants publiés trouvables dans Assets du fichier Projet

### 8. Nomenclature

- [ ] 0 page nommée automatiquement — toutes les pages ont un nom conforme
- [ ] 0 layer nommé automatiquement (Frame 1, Group 1, Rectangle...) dans les frames de base
- [ ] snake_case strict sur tous les noms de layers et sections

### 9. Matrice des permissions

- [ ] Matrice des accès produite dans `specs/[epic-slug]/figma-permissions.md`
- [ ] Accès configurés selon la matrice (à appliquer manuellement par le Product Designer)

---

**Résultat :** [N]/[N] critères validés

**Éléments bloquants restants :**
- [ ] [Élément non conforme + action corrective]

**Décision Product Designer :** ✅ Gate 3 validée / ❌ Corrections requises avant de continuer
```

## Phase de validation

1. La réponse Auto/Non a-t-elle été donnée par le Product Designer — pas déduite ? (oui / non)
2. Les URLs ont-ils été lus via MCP — inventaire produit ? (oui / non / N/A si nouveau projet)
3. `specs/[epic-slug]/figma-urls.md` créé avec les URLs de référence ? (oui / non)

## Résumé de fin d'exécution

```
✅ setup-figma-init "$ARGUMENTS" terminé
Mode : [Automatisé / Manuel]
📁 specs/$ARGUMENTS/figma-urls.md

Prochaines étapes selon le mode :
[Auto]  → /write-figma-userflow $ARGUMENTS
[Manuel] → Partager les URLs → analyse → corrections si besoin → /write-figma-userflow

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```
