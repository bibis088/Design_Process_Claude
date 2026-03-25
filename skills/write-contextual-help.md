---
name: write-contextual-help
description: Spécifie l'aide contextuelle d'une feature — empty states, messages d'erreur explicites et labels d'action — intégrée directement dans les specs d'écran. Exécuté par le ux-designer.
argument-hint: [feature-slug]
disable-model-invocation: false
context: fork
agent: ux-designer
---

## Rôle
Produire les textes d'aide contextuelle pour chaque écran d'une feature — messages d'erreur, états vides, tooltips inline — directement exploitables par le UI Designer et le Dev.

## Agents consommateurs
- UX Designer (pilote)
- UI Designer (consommateur — intègre dans les frames)
- Dev (consommateur — implémente les strings)

## Prérequis
- [ ] Specs d'écran `S-XX-[nom].md` disponibles dans `./design/[feature-slug]/ux/screens/`
- [ ] User stories et critères d'acceptance disponibles

## Gestion des erreurs

Si les prérequis ne sont pas remplis :
> ❌ Specs d'écran manquantes — lancer d'abord `/write-screen-spec $ARGUMENTS`.

## Processus de génération

### Étape 1 — Inventorier tous les états contextuels

Pour chaque écran, identifier :
- État `Empty` — quand il n'y a pas de contenu
- État `Error` — erreurs réseau, validation, permissions
- État `Loading` — messages de chargement si texte nécessaire
- Tooltips et messages d'aide inline

### Étape 2 — Rédiger les textes

**Règles de rédaction :**
- Les messages d'erreur expliquent **quoi faire**, pas juste ce qui s'est passé
- Les états vides proposent toujours une **action** (CTA)
- Le ton est cohérent avec la direction de marque
- Les textes sont en français (langue du projet)
- Pas de jargon technique visible pour l'utilisateur

```markdown
# Aide Contextuelle — [NomFeature] — [YYYY-MM-DD]

## [S-XX] [Nom de l'écran]

### État Empty
- **Titre** : [ex: "Aucune tâche pour le moment"]
- **Sous-titre** : [ex: "Commencez par créer votre première tâche"]
- **CTA** : [ex: "Créer une tâche"]
- **Accessibilité** : [Label VoiceOver pour l'illustration si applicable]

### Messages d'erreur

| Code erreur | Message affiché | Action proposée | Accessibilité |
|-------------|----------------|----------------|--------------|
| Réseau indisponible | "Impossible de se connecter. Vérifiez votre connexion." | Bouton "Réessayer" | "Erreur réseau. Bouton Réessayer disponible." |
| Champ vide | "Ce champ est obligatoire." | Focus retourné sur le champ | "Erreur : [Nom du champ] est obligatoire." |
| Erreur serveur | "Une erreur est survenue. Nous travaillons à la résoudre." | Bouton "Contacter le support" | "Erreur serveur. Bouton Contacter le support disponible." |
| Permission refusée | "Autorisez l'accès à [ressource] pour continuer." | Bouton "Modifier les paramètres" | "Permission requise. Bouton Modifier les paramètres disponible." |

### Tooltips / aide inline
| Élément | Texte d'aide | Déclencheur |
|---------|-------------|------------|
| [Champ complexe] | [Explication courte] | Tap sur icône ? |
```

## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. Chaque message d'erreur propose-t-il une action concrète — pas juste un constat ? (oui / non + messages à retravailler)
2. Chaque état vide a-t-il un CTA visible qui guide l'utilisateur vers la prochaine action ? (oui / non + états manquants)
3. Les textes sont-ils cohérents avec le ton défini lors de la direction artistique ? (oui / non)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-contextual-help "$ARGUMENTS" terminé
📁 design/$ARGUMENTS/ux/contextual-help.md

Prochaines étapes :
→ Intégrer les textes dans les frames Figma correspondantes
→ /write-onboarding-spec $ARGUMENTS (si onboarding prévu)
→ /write-accessibility-annotations $ARGUMENTS (vérifier les labels)
```
