---
name: write-usability-test
description: "Produit le protocole de test d'utilisabilité et le template de rapport. Guide le Product Designer pour tester les maquettes UX avec de vrais utilisateurs. Exécuté par le ux-researcher après les maquettes UX."
argument-hint: "[feature-slug]"
agent: ux-researcher
---

## Rôle
Produire le protocole de test d'utilisabilité et le template de rapport pour tester les maquettes UX avec de vrais utilisateurs — avant la direction artistique et la création des écrans UI.

## Agents consommateurs
UX Researcher  · Product Designer  · UX Designer 

## Prérequis
- [ ] Maquettes UX disponibles dans Figma (user flows visuels + specs d'écran)
- [ ] Prototype Figma navigable configuré
- [ ] Insights de recherche disponibles (`write-research-insights`)
- [ ] Participants recrutés (mêmes profils que les interviews)

## Processus

### Étape 1 — Définir les scénarios de test

Depuis les flux fonctionnels et les insights, définir les tâches à tester :

```markdown
## Scénarios de test — [feature-slug]

| # | Scénario | Flux testé | Critère de succès | Temps max |
|---|----------|-----------|------------------|----------|
| T1 | "[Tâche formulée en langage utilisateur]" | FLUX-### | [Ce qui constitue un succès] | [N min] |
| T2 | "[Tâche]" | FLUX-### | [Critère] | [N min] |
```

**Règle de formulation des tâches :**
- ✅ "Imaginez que vous voulez [objectif utilisateur]. Montrez-moi comment vous feriez."
- ❌ "Cliquez sur le bouton Connexion" — trop directif, invalide le test

### Étape 2 — Protocole de test

```markdown
# Protocole de test d'utilisabilité — [feature-slug] — [YYYY-MM-DD]

## Informations pratiques
- Format : [Modéré à distance / Modéré en présentiel / Non modéré]
- Durée : [45-60 minutes]
- Outil : [Figma prototype — URL]
- Enregistrement : [Oui — avec consentement]
- Nombre de participants : [5 recommandés]

## Introduction (5 min)
Script :
"Bonjour, aujourd'hui je vais vous demander d'utiliser un prototype
d'application. Ce n'est pas un test de vos compétences — c'est un test
du design. Il n'y a pas de mauvaise réponse.
Pensez à voix haute pendant que vous naviguez — dites ce que vous
pensez, ce que vous attendez, ce qui vous surprend.
Si vous êtes bloqué, c'est une information précieuse pour nous."

## Warm-up (5 min)
- "Vous avez déjà utilisé [type d'app similaire] ?"
- "Sur quoi naviguez-vous habituellement — iOS ou Android ?"

## Tâches (30-40 min)

### Tâche 1
Énoncé : "[Scénario T1]"
Donner l'énoncé à l'écrit — ne pas expliquer.
Observer sans intervenir sauf si bloqué depuis > 3 minutes.
Grille d'observation :
- Chemin emprunté : [noter chaque écran visité]
- Points de friction : [où l'utilisateur hésite]
- Erreurs : [mauvaise action, retour en arrière]
- Verbatim : "[Citations pendant la navigation]"
- Résultat : [Succès / Partiel / Échec]

### Tâche 2
[Même structure]

## Questions post-test (5-10 min)
- "Quelle tâche vous a semblé la plus difficile ? Pourquoi ?"
- "Qu'est-ce qui vous a le plus surpris ?"
- "Si vous deviez changer une chose, ce serait quoi ?"
- "Sur une échelle de 1 à 5, comment évaluez-vous la facilité d'utilisation ?"
```

### Étape 3 — Template de rapport

```markdown
# Rapport Test Utilisabilité — [feature-slug] — [YYYY-MM-DD]

## Résumé
[N] participants testés — [N] tâches — [Score moyen facilité : X/5]

## Résultats par tâche

### Tâche 1 — [Énoncé]
| Participant | Résultat | Temps | Points de friction |
|-------------|----------|-------|-------------------|
| P1 | ✅ Succès | [N min] | [Friction si applicable] |
| P2 | ⚠️ Partiel | [N min] | [Friction] |
| P3 | ❌ Échec | — | [Où bloqué] |

Taux de succès : [N/N]
Problème principal identifié : [Description]

## Problèmes d'utilisabilité

| Sévérité | Écran | Problème | Fréquence | Recommandation |
|----------|-------|---------|-----------|----------------|
| 🔴 Critique | S-XX | [Problème] | [N/N] | [Correction] |
| 🟡 Majeur | S-XX | [Problème] | [N/N] | [Correction] |
| 🟢 Mineur | S-XX | [Problème] | [N/N] | [Correction] |

## Corrections recommandées pour le UX Designer

### Avant direction artistique — bloquant
- [S-XX] : [Correction précise à apporter]

### Après direction artistique — important
- [S-XX] : [Amélioration à prévoir]
```

## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. Les scénarios de test couvrent-ils les flux principaux identifiés comme critiques dans les insights ? (oui / non + flux manquants)
2. Les tâches sont-elles formulées en langage utilisateur — aucune tâche directive qui biaiserait le test ? (oui / non + tâches à reformuler)
3. Les corrections bloquantes sont-elles clairement distinguées des améliorations non bloquantes ? (oui / non)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-usability-test "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/research/usability-test-protocol.md
📁 specs/$ARGUMENTS/research/usability-test-report.md

Prochaines étapes :
→ UX Designer : intégrer les corrections bloquantes dans les specs d'écran
→ /write-screen-spec $ARGUMENTS (après corrections)
→ Corrections mineures intégrées après direction artistique
```
