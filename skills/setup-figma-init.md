---
name: setup-figma-init
description: "Point d'entrée obligatoire du setup Figma — le Product Designer choisit le mode d'exécution (auto / mixte / humain + review) et fournit les URLs Figma existants avant de démarrer. Exécuté par le design-system-manager. Use when user says 'setup figma', 'initialise figma', 'démarre le setup figma', 'configure figma', or 'prépare figma'."
argument-hint: "[epic-slug]"
agent: design-system-manager
---

## Rôle
Poser les deux questions nécessaires avant tout setup Figma — mode d'exécution et URLs existants — puis orchestrer les skills de setup dans le bon ordre selon le mode choisi.

## Prérequis
- [ ] `project-init` exécuté — mode Figma choisi dans `specs/[project-slug]/project-config.md`
- [ ] Cadrage validé (`write-cadrage`)
- [ ] Stack technique connue (iOS / Android / les deux)

> Si `project-init` a déjà défini le mode pour le Groupe 3, utiliser ce choix
> plutôt que de redemander. Le mode A/B/C de `setup-figma-init` correspond
> à Auto/Mixte/Manuel du `project-init`.

---

## Étape 1 — Poser les deux questions en un seul message

```
Avant de démarrer le setup Figma pour [epic-slug], j'ai besoin de deux informations.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

QUESTION 1 — Mode d'exécution

Quel mode de setup Figma souhaitez-vous ?

  A) Automatique
     Toutes les étapes sont exécutées par les agents via MCP Figma.
     Résultat : setup complet sans intervention.
     Idéal pour : nouveau projet, fichiers vierges.

  B) Mixte — humain + automatisation
     Vous réalisez certaines étapes manuellement dans Figma
     (ex : direction artistique, assets spécifiques du client),
     les agents complètent le reste.
     Idéal pour : contraintes de charte, assets existants à intégrer.

  C) Humain + review automatisée
     Vous réalisez toutes les étapes manuellement dans Figma.
     À la fin, les agents auditent la qualité et produisent un rapport.
     Idéal pour : projet existant, migration, reprise d'un fichier tiers.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

QUESTION 2 — URLs Figma existants

Partagez les URLs Figma du projet si des fichiers existent déjà.
Pour chaque URL, précisez son rôle.

  Exemple :
  - Fichier Design System : https://figma.com/file/xxx
  - Fichier Projet EPIC-001 : https://figma.com/file/yyy
  - Fichier charte client : https://figma.com/file/zzz
  - Fichier reference UI : https://figma.com/file/www

  Si aucun fichier n'existe encore : "Aucun — nouveau projet"

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Étape 2 — Traiter les réponses

### Traitement des URLs

Pour chaque URL fournie, lire via MCP avant de continuer :

```
get_metadata [URL 1] → type de fichier, pages existantes, composants
get_metadata [URL 2] → idem
get_variable_defs [URL DS si applicable] → tokens existants
```

Produire un inventaire :

```markdown
## Inventaire Figma existant — [epic-slug]

| Fichier | URL | Pages | Composants | Tokens | Statut |
|---------|-----|-------|-----------|--------|--------|
| Design System | [URL] | [N] | [N] | [N] | [À utiliser / À refactorer] |
| Fichier Projet | [URL] | [N] | [N] | — | [À utiliser / À créer] |
| Charte client | [URL] | [N] | [N] | [N] | [Référence] |
```

---

## Étape 3 — Orchestrer selon le mode choisi

### Mode A — Automatique

Enchaîner automatiquement tous les skills dans l'ordre :

```
1. /setup-figma-project [epic-slug]
2. /setup-figma-tokens [epic-slug]
3. /setup-figma-grid [epic-slug]
4. /setup-figma-library [epic-slug]
5. /setup-figma-permissions [epic-slug]  ← produit la matrice, exécution manuelle PD
```

Informer le Product Designer à chaque étape terminée.
Arrêter et alerter si une étape échoue.

---

### Mode B — Mixte

Présenter le plan d'exécution et demander validation avant chaque étape manuelle :

```markdown
## Plan de setup — Mode Mixte

| Étape | Mode | Skill | Prérequis |
|-------|------|-------|----------|
| Structure pages | 🤖 Auto | setup-figma-project | — |
| Direction stylistique | ✋ Humain | — | Réponse Product Designer |
| Tokens couleurs | 🤖 Auto | setup-figma-tokens (depuis charte client si fournie) | Direction stylistique |
| Modes light/dark | 🤖 Auto | setup-figma-tokens (étapes 6-7) | Tokens couleurs |
| Text Styles | 🤖 Auto | setup-figma-tokens (étape 7) | — |
| Grilles | 🤖 Auto | setup-figma-grid | — |
| Assets client (icônes, illustrations) | ✋ Humain | — | Fichiers fournis par client |
| Connexion bibliothèque | 🤖 Auto | setup-figma-library | Étapes précédentes |
| Permissions | 🤖 Auto (matrice) + ✋ Humain (exécution) | setup-figma-permissions | — |

