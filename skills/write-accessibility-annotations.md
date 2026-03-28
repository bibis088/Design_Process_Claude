---
name: write-accessibility-annotations
description: "Produit les annotations d'accessibilité RAAM pour un écran — labels, ordre de focus, zones tactiles, contrastes — prêtes à intégrer dans Figma et les specs d'écran. Exécuté par le ux-designer."
argument-hint: "[feature-slug]/[screen-id]-[screen-slug]"
agent: ux-designer
---

## Rôle
Produire les annotations d'accessibilité RAAM complètes pour un écran spécifique — document de référence pour les annotations Figma et l'implémentation Dev.

## Agents consommateurs
UX Designer  · UI Designer  · QA Engineer 

## Distinction avec `write-accessibility-spec`

| Skill | Périmètre | Granularité |
|-------|-----------|------------|
| `write-accessibility-spec` | Feature entière | Vue d'ensemble — critères RAAM applicables, checklist globale |
| `write-accessibility-annotations` | Écran par écran | Détail opérationnel — chaque élément annoté individuellement |

Ce skill est complémentaire — il produit le détail opérationnel d'un écran spécifique.

## Prérequis
- [ ] Spec d'écran (`S-XX-[nom].md`) disponible dans `./design/[feature-slug]/ux/screens/`
- [ ] Spec accessibilité RAAM feature disponible dans `./design/[feature-slug]/ux/accessibility-spec.md`
- [ ] URL du frame Figma renseignée dans la spec d'écran
- [ ] `rules/accessibility.md` consulté pour les critères applicables

## Gestion des erreurs
> ❌ Spec d'écran manquante — produire d'abord `/write-screen-spec $ARGUMENTS`.

## Processus de génération

