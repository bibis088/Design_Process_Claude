---
name: run-user-tests
description: "Prépare les scénarios de test utilisateur depuis les frames Figma validées puis orchestre l'exécution — partie automatisée (métriques, analytics) et sessions humaines (enregistrements audio/vidéo). Use when user says 'tests utilisateur', 'test ui', 'run user tests', 'scénarios de test', or 'user testing'."
argument-hint: "[epic-slug]/[feature-slug]"
agent: ux-researcher
---

## Rôle
Préparer les scénarios depuis les flows et frames Figma, puis exécuter les tests en deux parties — automatisée et humaine — et synthétiser les résultats.


## Prérequis
- [ ] Gate 8 validée — tous les écrans finalisés
- [ ] US-### disponibles dans `specs/[epic-slug]/stories/`
- [ ] MCP Figma connecté
- [ ] Participants recrutés · consentements signés
- [ ] Outil d'enregistrement configuré (Loom / OBS / Google Meet)
- [ ] Espace de stockage configuré pour les enregistrements (Drive, Dropbox ou équivalent)
- [ ] Frames Figma de l'OS de base en page `Validated UI`
- [ ] Prototype React v2 disponible (`write-prototype-react`)
- [ ] Scénarios de test disponibles (Phase A du skill)
- [ ] Participants recrutés (5 regular / 3 new / 3 edge)

## Phase A — Préparation des scénarios
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

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```

---

## Phase B — Exécution des tests
### Partie A — Automatisée (agent exécute)

#### A1 — Métriques prototype React

L'agent injecte un script de tracking dans le prototype React avant les sessions :

```javascript
// Script de métriques à ajouter dans le prototype
const TestMetrics = {
  sessionId: `session_${Date.now()}`,
  events: [],

  track(type, data) {
    this.events.push({
      type,
      timestamp: Date.now(),
      screen: data.screen,
      element: data.element,
      ...data
    });
  },

  // Appeler en fin de session
  export() {
    return JSON.stringify(this.events, null, 2);
  }
};

// Sur chaque navigation
TestMetrics.track('screen_view', { screen: currentScreen });
// Sur chaque tap
TestMetrics.track('element_tap', { screen: currentScreen, element: elementId });
// Sur chaque erreur
TestMetrics.track('error_shown', { screen: currentScreen, errorCode });
```

Métriques collectées automatiquement :
- Temps passé par écran
- Nombre de taps par élément
- Erreurs déclenchées
- Chemins de navigation empruntés
- Taux de complétion par scénario

#### A2 — Formulaire post-session automatique

Google Form envoyé automatiquement après chaque session :

```markdown
## Formulaire post-session — [NomProjet]

1. Facilité globale d'utilisation (SEQ) : 1 (très difficile) → 7 (très facile)
2. Quelle tâche vous a semblé la plus difficile ? (texte libre)
3. Qu'est-ce qui vous a le plus surpris ? (texte libre)
4. Recommanderiez-vous cette app à un ami ? 1-10 (NPS)
5. Commentaires libres (texte libre)
```

#### A3 — Vérification plan de tagage

L'agent vérifie que les événements du plan de tagage se déclenchent correctement pendant les sessions :

```markdown
## Vérification événements analytics

| Événement | Attendu | Déclenché | Propriétés correctes | Notes |
|-----------|---------|-----------|---------------------|-------|
| `login_attempted` | Oui | ✅ | ✅ | — |
| `onboarding_step_viewed` | Oui | ❌ | — | Pas déclenché sur step 2 |
```

---

### Partie B — Humaine (Product Designer conduit)

#### B1 — Guide de session pour le Product Designer

```markdown
## Protocole de session — [Product Designer]

### Avant la session (5 min)
- [ ] Tester le prototype sur le device du participant
- [ ] Lancer l'enregistrement (écran + audio)
- [ ] Lire le script d'introduction
- [ ] Vérifier le consentement signé

### Script d'introduction
"Bonjour, merci de participer. Je vais vous demander d'utiliser un prototype
d'application. Ce n'est pas un test de vos compétences — c'est un test du design.
Il n'y a pas de bonne ou mauvaise réponse. Pensez à voix haute pendant
que vous naviguez. Je ne pourrai pas vous aider pendant les tâches —
c'est votre réaction naturelle qui nous intéresse."

### Pendant la session
- Ne pas intervenir sauf blocage > 3 minutes
- Si blocage : "Que cherchez-vous à faire ?"
- Relancer le think-aloud : "Que pensez-vous ?"
- Noter les citations exactes en temps réel

### Grille d'observation en temps réel
| Scénario | Résultat | Temps | Erreurs | Citations | Comportement notable |
|----------|---------|-------|---------|-----------|---------------------|
| S1 | ✅/⚠️/❌ | [s] | [N] | "[...]" | [Observation] |