Pour chaque étape ✋ Humain, me prévenir quand vous avez terminé
pour que je passe à l'étape suivante.
```

Attendre la confirmation du Product Designer entre chaque étape manuelle.

---

### Mode C — Humain + review automatisée

Informer le Product Designer des étapes à réaliser et attendre la confirmation de fin :

```markdown
## Checklist setup manuel — Mode C

Réalisez ces étapes dans Figma, puis revenez me confirmer quand c'est terminé.

### Fichier Design System
- [ ] Créer le fichier `[NomProjet] — Design System`
- [ ] Créer les pages : 🎨 foundations / 🧩 components / 📐 patterns / 📖 documentation
- [ ] Configurer les variables (tokens couleurs, spacing, radius, motion)
- [ ] Configurer les modes light et dark sur les tokens sémantiques
- [ ] Créer les Text Styles iOS et Android
- [ ] Créer les Effect Styles (ombres)
- [ ] Publier la bibliothèque

### Fichier Projet
- [ ] Créer le fichier `[NomProjet] — [EPIC-###]`
- [ ] Créer les pages : 🗺️ user_flows / 📱 screens / 🔧 components / 🚀 handoff / 📦 store_assets
- [ ] Créer les sections US dans la page screens
- [ ] Connecter la bibliothèque Design System
- [ ] Configurer les grilles iOS/Android

### Nomenclature obligatoire (snake_case)
- Frames : `US_###_[nom]_[plateforme]_[état]`
- Layers : `snake_case` — jamais de noms auto Figma
- Composants : `categorie/nom/variante/etat`
- Pages : `🎨 foundations` (minuscules)

Confirmez quand tout est terminé avec les URLs des fichiers créés.
```

Dès la confirmation reçue, lancer l'audit :

```
/audit-figma-existing [figma-url-projet]
/audit-figma-existing [figma-url-ds]
```

Produire un rapport de qualité avec corrections recommandées.

---

## Étape 4 — Rapport d'initialisation

Quel que soit le mode, produire à la fin :

```markdown
# Rapport Setup Figma — [epic-slug] — [YYYY-MM-DD]

## Mode d'exécution
[A — Automatique / B — Mixte / C — Humain + review]

## Fichiers Figma du projet

| Rôle | URL | Statut |
|------|-----|--------|
| Design System | [URL] | ✅ Prêt |
| Fichier Projet EPIC-### | [URL] | ✅ Prêt |

## URLs à conserver pour la suite du process
Ces URLs sont utilisées par tous les skills Figma suivants.
→ Les inclure dans `specs/[epic-slug]/figma-urls.md`

## Résumé des étapes

| Étape | Mode | Statut |
|-------|------|--------|
| Structure pages | [Auto/Humain] | ✅ |
| Tokens + modes light/dark | [Auto/Humain] | ✅ |
| Text Styles | [Auto/Humain] | ✅ |
| Grilles | [Auto/Humain] | ✅ |
| Connexion bibliothèque | [Auto/Humain] | ✅ |
| Permissions | Matrice produite | ✅ (exécution manuelle PD) |

## Points d'attention
[Éléments signalés pendant le setup — déviations, étapes manuelles incomplètes, etc.]
```

Sauvegarder les URLs dans `specs/[epic-slug]/figma-urls.md` pour référence dans tous les skills suivants.

## Phase de validation — niveau standard

1. Le mode d'exécution a-t-il été choisi par le Product Designer — pas par l'agent ? (oui / non)
2. Toutes les URLs Figma existantes ont-elles été inventoriées et lues via MCP ? (oui / non + URLs manquantes)
3. Les fichiers DS et Projet sont-ils opérationnels selon le mode choisi ? (oui / non)
4. Le fichier `specs/[epic-slug]/figma-urls.md` a-t-il été créé avec les URLs de référence ? (oui / non)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.

## Résumé de fin d'exécution

```
✅ setup-figma-init "$ARGUMENTS" terminé
Mode : [A / B / C]
📁 specs/$ARGUMENTS/figma-urls.md

⚠️ Gate 3 — Validation Product Designer
"La structure Figma est-elle prête pour démarrer la phase UX ?"

Prochaines étapes :
→ /write-figma-userflow $ARGUMENTS (flows visuels)
→ /write-screen-spec $ARGUMENTS (specs UX)
```
