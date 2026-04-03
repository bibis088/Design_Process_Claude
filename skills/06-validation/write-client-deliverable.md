---
name: write-client-deliverable
description: "Génère un livrable client formaté pour Google Doc ou Google Sheets depuis les documents internes — résumé de cadrage, user stories, rapport QA, release notes. Inclut une section \"Ce qu'on vous demande de valider\". Exécuté par le product-owner."
argument-hint: "[type] [epic-slug]"
argument-hint-detail: "types disponibles : cadrage | user-stories | maquettes | prototype | qa | release-notes | functional-doc"
agent: product-owner
---

## Rôle
Transformer un document interne en livrable client — langage simplifié, contexte expliqué, section de validation explicite. Format Google Doc ou Google Sheets selon le type.

## Types de livrables client

| Type | Source interne | Format | Gate associée |
|------|---------------|--------|--------------|
| `cadrage` | `cadrage.md` | Google Doc | Gate 0 |
| `user-stories` | `US-###.md` × N | Google Sheets | Gate 2 |
| `maquettes` | URLs Figma + `SCREENS-MAP.md` | Google Doc | Gate 7 ou 8 |
| `prototype` | Fichier HTML + `FLUX-###` | Google Doc | Après `write-prototype-react` |
| `qa` | `QA-###.md` × N | Google Doc | Gate 11 |
| `release-notes` | `release-notes-v[X].md` | Google Doc | Avant publication |
| `functional-doc` | `functional-doc-v[X].md` | Google Doc | Gate 9 |

## Prérequis
- [ ] Document source disponible et en statut `Approved` ou `In Review`
- [ ] Type de livrable précisé dans l'argument
- [ ] `cadrage.md` disponible — cadre contractuel des itérations défini
- [ ] `US-###` disponibles (pour type user-stories)
- [ ] Gate correspondante au type de livrable atteinte
- [ ] `FLUX-###` disponibles (type functional-doc)
- [ ] `SCREENS-MAP.md` disponible (type functional-doc)
- [ ] Prototype React disponible si type inclut la démo
- [ ] `write-qa-report` disponible si livrable de type QA

## Processus de génération

### Étape 1 — Lire le document source

Depuis le chemin correspondant au type, extraire le contenu pertinent pour le client.

**Règles de transformation :**
- Supprimer les références internes (`EPIC-###`, `RB-###`, chemins de fichiers)
- Remplacer le jargon technique par du langage métier
- Conserver les critères d'acceptance en langage utilisateur
- Ajouter du contexte là où le client n'a pas le background technique

---

### Templates par type

#### Type : `cadrage`

```markdown
# Résumé de cadrage — [NomProjet] — [YYYY-MM-DD]

## À propos de ce document
Ce document résume notre compréhension commune du projet.
**Votre validation est requise avant que nous démarrions les spécifications.**

---

## Ce qu'on a compris

### Le problème à résoudre
[Reformulation simple du problème métier]

### Vos utilisateurs
[Description des personas en langage non technique]

### Ce qu'on va construire (V1)
[Liste des fonctionnalités en langage utilisateur]

### Ce qui est prévu pour plus tard
[Fonctionnalités hors scope V1]

### La technologie choisie
[Description accessible — ex: "application native iOS et Android"]

---

## ✅ Ce qu'on vous demande de valider

1. Est-ce que ce résumé reflète bien ce que vous nous avez demandé ?
2. Y a-t-il des éléments manquants ou incorrects dans le périmètre V1 ?
3. Les utilisateurs décrits correspondent-ils bien à votre réalité ?

**Réponse attendue :** Oui, validé / Non, voici les corrections : [...]
```

---

#### Type : `user-stories` (Google Sheets)

```
Feuille 1 : Vue d'ensemble
Colonnes : Feature | Fonctionnalité | Bénéfice utilisateur | Priorité | Statut

Feuille 2 : Détail par story
Colonnes : Fonctionnalité | En tant que | Je veux | Pour | Critères de validation | Écran Figma

Note : les critères d'acceptance sont reformulés en "critères de validation"
visibles par le client — sans jargon technique.
```

Format du contenu :
```markdown
# Fonctionnalités — [NomProjet] — [YYYY-MM-DD]

## À propos de ce document
Ce document liste les fonctionnalités que nous allons développer.
**Votre validation est requise sur les priorités et les critères.**

---

## Récapitulatif

| Fonctionnalité | Description | Priorité | Pour qui |
|---------------|-------------|---------|---------|
| [Titre US] | [Description simple] | [Essentiel / Important / Optionnel] | [Persona] |

---

## ✅ Ce qu'on vous demande de valider

1. Les fonctionnalités listées correspondent-elles à vos attentes ?
2. Les priorités sont-elles correctes ?
3. Y a-t-il des fonctionnalités manquantes ?
```

---

#### Type : `maquettes`

