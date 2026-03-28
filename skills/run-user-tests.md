---
name: run-user-tests
description: "Orchestre l'exécution des tests utilisateur — partie automatisée (métriques, analytics, heatmaps) et partie humaine (sessions modérées avec enregistrements audio/vidéo). Synthétise les résultats dans un rapport consolidé."
argument-hint: "[epic-slug]/[feature-slug]"
agent: ux-researcher
---

## Rôle
Orchestrer et documenter l'exécution complète des tests utilisateur — combiner la collecte automatisée de données (métriques, analytics) avec les sessions humaines modérées (enregistrements, observations) — puis synthétiser dans un rapport actionnable.

## Architecture du test — deux dimensions complémentaires

```
Tests utilisateur
├── Partie automatisée (agent)
│   ├── Métriques prototype React (temps, clics, erreurs)
│   ├── Vérification analytics (événements plan de tagage)
│   ├── Heatmaps et flux de navigation (si outil connecté)
│   └── Score SEQ automatique (formulaire post-session)
└── Partie humaine (Product Designer)
    ├── Sessions modérées avec participants
    ├── Enregistrements audio/vidéo
    ├── Notes d'observation structurées
    └── Debriefs post-session
```

## Agents consommateurs
UX Researcher  · Product Designer  · QA Engineer 

## Prérequis
- [ ] Scénarios de test disponibles (`write-user-test-scenarios`)
- [ ] Participants recrutés selon le guide de recrutement
- [ ] Prototype React v2 accessible (`write-prototype-react`)
- [ ] Outil d'enregistrement configuré (voir section Setup)
- [ ] Consentements signés par les participants

---

## Setup — avant les sessions

### Outils recommandés

| Besoin | Outil gratuit | Outil premium |
|--------|--------------|---------------|
| Enregistrement écran + audio | Loom, OBS | Lookback, Maze |
| Visioconférence + enregistrement | Google Meet, Teams | Zoom avec transcription |
| Heatmaps prototype web | Hotjar (gratuit limité) | Hotjar, FullStory |
| Formulaire post-session | Google Forms | Typeform |
| Stockage enregistrements | Google Drive | — |

### Configuration minimale
```
1. Tester le prototype React sur le device cible avant les sessions
2. Configurer l'enregistrement écran sur le device du participant
3. Préparer le formulaire post-session (lien Google Forms)
4. Créer un dossier Drive : sessions/[feature-slug]/[date]/
5. Vérifier le consentement d'enregistrement
```

### Template de consentement d'enregistrement
```markdown
"Cette session sera enregistrée (écran + audio/vidéo) uniquement
à des fins d'amélioration du produit. Les enregistrements ne seront
partagés qu'avec l'équipe projet et seront supprimés après analyse.
Vous pouvez arrêter la session à tout moment."

Signature participant : _______________
Date : _______________
```

---

## Processus d'exécution

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
```
