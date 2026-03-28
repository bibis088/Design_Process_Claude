---
name: write-user-test-scenarios
description: "Prépare les scénarios de test utilisateur sur base des flux fonctionnels et des écrans Figma — tâches réalistes, critères de succès, points d'observation. Base pour l'exécution des tests. Exécuté par le ux-researcher après Gate 8."
argument-hint: "[epic-slug]/[feature-slug]"
agent: ux-researcher
---

## Rôle
Produire les scénarios de test utilisateur depuis les flux fonctionnels validés et les frames Figma finalisées — pour guider la phase d'exécution des tests (automatisée + humaine).

## Différence avec `write-usability-test`

| Skill | Périmètre | Quand |
|-------|-----------|-------|
| `write-usability-test` | Tests sur maquettes UX (wireframes) — avant direction artistique | Phase 4a |
| `write-user-test-scenarios` | Tests sur UI finale (frames Figma prêtes) — après Gate 8 | Phase 8b |

Ce skill produit des scénarios plus riches car il s'appuie sur les écrans UI finaux, le contenu réel injecté et le plan de tagage.

## Agents consommateurs
UX Researcher  · Product Designer  · QA Engineer 

## Prérequis
- [ ] Gate 8 validée — tous les écrans finalisés
- [ ] Plan de tagage révisé disponible (`review-tracking-plan`)
- [ ] Prototype React v2 disponible (`write-prototype-react`)
- [ ] Insights de recherche disponibles (`write-research-insights`)
- [ ] MCP Figma connecté pour lire les frames

## Processus de génération

### Étape 1 — Extraire les flux critiques à tester

Depuis les flux fonctionnels et le plan de tagage, identifier les flux prioritaires :

```markdown
## Flux prioritaires à tester

| Priorité | Flux | Écrans couverts | KPI lié | Risque UX identifié |
|----------|------|----------------|---------|---------------------|
| 1 | FLUX_001 — Connexion | S-01 à S-03 | Taux conversion login | Friction formulaire |
| 2 | FLUX_002 — Onboarding | S-04 à S-08 | Taux complétion onboarding | Abandon étape 3 |
| 3 | FLUX_003 — Action principale | S-09 à S-12 | Temps première action | Découvrabilité CTA |
```

### Étape 2 — Lire les frames Figma via MCP

```
get_metadata [URL page screens] → inventaire frames par flux
get_screenshot [URL frames critiques] → vérification visuelle
```

### Étape 3 — Construire les scénarios

Pour chaque flux prioritaire, créer un scénario de test complet :

```markdown
# Scénarios de test utilisateur — [feature-slug] — [YYYY-MM-DD]

---

## Scénario [N] — [Nom du flux]

### Contexte narratif
[Histoire réaliste — pas "cliquez sur le bouton" mais une situation de vie]
Ex : "Vous venez de télécharger l'app recommandée par un ami. Vous voulez
créer votre compte et effectuer votre première action."

### Personas concernés
- [PERSONA-001] regular-user — [Pourquoi ce flux le concerne]
- [PERSONA-003] edge-user — [Variante avec contraintes : connexion lente, handicap visuel...]

### Tâche principale
"[Formulation en langage naturel — jamais directive]"
✅ Ex : "Vous souhaitez [objectif utilisateur]. Montrez-moi comment vous feriez."
❌ Ex : "Cliquez sur le bouton Connexion en haut à droite."

### Tâches secondaires (si applicable)
1. "[Tâche secondaire 1]"
2. "[Tâche secondaire 2]"

### Critères de succès
| Critère | Mesure | Seuil cible |
|---------|--------|------------|
| Complétion de la tâche | Oui / Partiel / Non | ≥ 80% succès |
| Temps de complétion | Secondes | ≤ [N]s |
| Erreurs commises | Nombre | ≤ 2 |
| Aide demandée | Oui / Non | 0 intervention |
| Score facilité (SEQ) | 1-7 | ≥ 5 |

### Points d'observation critiques
À noter pendant la session :
- [ ] Hésitation à l'étape [N] — pourquoi ?
- [ ] Compréhension du label "[Libellé]" — intuitif ?
- [ ] Réaction au message d'erreur [ERR-XX] — actionnable ?
- [ ] Retour arrière inattendu — où et pourquoi ?
- [ ] Verbalisation spontanée — noter les citations exactes

### Événements analytics à observer
Depuis le plan de tagage — vérifier que ces événements se déclenchent :
- `[event_name]` → déclenché à l'étape [N]
- `[event_name]` → déclenché si erreur

### Frames de référence Figma
| Étape | Frame Figma | URL |
|-------|------------|-----|
| 1 | US_###_[nom]_ios_default | [URL] |
| 2 | US_###_[nom]_ios_error | [URL] |

### Accès au prototype
- URL prototype v2 : [URL fichier HTML]
- Device de test recommandé : [iPhone 14 / Samsung Galaxy A54]
- Mode de test : [Modéré / Non modéré]
```

### Étape 4 — Définir la matrice de couverture

```markdown
## Matrice de couverture des scénarios

| Scénario | PERSONA-001 | PERSONA-002 | PERSONA-003 | Plateforme | Mode |
|----------|------------|------------|------------|-----------|------|
| S1 — Connexion | ✅ | ✅ | ✅ | iOS + Android | Modéré |
| S2 — Onboarding | ❌ | ✅ | ✅ | iOS | Non modéré |
| S3 — Action principale | ✅ | ❌ | ✅ | Android | Modéré |

Couverture FLUX : [N/N flux critiques couverts]
Couverture personas : tous les personas testés sur au moins 2 scénarios
```

### Étape 5 — Guide de recrutement des participants

```markdown
## Participants requis

| Profil | Persona | Nombre | Critères clés |
|--------|---------|--------|--------------|
| Utilisateurs réguliers | PERSONA-001 | 5 | [Critères] |
| Nouveaux utilisateurs | PERSONA-002 | 3 | [Critères] |
| Utilisateurs aux limites | PERSONA-003 | 3 | Accessibilité, senior, expert |

Total : 11 participants minimum
```

## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. Chaque scénario est-il formulé en contexte narratif — aucune tâche directive ? (oui / non + scénarios à reformuler)
2. Les flux prioritaires couverts correspondent-ils aux KPIs définis au cadrage ? (oui / non + flux manquants)
3. Chaque persona est-il testé sur au moins 2 scénarios ? (oui / non + personas non couverts)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-user-test-scenarios "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/research/user-test-scenarios.md

⚠️ Validation Product Designer requise
"Les scénarios couvrent-ils les flux critiques du projet ?"

Prochaines étapes :
→ /run-user-tests $ARGUMENTS (exécution des tests)
→ Recruter les participants selon le guide de recrutement
```
