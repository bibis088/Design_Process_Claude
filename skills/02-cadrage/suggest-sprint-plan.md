---
name: suggest-sprint-plan
description: "Optionnel — propose une découpe en sprints de design à partir du cadrage validé. Analyse les US, leur complexité et les dépendances pour suggérer une répartition en sprints cohérente. Le Product Designer valide ou ajuste avant de l'intégrer dans le cadrage. Use when user says 'suggère les sprints', 'répartition en sprints', 'découpe sprint', 'planifie les sprints', or 'suggest-sprint-plan'."
argument-hint: "[epic-slug]"
agent: product-owner
---

## Rôle
Proposer une répartition des features et user stories en sprints de design à partir du cadrage, en tenant compte des dépendances, de la complexité et des contraintes de calendrier. Ce skill est **optionnel** — il aide le Product Designer à définir les sprints avant de valider le cadrage (critère 14 de la DoD).

## Quand l'utiliser

Déclencher ce skill si la répartition en sprints n'est pas encore définie au moment de valider le cadrage :
- En fin de `write-cadrage` quand la question Q11 (contenu des sprints) est incomplète
- Quand le cadrage est validé mais que la répartition sprint mérite d'être optimisée
- Quand le périmètre évolue et que les sprints doivent être recalculés

## Prérequis
- [ ] `write-cadrage` exécuté — `specs/[epic-slug]/cadrage.md` disponible
- [ ] `write-user-story` exécuté (ou en cours) — US disponibles avec estimation et priorité MoSCoW
- [ ] Retro planning disponible dans `cadrage.md` (Q11 — dates clés et deadline V1)
- [ ] Durée d'un sprint définie dans `cadrage.md` (ex : 2 semaines)

---

## Étape 1 — Lire les inputs depuis le cadrage

Depuis `specs/[epic-slug]/cadrage.md` extraire :
```
Deadline V1              : [Date]
Durée d'un sprint        : [N semaines]
Nombre de sprints dispos : [calculé = (deadline - today) / durée sprint]
Capacité estimée / sprint: [UX N jours · UI N jours]
Priorités MoSCoW         : Must / Should / Could / Won't
Estimations par US       : UX [Nj] · UI [Nj]
```

---

## Étape 2 — Analyser les dépendances entre US

Construire le graphe de dépendances :

```markdown
## Graphe de dépendances

| US | Titre | Bloquée par | Bloque |
|----|-------|-------------|--------|
| US_001 | Authentification | — | US_002, US_003, US_010 |
| US_002 | Dashboard | US_001 | US_004 |
| US_003 | Profil utilisateur | US_001 | — |
```

Identifier :
- **US fondatrices** : sans dépendance amont → candidats naturels au Sprint 1
- **US bloquantes** : bloquent le plus d'autres US → à prioriser
- **US indépendantes** : peuvent être décalées sans impact

---

## Étape 3 — Calculer la capacité par sprint

```markdown
## Capacité design par sprint

Durée sprint : [N semaines]
Jours UX disponibles / sprint : [N jours]
Jours UI disponibles / sprint  : [N jours]

Capacité totale :
  Sprint 1 → UX [Nj] · UI [Nj]
  Sprint 2 → UX [Nj] · UI [Nj]
  ...
```

Appliquer un facteur de réalisme : **−20% pour les aléas** (réunions, retours client, ajustements).

---

## Étape 4 — Proposer la répartition

Construire le plan de sprints selon ces règles de priorité :
1. **Must** avant **Should** avant **Could**
2. **US fondatrices** et **US bloquantes** en premier
3. Respecter les dépendances — ne pas mettre une US avant celles qu'elle nécessite
4. Équilibrer la charge UX et UI — ne pas surcharger un sprint
5. Garder une marge sur le dernier sprint pour les ajustements et la revue finale

```markdown
## Proposition de répartition — [NomProjet]

**Hypothèses :**
- Durée sprint : [N semaines]
- Nombre de sprints : [N]
- Deadline V1 : [Date]
- Facteur réalisme : −20%

---

### Sprint 1 — [Thème] — du [Date] au [Date]

| US | Titre | Priorité | UX | UI | Total | Dépendances |
|----|-------|----------|----|----|-------|-------------|
| US_001 | [Titre] | Must | [Nj] | [Nj] | [Nj] | — |
| US_002 | [Titre] | Must | [Nj] | [Nj] | [Nj] | US_001 |

**Charge sprint 1 :** UX [Nj]/[capacité] · UI [Nj]/[capacité]
**DoD sprint 1 :**
- [ ] Maquettes Figma US_001 et US_002 validées par le Product Designer
- [ ] Prototype navigation Sprint 1 disponible
- [ ] Revue client planifiée le [Date]

---

### Sprint 2 — [Thème] — du [Date] au [Date]

[même structure]

---

### Récapitulatif

| Sprint | US couvertes | Charge UX | Charge UI | Revue client |
|--------|-------------|-----------|-----------|-------------|
| Sprint 1 | US_001, US_002 | [Nj] | [Nj] | [Date] |
| Sprint 2 | US_003, US_004 | [Nj] | [Nj] | [Date] |
| **Total** | [N] US | [Nj] | [Nj] | — |

### US reportées en V2 (Could / Won't ou capacité insuffisante)
| US | Titre | Raison du report |
|----|-------|-----------------|
| US_### | [Titre] | [Capacité insuffisante / Dépendance V2 / Could] |
```

---

## Étape 5 — Présenter les options

Si des arbitrages sont nécessaires (trop d'US Must pour la capacité disponible), présenter **2 options** :

```markdown
## ⚠️ Arbitrage nécessaire

La capacité totale ([N jours]) est insuffisante pour couvrir toutes les US Must ([N jours estimés]).

Option A — Scope réduit
  → Reporter [US_###, US_###] en V2
  → Livraison V1 dans les délais
  → Risque : fonctionnalités manquantes

Option B — Sprint supplémentaire
  → Ajouter 1 sprint → deadline V1 repoussée de [N semaines]
  → Scope complet
  → Risque : dépassement planning

Quelle option préférez-vous ?
```

---

## Étape 6 — Intégrer dans le cadrage

Après validation du Product Designer, mettre à jour `specs/[epic-slug]/cadrage.md` section "Planification des sprints" (Q11) avec la répartition validée.

---

## Phase de validation

1. Toutes les US Must sont-elles couvertes dans au moins un sprint — ou explicitement reportées avec justification ? (oui / non)
2. Les dépendances sont-elles respectées — aucune US placée avant une US dont elle dépend ? (oui / non + violations)
3. La charge UX + UI par sprint est-elle ≤ la capacité moins 20% ? (oui / non + sprints surchargés)
4. Chaque sprint a-t-il une DoD et une date de revue client ? (oui / non)
5. Le Product Designer a-t-il validé la répartition ? (oui / non — **bloquant** pour la Gate 0)

## Résumé de fin d'exécution

```
✅ suggest-sprint-plan "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/cadrage.md — section Q11 mise à jour

[N] sprints · [N] US réparties · [N] US reportées V2

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]

Prochaine étape :
→ Valider la répartition (critère 14 DoD cadrage)
→ Gate 0 — Product Designer valide le cadrage complet
```