```markdown
# Maquettes — [NomProjet] — [YYYY-MM-DD]

## À propos de ce document
Les maquettes représentent le design de l'application.
Ce n'est pas encore le produit final — c'est la version design à valider.

---

## Accès aux maquettes

🔗 **Lien Figma :** [URL page Screens]
*(Pas besoin de compte Figma — le lien permet la consultation en lecture)*

### Comment naviguer dans Figma
1. Cliquez sur le lien ci-dessus
2. Utilisez la molette pour zoomer
3. Cliquez sur "▶ Présentation" en haut à droite pour voir le prototype Figma

---

## Écrans produits

| Section | Écrans | Lien direct |
|---------|--------|-------------|
| [NomUS] | [S-XX Default, Loading, Empty, Error] | [URL Figma] |

---

## ✅ Ce qu'on vous demande de valider

1. Les écrans correspondent-ils au parcours que vous attendiez ?
2. Y a-t-il des éléments visuels ou de contenu à corriger ?
3. Les cas d'erreur et états vides sont-ils bien gérés ?
```

---

#### Type : `prototype`

> ⚠️ Ce livrable ne doit être partagé qu'après les tests utilisateur et corrections critiques.
> Mentionner le numéro de cycle dans le document (Cycle 1 / Cycle 2).
> Après le Cycle 2, toute demande de modification supplémentaire déclenche
> une décision de scope par le PO et le Product Designer.

```markdown
# Prototype interactif — [NomProjet] — [YYYY-MM-DD]

## À propos de ce document
Le prototype vous permet de naviguer dans l'application comme si elle était réelle.
C'est une simulation — les données sont fictives et toutes les fonctionnalités
ne sont pas connectées.

---

## Accès aux prototypes

## Cycle de validation contractuel
> Référence : `specs/[epic-slug]/cadrage.md` — section "Cadre contractuel des itérations"

- Numéro de cycle : [Cycle 1 / Cycle 2 / ...]
- Cycles contractuels inclus : [N — défini au cadrage]
- Cycles restants : [N]
- Date du partage : [YYYY-MM-DD]
- Délai de réponse client attendu : [N jours ouvrés — défini au cadrage]

⚠️ Si ce partage dépasse le nombre de cycles contractuels :
→ Informer le client avant d'envoyer
→ PO + Product Designer décident : appliquer / reporter V2 / avenant

| Parcours | Description | Lien | Niveau |
|---------|-------------|------|--------|
| [FLUX-###] | [Description du flow] | [URL fichier HTML] | [Cliquable / Interactif] |

### Comment utiliser le prototype
- Ouvrir le lien dans Chrome ou Safari sur mobile
- Naviguer comme dans une vraie application
- La barre en bas rappelle que c'est un prototype
- Revenir en arrière avec le bouton ← ou le geste natif

---

## ✅ Ce qu'on vous demande de valider

1. Le parcours est-il fluide et logique de bout en bout ?
2. Y a-t-il des étapes manquantes ou surprenantes ?
3. Les textes et contenus sont-ils corrects ?
4. Sur une échelle de 1 à 5, comment évaluez-vous la facilité d'utilisation ?
```

---

#### Type : `qa`

```markdown
# Rapport de validation — [NomProjet] v[X.X] — [YYYY-MM-DD]

## À propos de ce document
Ce document résume les tests effectués avant la mise en production.

---

## Résultat global

✅ **Validé** / ❌ **Non validé** / ⚠️ **Validé avec réserves**

---

## Ce qui a été testé

| Fonctionnalité | iOS | Android | Résultat |
|---------------|-----|---------|---------|
| [Titre US] | ✅ | ✅ | Validé |

## Points d'attention (réserves)
[Si applicable — description non technique des réserves]

---

## ✅ Ce qu'on vous demande de valider

"Validez-vous cette version pour mise en production ?"

Réponse attendue : Oui / Non + commentaires
```

---

#### Type : `release-notes`

```markdown
# Nouveautés — [NomProjet] v[X.X] — [YYYY-MM-DD]

## Ce qui change dans cette version

### Nouvelles fonctionnalités
- 🎉 [Fonctionnalité] — [Bénéfice en une phrase]

### Améliorations
- ✨ [Amélioration] — [Impact]

### Corrections
- 🐛 [Problème corrigé]

---

## ✅ Ce qu'on vous demande de valider

"Validez-vous le texte de release notes pour publication sur les stores ?"
```

---

#### Type : `functional-doc`

Utiliser directement `write-functional-doc` — le document produit est déjà adapté au client.
Ajouter uniquement la section de validation :

```markdown
---

## ✅ Ce qu'on vous demande de valider

1. La documentation fonctionnelle reflète-t-elle fidèlement le projet convenu ?
2. La matrice de traçabilité couvre-t-elle toutes les fonctionnalités attendues ?
3. Y a-t-il des corrections à apporter avant de démarrer le développement ?

Réponse attendue : Validé / Non validé + corrections
```

---

## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. Le document est-il exempt de jargon technique — compréhensible par un client non technique ? (oui / non + passages à reformuler)
2. La section "Ce qu'on vous demande de valider" pose-t-elle des questions claires et actionnables ? (oui / non)
3. Les liens Figma et prototype sont-ils inclus et accessibles sans compte ? (oui / non / N/A)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-client-deliverable "[type] $ARGUMENTS" terminé
📁 specs/$ARGUMENTS/client/[type]-client-[YYYY-MM-DD].md

Prochaines étapes :
→ Copier le contenu dans Google Doc / Google Sheets
→ Partager au client avec droits commentaire
→ Attendre validation client avant de continuer

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```
