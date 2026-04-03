---
name: write-discovery-interview
description: "Prépare et documente les interviews de découverte utilisateur — recueille les besoins larges au-delà de l'expérience mobile/digital pour alimenter la roadmap fonctionnelle au-delà du MVP. Exécuté par le ux-researcher en amont ou en parallèle du cadrage."
argument-hint: "[project-slug]"
agent: ux-researcher
---

## Rôle
Conduire des interviews de découverte ouvertes avec des utilisateurs potentiels — pas pour valider un design, mais pour comprendre les besoins profonds, les habitudes, les frustrations et les opportunités non adressées par le MVP. Les insights alimentent directement la roadmap fonctionnelle.

## Différence avec les autres interviews du process

| Skill | Objectif | Quand | Portée |
|-------|---------|-------|--------|
| `write-interview-guide` | Valider les hypothèses de personas et flux | Phase 4a | MVP défini |
| `write-usability-test` | Tester les maquettes UX | Phase 4a | Design défini |
| `write-user-test-scenarios` | Tester l'UI finale | Phase 8b | UI finalisée |
| `write-discovery-interview` | Découvrir les besoins larges | Avant ou pendant le cadrage | Aucune contrainte — ouvert |

Ce skill n'est pas lié à un design existant. Il explore ce que les utilisateurs font, pensent et veulent — indépendamment de ce qu'on a prévu de construire.

## Agents consommateurs
UX Researcher  · Product Designer  · Product Owner  · Business Analyst 

## Prérequis
- [ ] Secteur d'activité et cible utilisateur identifiés (même partiellement)
- [ ] Participants potentiels identifiés (pas nécessairement des utilisateurs actuels)
- [ ] Aucun design ni specs requis — c'est intentionnel

## Processus de génération

### Étape 1 — Définir l'angle d'exploration

Depuis le cadrage initial ou le brief partiel, identifier :

```markdown
## Angle d'exploration — [project-slug]

### Ce qu'on sait déjà
[Ce que le cadrage ou le client a exprimé comme besoin]

### Ce qu'on ne sait pas encore
- [Question ouverte 1 — comportement actuel des utilisateurs]
- [Question ouverte 2 — frustrations non adressées]
- [Question ouverte 3 — habitudes existantes à ne pas casser]

### Ce qu'on cherche dans ces interviews
- Besoins non exprimés au-delà du périmètre MVP
- Occasions manquées identifiées par les utilisateurs eux-mêmes
- Signaux faibles pour la roadmap V2, V3
- Vocabulaire réel des utilisateurs (pour le glossaire et les libellés)
```

### Étape 2 — Recruter les participants

Profils recherchés — plus larges que pour les tests de design :

```markdown
## Profils de découverte

### Profil A — Utilisateurs du domaine (pas forcément du produit)
[Personnes qui vivent le problème au quotidien]
Nombre : 5-7
Critères : [Pratiquent le domaine, pas nécessairement tech-savvy]

### Profil B — Experts / professionnels du secteur
[Personnes avec une vue systémique du domaine]
Nombre : 2-3
Critères : [Expérience terrain, vision du secteur]

### Profil C — Cas extrêmes / outliers
[Utilisateurs qui font autrement, qui ont trouvé des workarounds]
Nombre : 2-3
Critères : [Comportement atypique, solutions maison]
```

### Étape 3 — Guide d'interview de découverte

