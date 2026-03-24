# Conventions pour le Quality Assurance

## Rôle du QA dans la chaîne des agents

Le QA intervient en **fin de chaîne** — après que le développement a consommé les specs fonctionnelles et les specs UI. Son rôle est de vérifier que ce qui a été livré correspond à ce qui a été spécifié.

```
BA → PO → UX → UI → Dev → QA → Done
```

Le QA ne recrée pas les specs — il les consomme comme référence de vérification.

---

## Documents consommés par le QA

| Document | Source | Utilisé pour |
|----------|--------|-------------|
| User stories (`US-###`) | Product Owner | Vérifier les critères d'acceptance |
| Règles métier (`RB-###`) | Business Analyst | Vérifier les contraintes métier |
| Specs d'écran (`S-XX-[nom].md`) | UX Designer | Vérifier les états et transitions |
| Composants UI | UI Designer | Vérifier les états visuels et l'accessibilité |
| DoD de chaque livrable | Agents concernés | Gate de validation avant `Done` |

---

## Types de tests à couvrir

### 1. Tests fonctionnels
Vérifier que chaque critère d'acceptance (`CA-##`) de chaque `US-###` est satisfait.

| Critère | Méthode de vérification |
|---------|------------------------|
| Flux heureux (happy path) | Test manuel ou automatisé du parcours nominal |
| Flux alternatifs (`ALT-##`) | Test de chaque branche alternative |
| Cas d'erreur (`ERR-##`) | Simulation des conditions d'erreur |

### 2. Tests d'accessibilité
- VoiceOver (iOS) : navigation complète au lecteur d'écran
- TalkBack (Android) : navigation complète au lecteur d'écran
- Texte agrandi (Dynamic Type / Large Font) : lisibilité préservée
- Contraste : minimum 4.5:1 pour texte normal, 3:1 pour grands textes
- Zones tactiles : minimum 44x44pt (iOS) / 48x48dp (Android)

### 3. Tests de régression
Après toute modification, vérifier que les fonctionnalités existantes ne sont pas impactées.
- Périmètre de régression défini par le PO avant chaque sprint
- Prioriser les flux critiques : authentification, paiement, données utilisateur

### 4. Tests de performance
- Temps de chargement des listes : < 2s
- Temps de réponse des actions : < 100ms pour le feedback visuel
- Comportement en connexion dégradée (3G, offline)

### 5. Tests de compatibilité
| Plateforme | Versions à couvrir |
|------------|-------------------|
| iOS | 2 dernières versions majeures |
| Android | 3 dernières versions majeures + appareils entrée de gamme |

---

## Format du rapport de test

```markdown
# Rapport QA — [US-### ou FEAT-###] — [YYYY-MM-DD]

## Métadonnées
- Feature : [FEAT-###]
- Stories testées : [US-###, US-###]
- Plateforme : [iOS / Android / Les deux]
- Version testée : [numéro de build]
- Statut global : [Pass / Fail / Partiel]

## Résultats par critère d'acceptance

| CA | Énoncé | Statut | Notes |
|----|--------|--------|-------|
| CA-01 | [Énoncé] | ✅ Pass / ❌ Fail / ⚠️ Partiel | [Observation] |
| CA-02 | [Énoncé] | ✅ Pass / ❌ Fail / ⚠️ Partiel | [Observation] |

## Bugs identifiés

| ID | Sévérité | Description | Étapes de reproduction | CA impacté |
|----|----------|-------------|----------------------|-----------|
| BUG-001 | [Critique / Majeur / Mineur] | [Description] | [Étapes] | [CA-##] |

## Accessibilité
- [ ] VoiceOver iOS : [Pass / Fail — détails]
- [ ] TalkBack Android : [Pass / Fail — détails]
- [ ] Texte agrandi : [Pass / Fail — détails]
- [ ] Contrastes : [Pass / Fail — détails]

## Verdict
- [ ] ✅ Approuvé — story peut passer en `Done`
- [ ] ❌ Refusé — [N] bugs bloquants à corriger
- [ ] ⚠️ Approuvé avec réserves — bugs mineurs à corriger en V+1
```

---

## Niveaux de sévérité des bugs

| Sévérité | Définition | Impact sur le statut |
|----------|-----------|---------------------|
| **Critique** | Crash, perte de données, sécurité compromise | Bloque le passage en `Done` — correction immédiate |
| **Majeur** | Fonctionnalité principale inutilisable, CA bloquant non satisfait | Bloque le passage en `Done` |
| **Mineur** | Comportement incorrect mais contournable, CA secondaire | Peut passer en `Done` avec ticket de suivi |
| **Cosmétique** | Problème visuel sans impact fonctionnel | Ne bloque pas — backlog V+1 |

---

## Convention de nommage des bugs

```
BUG-### — [plateforme] — [description courte]
```

Exemples :
- `BUG-001 — iOS — Bouton connexion reste désactivé après succès`
- `BUG-002 — Android — Liste vide n'affiche pas le CTA`

---

## Statuts QA

| Statut | Condition |
|--------|-----------|
| `To Test` | Story en `Done` côté Dev, prête pour QA |
| `In Progress` | Tests en cours |
| `Pass` | Tous les CA satisfaits, aucun bug bloquant |
| `Fail` | Au moins un bug Critique ou Majeur |
| `Approved` | PO a validé le rapport QA — story passe en `Done` |
