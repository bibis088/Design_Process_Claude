---
name: write-persona
description: "Génère les 3 personas obligatoires d'un projet en un seul appel — regular-user, new-user et edge-user — adaptés au contexte du projet depuis le cadrage. Exécuté par le business-analyst. Use when user says 'crée les personas', 'génère les personas', 'write-persona', 'personas du projet', or 'qui sont nos utilisateurs'."
argument-hint: "[epic-slug]"
agent: business-analyst
---

## Rôle
Générer les 3 personas obligatoires du projet en un seul appel, chacun adapté à son archétype depuis les réponses au cadrage.

## Agents consommateurs
Business Analyst  · Product Owner  · UX Designer 

## Usage dans le process
Les personas produits par ce skill sont utilisés à deux moments :
1. **Au cadrage** — pour orienter les flux fonctionnels et les règles métier
2. **Dans write-user-story** — cross-check automatique avant chaque US pour vérifier la cohérence

> Un persona mal défini entraîne des US incohérentes. Si les insights du research sont disponibles, les utiliser systématiquement pour ancrer les personas dans la réalité terrain.

## Prérequis
- [ ] Cadrage disponible `specs/$ARGUMENTS/cadrage.md` avec réponse à Q1 (Qui ?)
- [ ] Contexte d'usage connu (mobile, fréquence, environnement)
- [ ] `insights.md` disponible — personas basés sur données terrain, pas hypothèses
- [ ] `discovery-interviews.md` disponible

## Les 3 archétypes obligatoires

| Slug | Archétype | Ce qu'il représente |
|------|-----------|---------------------|
| `regular-user` | Utilisateur régulier | Cœur de cible — usage fréquent, workflows établis |
| `new-user` | Nouvel utilisateur | Découverte du produit — onboarding, barrières à l'entrée |
| `edge-user` | Utilisateur aux limites | Contraintes : accessibilité, connexion dégradée, usage extrême, senior ou expert |

## Processus de génération

### Étape 1 — Lire le cadrage
Depuis `specs/$ARGUMENTS/cadrage.md`, extraire :
- Q1 : Qui utilise la fonctionnalité ?
- Q2 : Quand et dans quel contexte ?
- Q4 : Quel problème est résolu ?

### Étape 2 — Générer les 3 personas

Produire les 3 fichiers dans l'ordre, chacun avec le template ci-dessous **adapté à son archétype** :

---

#### PERSONA-001 — regular-user

**Angle de construction :**
- Utilisateur qui connaît déjà le domaine — il a des habitudes établies
- Frustrations sur l'**efficacité et la fluidité** — il veut aller plus vite
- Fréquence d'usage : quotidien ou hebdomadaire
- Maturité tech : intermédiaire à avancé

---

#### PERSONA-002 — new-user

**Angle de construction :**
- Utilisateur qui découvre le produit — premier contact
- Frustrations sur la **complexité et le manque de guidage**
- Questions : "Par où je commence ? Est-ce que je fais bien ?"
- Fréquence d'usage : occasionnel au démarrage
- Maturité tech : débutant à intermédiaire
- Enjeux onboarding critiques pour ce persona

---

#### PERSONA-003 — edge-user

**Angle de construction :**
- Utilisateur aux limites du système — contraintes extrêmes
- Choisir l'angle le plus pertinent selon le projet :
  - **Accessibilité** : handicap visuel, moteur ou cognitif
  - **Technique** : connexion dégradée, vieux device, mode offline
  - **Démographique** : senior (65+) ou enfant selon le contexte
  - **Expert** : power user qui pousse les limites du produit
- Ce persona révèle les failles de robustesse et d'accessibilité
- Maturité tech : variable selon l'angle choisi

---

### Étape 3 — Template commun (x3)

Appliquer ce template pour chaque persona en adaptant le contenu à l'archétype :

```markdown
# [PERSONA-###] [Prénom] — [Archétype]

## Métadonnées
- Archétype : [regular-user / new-user / edge-user]
- Epic(s) associé(s) : [EPIC-###]
- Statut : Draft
- Date de création : [YYYY-MM-DD]

## Identité
- Prénom fictif : [Prénom neutre et mémorable]
- Rôle : [Rôle dans son quotidien]
- Âge approximatif : [ex: 34 ans]

## Contexte d'usage
| Dimension | Détail |
|-----------|--------|
| Quand ? | [Moment d'usage] |
| Device | [iOS / Android / Les deux] |
| Fréquence | [Quotidien / Hebdomadaire / Occasionnel] |
| Environnement | [En mouvement / Au bureau / Connexion instable...] |
| Contexte émotionnel | [Sous pression / Détendu / Pressé] |

## Maturité technologique
- Niveau : [Débutant / Intermédiaire / Avancé]
- Description : [1-2 phrases sur son aisance mobile]

## Objectifs
1. [Objectif principal]
2. [Objectif secondaire]

## Frustrations actuelles
- [Frustration principale — adaptée à l'archétype]
- [Frustration secondaire]

## Besoins principaux
| Besoin | Priorité |
|--------|---------|
| [Besoin 1] | Haute |
| [Besoin 2] | Moyenne |

## Citation représentative
> "[Phrase à la première personne — authentique, pas marketing]"

## Ce persona n'est PAS
- [Distinction avec PERSONA-001 si nécessaire]
- [Distinction avec PERSONA-002 si nécessaire]
```

### Étape 4 — Vérifier la cohérence des 3 personas

Après génération, vérifier :
- Les 3 personas couvrent des besoins et comportements distincts — pas de doublon
- La citation de chacun reflète bien son archétype
- L'edge-user est suffisamment différent pour apporter une valeur réelle au projet

## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. Les 3 personas sont-ils suffisamment distincts — aucun doublon de besoins ou comportements ? (oui / fusionner si non)
2. Chaque citation sonne-t-elle naturelle à la première personne ? (oui / reformuler)
3. L'edge-user est-il pertinent par rapport au contexte du projet — l'angle choisi est-il justifié ? (oui / non + ajuster l'angle)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-persona "$ARGUMENTS" terminé — 3 personas générés
📁 specs/$ARGUMENTS/personas/PERSONA-001-regular-user.md
📁 specs/$ARGUMENTS/personas/PERSONA-002-new-user.md
📁 specs/$ARGUMENTS/personas/PERSONA-003-edge-user.md

Prochaines étapes :
→ /write-brief-fonctionnel $ARGUMENTS
→ /review-dod PERSONA

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]
```
