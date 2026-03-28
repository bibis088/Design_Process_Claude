---
name: write-qa-report
description: "Produit un rapport de test QA structuré pour une US-### ou FEAT-###. Vérifie les critères d'acceptance, documente les bugs, teste l'accessibilité RAAM et émet un verdict. Exécuté par le qa-engineer."
argument-hint: "[epic-slug]/[us-slug ou feat-slug]"
disable-model-invocation: false
context: fork
agent: qa-engineer
---

## Rôle
Produire un rapport de test QA complet et structuré depuis les specs fonctionnelles et design d'une story ou feature.

## Agents consommateurs
- QA Engineer (pilote)
- Product Owner (destinataire du verdict)

## Prérequis
- [ ] User story (`US-###`) ou feature ticket (`FEAT-###`) en statut `Dev Ready`
- [ ] Critères d'acceptance (`CA-##`) disponibles dans `./specs/$ARGUMENTS/stories/`
- [ ] Spec d'écran (`S-XX-[nom].md`) disponible dans `./design/[feature-slug]/ux/screens/`
- [ ] Spec accessibilité RAAM disponible dans `./design/[feature-slug]/ux/accessibility-spec.md`
- [ ] URLs Figma référencées dans les specs d'écran

## Gestion des erreurs

Si les prérequis ne sont pas remplis :
> ❌ Story ou feature non prête — vérifier le statut (`Dev Ready` requis) et la disponibilité des specs avant de lancer les tests.

Si un fichier source est introuvable via chemin relatif, utiliser Glob avec le pattern `**/specs/$ARGUMENTS/**` pour localiser les fichiers depuis leur chemin absolu.

## Processus de génération

### Étape 1 — Lire les specs
1. Lire tous les `CA-##` de la `US-###` ou `FEAT-###`
2. Lire la spec d'écran `S-XX-[nom].md` correspondante
3. Consulter la spec accessibilité RAAM de la feature
4. Vérifier les URLs Figma pour comparaison visuelle

### Étape 2 — Définir le plan de test
Pour chaque `CA-##`, définir un cas de test avec :
- Type : Fonctionnel / Accessibilité / Performance / Régression
- Plateforme : iOS / Android / Les deux
- Priorité : Haute / Moyenne / Basse

### Étape 3 — Exécuter et documenter

```markdown
# Rapport QA — [US-### ou FEAT-###] — [YYYY-MM-DD]

## Métadonnées
- Feature : [FEAT-###]
- Stories testées : [US-###, US-###]
- Plateforme : [iOS / Android / Les deux]
- Version testée : [numéro de build]
- Référence Figma : [URL frame principal]
- Statut global : [Pass / Fail / Partiel]
- QA Engineer : [nom]

## Plan de test

| ID | Cas de test | Type | CA couvert | Plateforme | Priorité |
|----|------------|------|-----------|-----------|---------|
| TC-01 | [Description] | Fonctionnel | CA-01 | iOS + Android | Haute |
| TC-02 | [Description] | Accessibilité | ACA-01 | iOS + Android | Haute |
| TC-03 | [Description] | Performance | CA-02 | Les deux | Moyenne |

## Résultats par critère d'acceptance

| CA | Énoncé | iOS | Android | Notes |
|----|--------|-----|---------|-------|
| CA-01 | [Énoncé] | ✅ / ❌ / ⚠️ | ✅ / ❌ / ⚠️ | [Observation] |
| CA-02 | [Énoncé] | ✅ / ❌ / ⚠️ | ✅ / ❌ / ⚠️ | [Observation] |
| ACA-01 | Navigation VoiceOver/TalkBack sans blocage | ✅ / ❌ / ⚠️ | ✅ / ❌ / ⚠️ | [Observation] |

## Conformité RAAM

### Niveau A — obligatoire
| Critère | Description | iOS | Android | Notes |
|---------|-------------|-----|---------|-------|
| RAAM 6.1 | Labels boutons | ✅ / ❌ | ✅ / ❌ | [Observation] |
| RAAM 7.1 | Ordre de focus | ✅ / ❌ | ✅ / ❌ | [Observation] |
| RAAM 13.1 | Zones tactiles ≥ 44x44pt / 48x48dp | ✅ / ❌ | ✅ / ❌ | [Observation] |

### Niveau AA — cible
| Critère | Description | iOS | Android | Notes |
|---------|-------------|-----|---------|-------|
| RAAM 2.2 | Contraste texte ≥ 4.5:1 | ✅ / ❌ | ✅ / ❌ | [Ratio mesuré] |
| RAAM 10.2 | Lisible à 200% | ✅ / ❌ | ✅ / ❌ | [Observation] |

## Vérification visuelle Figma
| Écran | URL Figma | Conforme au livré | Écarts notés |
|-------|----------|------------------|-------------|
| S-01 | [URL] | ✅ / ❌ | [Description écart si applicable] |

## Bugs identifiés

| ID | Sévérité | Plateforme | RAAM | Description | Étapes de reproduction | CA impacté |
|----|----------|-----------|------|-------------|----------------------|-----------|
| BUG-001 | [Critique / Majeur / Mineur / Cosmétique] | [iOS / Android] | [RAAM ## si applicable] | [Description] | 1.[...] 2.[...] | CA-## |

## Performance
| Métrique | Cible | iOS | Android |
|----------|-------|-----|---------|
| Chargement liste | < 2s | [valeur] | [valeur] |
| Feedback action | < 100ms | [valeur] | [valeur] |
| Comportement offline | Graceful | [description] | [description] |

## Verdict

- [ ] ✅ Approuvé — story peut passer en `Done`
- [ ] ❌ Refusé — [N] bugs Critique/Majeur à corriger
- [ ] ⚠️ Approuvé avec réserves — bugs Mineurs en V+1

## Actions requises (si Refusé ou Réserves)

| BUG | Action | Responsable | Deadline |
|-----|--------|-------------|---------|
| BUG-001 | [Correction] | Dev iOS / Android | [YYYY-MM-DD] |
```

## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. Tous les `CA-##` ont-ils été testés sur iOS ET Android — aucun oublié ? (oui / non + CA manquants)
2. La conformité RAAM niveau A est-elle complète — tous les critères applicables sont cochés ? (oui / non + critères manquants)
3. Chaque bug a-t-il une sévérité, des étapes de reproduction et un CA impacté documentés ? (oui / non + bugs incomplets)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-qa-report "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/qa/QA-###-[slug].md

Prochaines étapes :
→ Notifier le PO pour validation finale
→ /review-dod $ARGUMENTS (si verdict Approuvé)
→ Retour Dev si bugs Critique/Majeur
```