### Étape 1 — Inventorier tous les éléments de l'écran
Depuis la spec d'écran (`S-XX-[nom].md`), lister exhaustivement :
- Tous les éléments interactifs (boutons, liens, champs, toggles, sliders)
- Toutes les images (informatives vs décoratives)
- Tous les textes (titres, corps, labels, messages d'erreur)
- Tous les gestes disponibles (tap, swipe, pinch, long press)

### Étape 2 — Appliquer les critères RAAM par élément

Pour chaque élément, identifier :
- Le critère RAAM applicable
- Le niveau (A ou AA)
- L'annotation requise

### Étape 3 — Rédiger les annotations

```markdown
# Annotations Accessibilité — [S-XX] [Nom de l'écran]

## Métadonnées
- Écran : [S-XX-[nom]]
- Feature : [FEAT-###]
- US couvertes : [US-###, US-###]
- Frame Figma : [URL directe]
- Statut annotations Figma : [To Do / In Progress / Done]
- UX Designer : [nom]
- Date : [YYYY-MM-DD]

## Éléments interactifs

| # | Élément | Type | Label accessible | Hint | Rôle | RAAM | Niveau |
|---|---------|------|----------------|------|------|------|--------|
| 1 | [Bouton Connexion] | Button | "Se connecter à votre compte" | — | Button | 6.1 | A |
| 2 | [Champ Email] | TextField | "Adresse email" | "Format attendu : nom@exemple.com" | TextField | 6.2 | A |
| 3 | [Champ Mot de passe] | SecureField | "Mot de passe" | "8 caractères minimum" | SecureField | 6.2 | A |
| 4 | [Lien Mot de passe oublié] | Link | "Réinitialiser mon mot de passe" | — | Link | 5.1 | A |
| 5 | [Toggle Rester connecté] | Toggle | "Rester connecté" | "Activé : session conservée 30 jours" | Switch | 6.1 | A |

## Images

| # | Élément | Type | Alt text | RAAM | Niveau |
|---|---------|------|---------|------|--------|
| 1 | [Logo app] | Informative | "Logo [NomApp]" | 1.1 | A |
| 2 | [Illustration fond] | Décorative | (ignorée par VoiceOver/TalkBack) | 1.2 | A |
| 3 | [Icône cadenas] | Informative | "Connexion sécurisée" | 1.1 | A |

## Ordre de focus

Navigation logique de haut en bas, gauche à droite :

```
1 → Titre de l'écran "[Nom]"
2 → Champ Email
3 → Champ Mot de passe
4 → Toggle Rester connecté
5 → Bouton Connexion
6 → Lien Mot de passe oublié
```

Comportements spécifiques :
- iOS : swipe back depuis le bord gauche → retour écran précédent
- Android : bouton back système → retour écran précédent
- Clavier physique : Tab navigue dans l'ordre ci-dessus

## Zones tactiles

| Élément | Taille visuelle | Zone tactile iOS | Zone tactile Android | Conforme |
|---------|----------------|-----------------|---------------------|---------|
| [Bouton Connexion] | 100% largeur × 48pt | 44x44pt min ✅ | 48x48dp min ✅ | ✅ |
| [Lien Mot de passe oublié] | 120pt × 20pt | Agrandir à 44pt ⚠️ | Agrandir à 48dp ⚠️ | ❌ |
| [Toggle] | 51pt × 31pt | 44x44pt ✅ | 48x48dp ✅ | ✅ |

## Contrastes

| Élément | Token couleur fond | Token couleur texte | Ratio estimé | Cible | RAAM | Conforme |
|---------|------------------|-------------------|-------------|-------|------|---------|
| Label champ | `color.background.primary` | `color.content.secondary` | À mesurer | ≥ 4.5:1 | 2.2 | ⚠️ À vérifier |
| Bouton CTA | `color.interactive.primary` | `color.content.onPrimary` | À mesurer | ≥ 3:1 | 2.4 | ⚠️ À vérifier |
| Message erreur | `color.feedback.error` | `color.content.onError` | À mesurer | ≥ 4.5:1 | 2.2 | ⚠️ À vérifier |

## Gestes et alternatives

| Geste | Disponible | Alternative | RAAM | Niveau |
|-------|-----------|-----------|------|--------|
| Swipe gauche pour supprimer | Non applicable sur cet écran | — | — | — |
| Tap pour soumettre | ✅ | — | 12.1 | A |

## Messages d'état pour les technologies d'assistance

| État | Message VoiceOver / TalkBack | Déclencheur |
|------|---------------------------|------------|
| Chargement en cours | "Connexion en cours, veuillez patienter" | Tap sur bouton Connexion |
| Erreur email | "Erreur : adresse email invalide. Champ email" | Soumission avec email invalide |
| Erreur réseau | "Erreur de connexion. Vérifiez votre réseau et réessayez" | Timeout réseau |
| Succès | "Connexion réussie. Redirection vers l'accueil" | Auth OK |

## Checklist annotations Figma

- [ ] Labels accessibles ajoutés sur tous les éléments interactifs
- [ ] Ordre de focus numéroté (1→N) visible sur le frame
- [ ] Zones tactiles : overlay 44x44pt iOS / 48x48dp Android
- [ ] Ratios de contraste indiqués sur textes et composants interactifs
- [ ] Images : alt text ou "décoratif" indiqué
- [ ] Messages d'état documentés dans les notes du frame
- [ ] Référence spec dans la description du frame Figma :
  ```
  Spec : S-XX-[nom] | US-###, US-### | FLUX-###
  Statut : Ready for dev
  ```
```

## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. Tous les éléments interactifs de l'écran ont-ils un label accessible documenté — aucun oublié ? (oui / non + éléments manquants)
2. L'ordre de focus est-il logique et cohérent avec le parcours utilisateur ? (oui / non + corrections)
3. Les zones tactiles non conformes sont-elles toutes identifiées avec une action corrective ? (oui / non)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-accessibility-annotations "$ARGUMENTS" terminé
📁 design/[feature-slug]/ux/screens/S-XX-[nom]-annotations.md

Prochaines étapes :
→ Intégrer les annotations dans Figma (frame : [URL])
→ /write-component-ui $ARGUMENTS (implémentation avec labels)
→ /write-qa-report [epic-slug]/[us-slug] (vérification QA)
```
