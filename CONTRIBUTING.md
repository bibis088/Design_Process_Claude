# Guide de contribution — Update_Antoine

Ce document explique comment ajouter, modifier ou supprimer un agent, une rule ou un skill en respectant les conventions du projet.

---

## Ajouter un nouvel agent

### 1. Créer le fichier
```
agents/[nom-agent].md
```

### 2. Respecter le frontmatter obligatoire
```yaml
---
name: [nom-agent]
description: [Description concise — rôle, périmètre, quand l'utiliser]
tools: [Read, Write, Edit, Glob, Grep, WebFetch — selon besoin]
model: claude-sonnet-4-6
---
```

### 3. Structure obligatoire du fichier

```markdown
# Intro — Qui es-tu ?
[1-2 phrases définissant le rôle sans ambiguïté]

## Relation avec les agents amont
[Tableau des inputs reçus et ce que l'agent en fait]

## Relation avec les agents aval
[Ce que l'agent produit et qui le consomme]

## Ta mission
[Liste numérotée des responsabilités]

## Documents que tu produis
[Templates des livrables avec chemins de fichiers]

## Conventions de nommage
[Référence aux conventions inter-agents — voir business-analyst.md section C]

## Quand tu génères des livrables
[Processus pas à pas]

## Definition of Done
[Socle non négociable + exigences additionnelles]
```

### 4. Mettre à jour
- `rules/communication.md` — ajouter l'agent dans la chaîne
- `README.md` — ajouter l'agent dans l'arborescence et la chaîne
- Les agents amont et aval — ajouter les références croisées

---

## Ajouter une nouvelle rule

### 1. Créer le fichier
```
rules/[nom-rule].md
```

### 2. Structure obligatoire

```markdown
# Conventions pour [Domaine]

## Rôle de cette rule
[Pourquoi elle existe, qui l'applique, quand elle s'applique]

## Agents concernés
[Liste des agents qui doivent lire et appliquer cette rule]

## Conventions
[Le contenu — formats, règles, templates]

## Règles de qualité
[Ce qui est valide vs invalide, avec exemples]
```

### 3. Mettre à jour
- `README.md` — ajouter la rule dans l'arborescence
- Les agents concernés — ajouter une référence à la rule dans leurs sections pertinentes

---

## Ajouter un nouveau skill

### 1. Créer le fichier
```
skills/[nom-skill].md
```

### 2. Respecter le frontmatter obligatoire
```yaml
---
name: [nom-skill]
description: [Description concise — ce que le skill produit, qui l'exécute]
argument-hint: [format de l'argument — ex: [epic-slug]/[story-slug]]
disable-model-invocation: false
context: fork
agent: [nom-agent-pilote]
---
```

### 3. Structure obligatoire

```markdown
## Rôle
[Ce que le skill produit en une phrase]

## Agents consommateurs
[Pilote + contributeurs]

## Prérequis
[Liste de prérequis cochables]

## Gestion des erreurs
[Comportement si prérequis manquants + fallback Glob]

## Processus de génération
### Étape 1 — [Nom]
### Étape 2 — [Nom]
[...]

## Phase de validation — niveau [léger / standard / approfondi]
[Questions de validation avant de passer au skill suivant]

> Réponds point par point. Si tout est validé, le skill se termine.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution
✅ [nom-skill] "$ARGUMENTS" terminé
📁 [chemin du fichier produit]
Prochaines étapes :
→ /[skill-suivant] $ARGUMENTS
```

### 4. Choisir le niveau de validation

| Niveau | Quand l'utiliser | Nombre de questions |
|--------|-----------------|-------------------|
| Léger | Livrable simple, peu de risque | 1 |
| Standard | Livrable fonctionnel ou design | 2-3 |
| Approfondi | Livrable critique ou transversal | 4-5 + vérification DoD |

### 5. Mettre à jour
- `README.md` — ajouter le skill dans le tableau des skills
- `review-dod.md` — si le skill produit un nouveau type de livrable, l'ajouter dans le tableau

---

## Modifier un fichier existant

### Convention d'annotation
Toute modification est annotée dans le fichier avec :

```markdown
<!-- ✅ MODIFIÉ [XX] : description courte du changement -->
```

ou pour une section ajoutée :

```markdown
## ✅ NOUVEAU [XX] — Titre de la section
```

La lettre de référence est attribuée dans l'ordre alphabétique depuis la dernière lettre utilisée (voir tableau de référence dans `README.md`).

### Processus
1. Faire le changement dans le fichier concerné
2. Ajouter le tag `✅ MODIFIÉ` ou `✅ NOUVEAU` avec la lettre de référence
3. Mettre à jour le tableau de référence dans `README.md`
4. Régénérer le zip si nécessaire

---

## Supprimer un fichier

Ne jamais supprimer un agent, une rule ou un skill sans :
1. Vérifier qu'aucun autre fichier ne le référence
2. Mettre à jour toutes les références croisées
3. Archiver le fichier dans `archives/` plutôt que de le supprimer définitivement
4. Mettre à jour `README.md`

---

## Checklist avant de pousser sur Git

- [ ] Le fichier respecte le frontmatter obligatoire (si agent ou skill)
- [ ] Les références croisées sont mises à jour (agents amont/aval, rules concernées)
- [ ] Le README est mis à jour (arborescence + tableau de référence)
- [ ] Le tag `✅ MODIFIÉ` ou `✅ NOUVEAU` est présent avec la bonne lettre
- [ ] Aucun `[NomFeature]` résiduel — utiliser `[epic-slug]` ou `[feature-slug]`
- [ ] Le message de commit est descriptif : `feat: [description]` ou `fix: [description]`
