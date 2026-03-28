---
name: fetch-content-for-frames
description: "Extrait du contenu (textes, images, données) depuis une URL et l'injecte dans les frames Figma correspondantes via `use_figma`. Exécuté par le ui-designer."
argument-hint: "[feature-slug]"
disable-model-invocation: false
context: fork
agent: ui-designer
---

## Rôle
Récupérer du contenu réel depuis une page web et l'injecter dans les layers texte et image des frames Figma — pour éviter les placeholders Lorem ipsum en handoff.

## Agents consommateurs
- UI Designer (pilote)

## Prérequis
- [ ] Frames Figma créées via `/setup-figma-frames $ARGUMENTS`
- [ ] URL(s) de référence fournies par le Product Designer
- [ ] Mapping URL → frames documenté (quelles données vont dans quelles frames)
- [ ] MCP Figma connecté
- [ ] Accès web disponible (web_fetch)

## Gestion des erreurs

Si l'URL est inaccessible :
> ❌ URL inaccessible ou contenu protégé — utiliser du contenu placeholder structuré et documenter les éléments à remplacer avant handoff.

Si le contenu extrait est insuffisant :
> ⚠️ Contenu partiel récupéré — documenter les éléments manquants dans le rapport et proposer des alternatives.

Si MCP Figma n'est pas accessible :
> ⚠️ MCP Figma indisponible — produire le fichier de contenu structuré à appliquer manuellement.

## Processus de génération

### Étape 1 — Collecter les URLs et le mapping

Demander à Antoine :
```
Pour injecter le contenu dans Figma, j'ai besoin de :

1. URL(s) source — quelles pages contiennent le contenu de référence ?
2. Mapping — quel contenu de quelle page va dans quelle frame ?
   Ex : "Le titre de la page d'accueil → frame S-01 / iOS / Default / layer Title"
3. Type de contenu prioritaire — textes / images / les deux ?
```

### Étape 2 — Extraire le contenu via web_fetch

Pour chaque URL fournie :

```
web_fetch(url)
→ Extraire :
  - Titres (h1, h2, h3)
  - Corps de texte (p, li)
  - Labels de boutons et liens (button, a)
  - URLs d'images (img src, background-image)
  - Données structurées (tableaux, listes)
  - Métadonnées (title, description)
```

Nettoyer le contenu extrait :
- Supprimer les balises HTML résiduelles
- Tronquer les textes trop longs selon la contrainte de la frame
- Valider que les URLs d'images sont accessibles

### Étape 3 — Mapper le contenu aux layers Figma

Créer un mapping structuré :

```markdown
## Mapping contenu → Figma

| Frame | Layer | Contenu extrait | Source | Statut |
|-------|-------|----------------|--------|--------|
| US_###_login_ios_default | Title | "Connexion à votre compte" | URL/page-login | ✅ |
| US_###_login_ios_default | Subtitle | "Entrez vos identifiants" | URL/page-login | ✅ |
| US_###_login_ios_default | hero_image | https://cdn.site.com/hero.jpg | URL/page-login | ✅ |
| [US-###] Dashboard / iOS / Default | Welcome | "Bonjour [Prénom]" | Dynamique — placeholder | ⚠️ |
```

### Étape 4 — Injecter dans Figma via `use_figma`

Pour chaque entrée du mapping :
1. Trouver le layer cible dans la frame Figma
2. Mettre à jour le contenu texte ou l'image de remplissage
3. Marquer le layer comme "Contenu réel" vs "Placeholder"

Pour les contenus dynamiques (noms, prix, dates) :
- Utiliser des données fictives réalistes — jamais "Lorem ipsum"
- Annoter le layer : `[Dynamique — à remplacer par données API]`

### Étape 5 — Générer le rapport d'injection

```markdown
# Rapport fetch-content — [feature-slug] — [YYYY-MM-DD]

## URLs traitées
| URL | Statut | Contenu extrait |
|-----|--------|----------------|
| [URL] | ✅ / ❌ / ⚠️ | [N] textes, [N] images |

## Résultat par frame
| Frame | Layers mis à jour | Layers en attente | Notes |
|-------|------------------|------------------|-------|
| US_###_login_ios_default | 4 | 1 | Hero image protégée |

## Éléments à compléter manuellement
| Frame | Layer | Raison | Action requise |
|-------|-------|--------|---------------|
| [US-###] Dashboard | User Name | Donnée dynamique | Remplacer par données réelles |

## Avertissements
- [ ] [N] images protégées non injectées — remplacer manuellement
- [ ] [N] contenus dynamiques marqués comme placeholder
```

## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. Les textes injectés sont-ils du contenu réel ou semi-réel — aucun Lorem ipsum ? (oui / non + frames concernées)
2. Les images non injectées sont-elles toutes documentées dans le rapport avec une action corrective ? (oui / non)
3. Les contenus dynamiques sont-ils annotés dans Figma pour alerter le Dev qu'ils seront remplacés par l'API ? (oui / non)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ fetch-content-for-frames "$ARGUMENTS" terminé
📋 Rapport : specs/$ARGUMENTS/figma-content-report.md

Prochaines étapes :
→ /create-figma-component $ARGUMENTS
→ Compléter manuellement les éléments en attente du rapport
```
