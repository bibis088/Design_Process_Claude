---
name: write-release-notes
description: "Génère les release notes orientées utilisateur pour une version — App Store, Play Store et in-app. Exécuté par le product-owner."
argument-hint: "[version] [epic-slug]"
agent: product-owner
---

## Rôle
Produire les release notes utilisateur pour une version livrée — résumé des nouveautés, corrections et améliorations — dans les formats requis par l'App Store, le Play Store et la communication in-app.

## Agents consommateurs
Product Owner 

## Prérequis
- [ ] US-### en statut `Done` pour cette version
- [ ] Rapport QA validé (`QA-###` en statut `Approved`)
- [ ] Version définie (ex: `1.2.0`)

## Gestion des erreurs
> ❌ US non terminées ou QA manquant — finaliser la version avant de rédiger les release notes.

## Processus de génération

### Étape 1 — Collecter les US livrées

Depuis `specs/[epic-slug]/stories/`, lister toutes les `US-###` en statut `Done` pour cette version.

Regrouper par catégorie :
- **Nouvelles fonctionnalités** — US nouvelles
- **Améliorations** — US d'amélioration d'existant
- **Corrections** — bugs corrigés (`BUG-###` résolus)

### Étape 2 — Rédiger les release notes

**Règles de rédaction :**
- Orienté bénéfice utilisateur — pas de jargon technique
- Court et scannable — l'utilisateur lit en 10 secondes
- Ton cohérent avec la direction de marque
- En français (langue du projet)
- Maximum 500 caractères pour App Store / Play Store

```markdown
# Release Notes — v[X.X.X] — [YYYY-MM-DD]

## Format App Store / Play Store (500 caractères max)
[Texte court et accrocheur pour les stores]

Exemple :
"Cette version apporte [fonctionnalité principale]. Vous pouvez maintenant [bénéfice clé].
Nous avons également amélioré [amélioration] et corrigé [problème connu].
Merci pour vos retours !"

---

## Format in-app (notification ou écran "Nouveautés")

### Titre
"Nouveautés de la version [X.X.X]"

### Nouvelles fonctionnalités
- 🎉 **[Fonctionnalité]** — [Bénéfice en une phrase]
- 🎉 **[Fonctionnalité]** — [Bénéfice en une phrase]

### Améliorations
- ✨ [Amélioration] — [Impact pour l'utilisateur]

### Corrections
- 🐛 [Problème corrigé] — [Description simple]

---

## Référence technique (usage interne — non publiée)

| US / BUG | Titre | Type | Impacte |
|----------|-------|------|---------|
| US-042 | [Titre] | Nouvelle fonctionnalité | [Écrans] |
| BUG-007 | [Titre] | Correction | [Écrans] |

Version DS utilisée : [X.X.X]
Build : [numéro de build]
```

## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. Le texte App Store / Play Store est-il ≤ 500 caractères et orienté bénéfice utilisateur — aucun jargon technique ? (oui / non + passage à retravailler)
2. Toutes les US `Done` de cette version sont-elles couvertes dans les release notes — aucune oubliée ? (oui / non + US manquantes)
3. Le ton est-il cohérent avec la direction de marque définie lors de la direction artistique ? (oui / non)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-release-notes "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/release-notes-v[X.X.X].md

Prochaines étapes :
→ Soumettre le texte App Store sur App Store Connect
→ Soumettre le texte Play Store sur Google Play Console
→ Intégrer l'écran "Nouveautés" dans Figma si in-app

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```
