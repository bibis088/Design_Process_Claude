---
name: figma-use-wrapper
description: "Skill fondateur pour toute écriture sur le canvas Figma. Wrappe figma-use avec les conventions du projet — nomenclature, tokens, grilles mobiles iOS/Android. Obligatoire avant tout skill d'écriture Figma. Exécuté par ui-designer ou design-system-manager."
argument-hint: "[action à réaliser sur le canvas]"
agent: ui-designer
---

## Rôle
Skill fondateur pour toute opération d'écriture sur le canvas Figma. Ce skill doit être invoqué — explicitement ou implicitement — avant tout autre skill qui écrit dans Figma. Il wrappe le skill officiel `figma-use` de Figma avec les conventions spécifiques du projet.


## Prérequis
- [ ] MCP Figma connecté et authentifié
- [ ] `figma-read-design` exécuté sur le fichier cible
## Relation avec figma-use officiel

```
figma-use-wrapper (nos conventions)
    → figma-use (skill officiel Figma)
    → use_figma (outil MCP)
    → canvas Figma
```

`figma-use` est le skill officiel fourni par Figma qui enseigne à l'agent comment utiliser correctement l'outil `use_figma`. Ce skill le complète avec nos règles projet.

## Agents consommateurs
UI Designer  · Design System Manager 

## Prérequis obligatoires avant toute écriture

### 1. Vérifier les permissions
```
Appeler : whoami
→ Vérifier que le seat type est Full Seat
→ Si Dev Seat : écriture autorisée dans les drafts uniquement
```

### 2. Lire avant d'écrire — pipeline obligatoire
```
Étape 1 : get_metadata [URL du frame ou sélection]
    → Inventorier les layers existants
    → Éviter les doublons

Étape 2 : get_variable_defs [URL ou sélection]
    → Identifier les tokens déjà disponibles
    → Ne jamais créer un token qui existe déjà

Étape 3 : search_design_system "[nom du composant]"
    → Chercher dans les bibliothèques connectées
    → Réutiliser ce qui existe avant de créer
```

### 3. Vérifier la connexion MCP
```
Si use_figma non disponible :
→ Basculer sur instructions manuelles (voir section Fallback)
→ Documenter les actions à réaliser manuellement
```

## Conventions d'écriture obligatoires

### Nomenclature — jamais de valeur auto Figma
```
✅ Frame : US_###_login_ios_default
✅ Layer : Button — Primary — Default
✅ Composant : button/primary/default
❌ Frame : Frame 1
❌ Layer : Rectangle 4
❌ Composant : Component 1
```

Consulter `rules/figma.md` pour la nomenclature complète.

### Tokens — jamais de valeur en dur
```
✅ color/interactive/primary
✅ spacing/md
✅ radius/lg
❌ #007AFF
❌ 16
❌ 12
```

Avant toute écriture de couleur ou espacement, appeler `get_variable_defs` pour vérifier les tokens disponibles.

### Auto Layout — toujours activé sur les composants
```
✅ Auto Layout horizontal/vertical selon le composant
✅ Padding = tokens spacing/*
✅ Gap = tokens spacing/*
❌ Positionnement fixe (x, y absolus) sur les composants
```

### Grilles — mobiles uniquement
```
iOS : 4 colonnes, gouttière 16pt, marge 16pt (375pt base)
Android : 4 colonnes, gouttière 16dp, marge 16dp (360dp base)
Base unit : 8pt/dp
```

## Processus d'écriture standard

### Pour créer un frame
```
1. get_metadata → vérifier que le frame n'existe pas
2. Nommer selon convention : US_###_[nom_us]_[plateforme]_[état]
3. use_figma → créer le frame avec grille active
4. Ajouter en description : "Spec : S-XX | US-###"
```

### Pour créer un composant
```
1. search_design_system "[nom]" → vérifier l'existence
2. get_variable_defs → identifier les tokens disponibles
3. use_figma → créer avec auto layout + tokens + variants
4. Nommer : [Catégorie] / [Nom] / [Variante] / [État]
```

### Pour modifier un élément existant
```
1. get_metadata [URL] → identifier le node ID exact
2. get_screenshot [URL] → vérifier l'état visuel actuel
3. use_figma → modifier en ciblant le node ID
4. get_screenshot [URL] → vérifier le résultat
```

## Gestion des erreurs
> ❌ Erreur écriture Figma — vérifier : seat type, permissions sur le fichier, connexion MCP.
> ⚠️ Composant existant trouvé — réutiliser plutôt que recréer. URL : [lien bibliothèque]
> ❌ Tokens manquants — lancer d'abord `/setup-figma-tokens [epic-slug]`.

## Fallback — MCP indisponible ou Dev Seat

Si `use_figma` n'est pas accessible, produire un document d'instructions manuelles :

```markdown
# Instructions Figma manuelles — [Action] — [YYYY-MM-DD]

## Contexte
Action non réalisable automatiquement : [raison]

## Étapes à réaliser manuellement dans Figma

| # | Action | Dans quel fichier | Page | Notes |
|---|--------|------------------|------|-------|
| 1 | [Action] | [NomFichier] | [Page] | [Convention à respecter] |

## Conventions à appliquer
- Nommage : [convention]
- Tokens : [tokens à utiliser]
- Auto layout : [configuration]
```

## Phase de validation — niveau léger

Avant de clore ce skill, confirme ce point :

1. La règle "lire avant écrire" a-t-elle été respectée — `get_metadata` et `get_variable_defs` appelés avant `use_figma` ? (oui / non — si non, relancer la lecture avant de valider)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ figma-use-wrapper "$ARGUMENTS" terminé
📋 Action réalisée sur le canvas Figma

Prochaines étapes :
→ /figma-read-design [URL] (vérifier le résultat)
→ Skill suivant selon le workflow en cours

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```