### Post-session (10 min)
- Envoyer le lien formulaire post-session
- Débrief : "Y a-t-il quelque chose que vous auriez voulu que je vous demande ?"
- Arrêter l'enregistrement
- Nommer le fichier : `session_[N]_[persona]_[YYYY-MM-DD].[format]`
- Uploader sur Drive : `sessions/[feature-slug]/[date]/`
```

#### B2 — Fichiers d'enregistrement

Format de nommage des enregistrements :
```
session_01_regular-user_2026-03-28.mp4
session_01_regular-user_2026-03-28_audio.m4a
session_02_new-user_2026-03-28.mp4
[...]
```

Structure Drive :
```
sessions/
└── [feature-slug]/
    └── [YYYY-MM-DD]/
        ├── session_01_regular-user.mp4
        ├── session_01_notes.md
        ├── session_02_new-user.mp4
        └── session_02_notes.md
```

---

### Partie C — Synthèse (agent consolide)

Le Product Designer fournit :
- Les grilles d'observation remplies
- Les résultats du formulaire post-session
- Les citations notées
- Les métriques exportées du prototype

L'agent produit le rapport de synthèse :

```markdown
# Rapport Tests Utilisateur — [feature-slug] — [YYYY-MM-DD]

## Résumé exécutif

| Métrique | Valeur | Cible | Statut |
|----------|--------|-------|--------|
| Participants testés | [N] | [N] | ✅ |
| Taux de complétion global | [%] | ≥ 80% | ✅/❌ |
| Score SEQ moyen | [N]/7 | ≥ 5 | ✅/❌ |
| NPS moyen | [N]/10 | ≥ 7 | ✅/❌ |
| Erreurs critiques identifiées | [N] | 0 | ✅/❌ |

## Résultats par scénario

### Scénario 1 — [Nom]
| Participant | Persona | Résultat | Temps | Erreurs | SEQ |
|------------|---------|---------|-------|---------|-----|
| P1 | regular-user | ✅ Succès | 45s | 0 | 6/7 |
| P2 | new-user | ⚠️ Partiel | 2m10s | 2 | 4/7 |
| P3 | edge-user | ❌ Échec | — | 5 | 2/7 |

Taux de succès : [N/N] — [%]
Problème principal : [Description]

## Insights consolidés (automatisé + humain)

### ✅ Ce qui fonctionne bien
- [Élément validé — source : [N] participants + métrique prototype]

### ⚠️ Points de friction

#### Friction 1 — [Titre court]
**Fréquence :** [N/N participants] ont rencontré ce problème
**Écran :** [US_###_nom_ios_default] — [URL Figma]
**Observation humaine :** "[Citation directe représentative]"
**Donnée automatisée :** Temps moyen sur cet écran : [Ns] vs attendu [Ns]
**Événement analytics :** `[event_name]` déclenché [N] fois de plus qu'attendu
**Recommandation :** [Action corrective précise]
**Priorité :** [Critique / Haute / Moyenne]

#### Friction 2 — [Titre court]
[Même structure]

### Enregistrements — moments clés
| Session | Timecode | Description | URL Drive |
|---------|---------|-------------|-----------|
| session_01 | 02:34 | Utilisateur bloqué sur le champ email | [URL] |
| session_03 | 05:12 | Réaction de surprise positive sur l'animation | [URL] |

## Vérification plan de tagage
[N/N] événements correctement déclenchés
Anomalies détectées : [liste si applicable]

## Actions recommandées

### 🔴 Critique — avant mise en production
| # | Friction | Écran | Action | Responsable |
|---|---------|-------|--------|------------|
| 1 | [Friction] | S-XX | [Correction] | UI Designer |

### 🟡 Haute — V suivante
| # | Friction | Écran | Action |
|---|---------|-------|--------|

### 🟢 Backlog
[Améliorations non bloquantes]
```

## Phase de validation — niveau approfondi

Avant de valider ce livrable et de passer à la suite, réponds à ces questions dans l'ordre :

1. Les données automatisées et humaines sont-elles consolidées pour chaque scénario — aucune source manquante ? (oui / non + sources manquantes)
2. Chaque friction identifiée a-t-elle une citation directe ET une donnée chiffrée — pas de friction basée sur une seule source ? (oui / non + frictions à étayer)
3. Les enregistrements sont-ils nommés correctement et uploadés sur Drive — liens inclus dans le rapport ? (oui / non)
4. Le plan de tagage a-t-il été vérifié — anomalies documentées si applicable ? (oui / non)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ run-user-tests "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/research/user-test-report.md
🎥 Enregistrements : Drive → sessions/[feature-slug]/[date]/

⚠️ Validation Product Designer requise (lot Gate 9)
"Les résultats des tests nécessitent-ils des corrections avant handoff ?"

Prochaines étapes :
→ Corrections critiques → retour UI Designer si nécessaire
→ /review-tracking-plan $ARGUMENTS (si anomalies analytics)
→ /write-functional-doc [epic-slug] (inclure les résultats de tests)
→ /write-figma-handoff [feature-slug] (si aucune correction critique)

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```

---

## Résumé de fin d'exécution

```
✅ run-user-tests "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/research/user-test-scenarios.md
📁 specs/$ARGUMENTS/research/user-test-report.md
🎥 Enregistrements : Drive → sessions/[feature-slug]/[date]/

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]

Prochaines étapes :
→ /write-tracking-plan $ARGUMENTS
→ Gate 9 (lot)
```