```markdown
# Guide d'interview de découverte — [project-slug] — [YYYY-MM-DD]

## Format
- Durée : 60-75 minutes
- Format : Conversationnel — pas de questions fermées
- Enregistrement : Oui avec consentement
- Outil : Visio ou présentiel selon disponibilité

## Introduction (5 min)
"Je travaille sur un projet dans le domaine [secteur]. Je ne vais pas vous
présenter de produit — je veux d'abord comprendre comment vous faites les
choses aujourd'hui, ce qui fonctionne et ce qui vous manque.
Il n'y a pas de bonne ou mauvaise réponse — votre expérience quotidienne
est ce qui m'intéresse."

---

## Bloc 1 — Contexte et habitudes actuelles (15-20 min)

Objectif : comprendre le quotidien sans biais

- "Décrivez-moi votre semaine type en lien avec [domaine]."
- "Comment gérez-vous [problème du domaine] aujourd'hui ?"
- "Quels outils, apps ou méthodes utilisez-vous ? Pourquoi ceux-là ?"
- "Qu'est-ce qui prend le plus de temps dans ce que vous faites ?"

⚠️ Ne pas mentionner de solution — écouter et noter les outils/méthodes évoqués spontanément.

---

## Bloc 2 — Frustrations et points de blocage (15-20 min)

Objectif : identifier les vrais pain points, pas ceux qu'on suppose

- "Qu'est-ce qui vous énerve le plus dans votre façon de faire actuelle ?"
- "Y a-t-il des choses que vous faites et qui vous semblent inutilement compliquées ?"
- "Vous est-il arrivé d'abandonner une tâche parce que c'était trop difficile ?"
- "Si vous pouviez changer une seule chose, ce serait quoi ?"
- "Qu'est-ce que vous faites 'à la main' parce qu'il n'existe rien pour vous aider ?"

---

## Bloc 3 — Besoins latents et opportunités (15-20 min)

Objectif : explorer l'espace au-delà du MVP

- "Dans un monde idéal, à quoi ressemblerait [domaine] pour vous ?"
- "Qu'est-ce qui vous aiderait le plus que personne ne propose encore ?"
- "Avez-vous essayé des outils qui n'ont pas fonctionné pour vous ? Pourquoi ?"
- "Est-ce que vous partagez cette difficulté avec d'autres personnes ?"
- "Si vous aviez un assistant magique pour [domaine], que lui demanderiez-vous ?"

---

## Bloc 4 — Rapport au digital et au mobile (10-15 min)

Objectif : comprendre les attentes et les freins digitaux spécifiques

- "Utilisez-vous votre téléphone pour [domaine] aujourd'hui ?"
- "Qu'est-ce qui vous ferait utiliser (ou pas) une app mobile pour ça ?"
- "Avez-vous déjà abandonné une app ? Pour quelle raison ?"
- "Qu'est-ce qu'une bonne app doit absolument avoir pour vous convaincre ?"

---

## Clôture (5 min)
- "Y a-t-il quelque chose que je n'ai pas demandé et qui vous semble important ?"
- "Connaissez-vous des personnes qui pourraient avoir une expérience très différente de la vôtre ?"
- Remercier, expliquer la suite

---

## Grille de capture en temps réel

| Bloc | Verbatim exact | Comportement/émotion | Signal fort | Tag roadmap |
|------|---------------|---------------------|------------|-------------|
| Habitudes | "[Citation]" | [Observation] | ⭐ si fort | V2/V3/Hors scope |
```

### Étape 4 — Synthèse et alimentation de la roadmap

```markdown
# Rapport Discovery Interviews — [project-slug] — [YYYY-MM-DD]

## Participants
[N] interviews — [N] heures de contenu
Profils : [A: N, B: N, C: N]

---

## Insights — Besoins actuels (MVP)
*Ce que le MVP doit absolument adresser*

| Besoin | Fréquence | Citations | Impact MVP |
|--------|-----------|-----------|-----------|
| [Besoin 1] | [N/N participants] | "[Citation]" | Critique |

---

## Insights — Opportunités V2+
*Besoins réels non adressés par le MVP — à intégrer en roadmap*

| Opportunité | Fréquence | Citations | Complexité estimée | Tag |
|------------|-----------|-----------|-------------------|-----|
| [Opportunité 1] | [N/N] | "[Citation]" | Faible/Moyen/Élevé | V2 |
| [Opportunité 2] | [N/N] | "[Citation]" | Moyen | V3 |

---

## Opportunités hors scope (signaux faibles)
*Besoins identifiés mais non prioritaires — à surveiller*

- [Signal 1] — [N] mention(s) — "[Citation]"

---

## Vocabulaire utilisateur
*Termes employés spontanément — à intégrer dans les libellés et le glossaire*

| Terme technique | Terme utilisateur | Fréquence |
|----------------|------------------|-----------|
| [Terme interne] | "[Terme naturel]" | [N/N] |

---

## Recommandations roadmap

### Intégrer au MVP (si faisable)
- [Élément à ajouter — justification]

### Planifier en V2 (quick wins post-MVP)
- [Feature V2 — effort estimé : Faible]
- [Feature V2 — effort estimé : Moyen]

### Planifier en V3+ (vision long terme)
- [Feature V3]

### Ne pas faire (anti-besoins identifiés)
- [Ce que les utilisateurs ne veulent surtout pas]
```

## Phase de validation — niveau approfondi

Avant de valider ce livrable, réponds à ces questions dans l'ordre :

1. Les insights MVP et V2+ sont-ils clairement séparés — aucun mélange entre ce qui est urgent et ce qui est aspirationnel ? (oui / non)
2. Chaque opportunité roadmap a-t-elle au moins 2 citations distinctes — pas de généralisation depuis 1 participant ? (oui / non + opportunités à étayer)
3. Le vocabulaire utilisateur a-t-il été transmis au BA pour intégration dans le glossaire ? (oui / non)
4. Le PO a-t-il été informé des opportunités V2+ pour arbitrage de la roadmap ? (oui / non — envoyer le rapport)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-discovery-interview "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/research/discovery-interviews.md

Prochaines étapes :
→ Transmettre au BA : vocabulaire utilisateur → glossaire
→ Transmettre au PO : opportunités V2+ → roadmap fonctionnelle
→ Les insights MVP enrichissent le cadrage en cours
→ /write-cadrage $ARGUMENTS (si pas encore démarré)

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```
