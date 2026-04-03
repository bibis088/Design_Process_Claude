---
name: write-user-story
description: "Produit les feature tickets et les user stories avec critères d'acceptance depuis le brief fonctionnel. Génération incrémentale (1 US à la fois), cross-check persona, découpage automatique si trop complexe. Use when user says 'user stories', 'features', 'US', 'stories', 'tickets', 'critères d acceptance', or 'write-user-story'."
argument-hint: "[epic-slug]"
agent: product-owner
---

## Rôle
Produire les features et user stories depuis le brief fonctionnel validé.
Gate de sortie : Gate 2.

## Règles fondamentales — lire avant de produire quoi que ce soit

### 1. Neutralité visuelle — STRICTEMENT OBLIGATOIRE
Les US et CA ne doivent **jamais** mentionner :
- Des composants UI : bouton, carousel, tab bar, bottom sheet, drawer, modal, card...
- Des gestes : swipe, pinch, long press, tap...
- Des éléments visuels : couleur, icône, taille de texte, layout...
- Des patterns d'implémentation : API, endpoint, base de données...

**L'US décrit l'intention utilisateur et la règle métier — pas l'interface.**

| ❌ Interdit | ✅ Correct |
|------------|-----------|
| "L'utilisateur swipe pour supprimer" | "L'utilisateur peut supprimer un élément" |
| "Un bouton Valider s'affiche" | "L'utilisateur peut confirmer son action" |
| "Le carousel affiche les résultats" | "Les résultats sont présentés à l'utilisateur" |
| "La modal de confirmation s'ouvre" | "L'utilisateur est invité à confirmer avant suppression" |

### 2. Génération incrémentale — 1 US à la fois
Ne jamais produire toutes les US d'un coup.
Après chaque US :
1. Afficher la story complète
2. Demander : "Valider cette US avant de passer à la suivante ?"
3. Attendre la confirmation explicite avant de continuer
4. Si correction demandée → corriger et reproposer avant de passer à la suivante

### 3. Découpage automatique — seuil de complexité
Si une US atteint l'un des seuils suivants :
- Plus de **5 critères d'acceptance** dans le happy path
- Plus de **3 flux alternatifs**
- Estimation **L ou XL**
- Couvre **plus de 2 intentions utilisateur distinctes**

→ Stopper la rédaction et proposer le découpage avant de continuer :

```
⚠️ Cette US dépasse le seuil de complexité.
Je suggère de la scinder en [N] unités distinctes :

  US-###a — [Titre intention 1]
  US-###b — [Titre intention 2]

Confirmer le découpage ou continuer avec l'US unique ?
```

### 4. Cross-check persona — avant chaque US
Avant de rédiger chaque US, vérifier explicitement :

```
Cross-check persona :
→ Persona concerné : [PERSONA-### — Nom]
→ Cette action est-elle cohérente avec son profil (rôle, contexte, maturité tech) ?
→ Insight utilisateur associé : [depuis insights.md si disponible]
```

Si l'action ne correspond pas au profil du persona → signaler et demander confirmation avant de continuer.

---

## Prérequis
- [ ] Gate 1 validée — `EPIC.md` + `FLUX-###` + règles + glossaire Approved
- [ ] `EPIC.md` disponible dans `specs/[epic-slug]/`
- [ ] `PERSONA-###` × 3 disponibles — chaque US doit référencer au moins un persona
- [ ] `insights.md` disponible (cross-check persona depuis données terrain)

---

## Étape 1 — Feature Tickets

### 1a — Délimiter la feature
Une feature regroupe un ensemble cohérent de user stories qui livrent une valeur complète.
- Pas trop large : livrable en 1 sprint maximum
- Pas trop étroite : minimum 2 stories

### 1b — Rédiger le ticket

```markdown
# [FEAT-###] Titre de la feature

## Métadonnées
- Epic : [EPIC-###]
- Product Owner : [nom]
- Priorité MoSCoW : [Must / Should / Could / Won't]
- Statut : Draft
- Date : [YYYY-MM-DD]

## Résumé
[2-3 phrases orientées utilisateur — valeur produit + problème résolu.
Aucune référence à l'interface ou à l'implémentation technique.]

## Objectifs
| # | Objectif | KPI | Cible | Délai |
|---|----------|-----|-------|-------|
| 1 | [Objectif mesurable] | [Métrique] | [Valeur] | [Sprint/Date] |

## Personas concernés
- [PERSONA-###] — [Nom] — [Pourquoi cette feature le concerne]

## Périmètre fonctionnel
### Inclus
- [Fonctionnalité 1] — [US-###]
### Hors périmètre (explicitement exclu)
- [Ce qui ne sera PAS fait — et pourquoi]

## Flux principal
1. [Acteur] [action]
2. [Système] [réponse]

## Gestion des erreurs globale
| Condition | Message présenté à l'utilisateur | Action corrective |
|-----------|----------------------------------|------------------|
| [Ex : perte réseau] | [Ex : "Connexion perdue. Vos données sont sauvegardées."] | [Ex : Réessayer automatiquement toutes les 30s] |

## KPIs de succès
| Métrique | Baseline | Cible | Délai |
|---------|----------|-------|-------|
| [Métrique] | [Valeur actuelle] | [Cible] | [J+30/J+90] |

## Dépendances
- Bloqué par : [FEAT-### ou "aucune"]
- Bloque : [FEAT-### ou "aucune"]

## Risques
| Risque | Probabilité | Impact | Mitigation |
|--------|------------|--------|-----------|
| [Risque] | [H/M/B] | [H/M/B] | [Action préventive] |
```

---

## Étape 2 — User Stories

### 2a — Ordre de production
Produire les US dans l'ordre du flux fonctionnel (FLUX-###) :
1. Happy path d'abord
2. Flux alternatifs ensuite
3. Cas d'erreur en dernier

**Produire 1 US à la fois — attendre validation avant la suivante.**

### 2b — Template US

```markdown
# [US-###] Titre orienté intention utilisateur

## Métadonnées
- Epic : [EPIC-###]
- Feature : [FEAT-###]
- Persona : [PERSONA-###]
- Flux source : [FLUX-###]
- Priorité MoSCoW : [Must / Should / Could / Won't]
- Estimation : [XS / S / M / L / XL]
- Statut : Draft

## Cross-check persona
- Profil : [Rôle · Contexte d'usage · Maturité technologique]
- Cohérence : [Pourquoi cette action est naturelle pour ce persona]
- Insight terrain : [Citation ou donnée depuis insights.md — si disponible]

## Contexte
[1-2 phrases — POURQUOI cette story existe et sa valeur métier.
Aucune référence à l'interface ou à la technique.]

## User Story
**En tant que** [PERSONA-### — rôle],
**Je veux** [intention précise — verbe d'action métier],
**Afin de** [bénéfice mesurable].

## Critères d'acceptance

### Flux heureux (happy path)
- [ ] CA-01 : [Condition observable — intention + résultat attendu. Zéro UI.]
- [ ] CA-02 : [Condition observable]

### Flux alternatifs
- [ ] CA-03 : Si [condition alternative], alors [comportement attendu]

### Gestion des erreurs — obligatoire
Pour chaque cas d'erreur, définir les 3 éléments suivants :

| # | Condition de déclenchement | Message présenté à l'utilisateur | Action corrective |
|---|---------------------------|----------------------------------|------------------|
| CA-04 | [Ex : identifiants incorrects] | [Ex : "Identifiant ou mot de passe incorrect."] | [Ex : Proposer réinitialisation mot de passe] |
| CA-05 | [Ex : perte de connexion] | [Ex : "Connexion impossible. Vérifiez votre réseau."] | [Ex : Réessayer — données mises en cache] |
| CA-06 | [Ex : permission refusée] | [Ex : "Accès refusé. Contactez votre administrateur."] | [Ex : Rediriger vers l'écran d'accueil] |

## Accessibilité — section obligatoire
| Critère | Exigence | Référence |
|---------|---------|-----------|
| Contraste | Ratio minimum 4.5:1 pour le texte courant | RAAM 2.2 |
| Contraste grand texte | Ratio minimum 3:1 (texte ≥ 18pt / 14pt gras) | RAAM 2.2 |
| Labels ARIA | Chaque élément interactif a un label textuel accessible | RAAM 6.1 |
| Navigation clavier | Parcours logique de haut en bas, gauche à droite | RAAM 7.1 |
| Mode paysage | Contenu accessible et utilisable en orientation landscape | RAAM 13.2 |
| Texte agrandi | Contenu lisible jusqu'à 200% de zoom sans perte de contenu | RAAM 4.2 |
| Annonces dynamiques | Tout changement d'état est annoncé à VoiceOver/TalkBack | RAAM 6.2 |

## Advisory — recommandations
> Points d'attention identifiés pendant la rédaction.
> Ces éléments ne bloquent pas l'US mais méritent d'être discutés.

- [Ex : La gestion du timeout n'est pas définie dans FLUX-###. Suggérer une valeur par défaut de 30s ?]
- [Ex : Ce cas d'usage est similaire à US-### — vérifier si une règle métier commune peut être factorisée.]

## Règles métier applicables
- [RB-###] : [Intitulé de la règle — copié depuis regles-metier.md]

## Dépendances
- Bloqué par : [US-### ou "aucune"]
- Bloque : [US-### ou "aucune"]

## Estimation
- Taille : [XS / S / M / L / XL]
- Points Scrum : [1 / 2 / 3-5 / 8 / 13+]
- Justification : [1 phrase sur la complexité]

## Definition of Done
- [ ] CA validés par le PO
- [ ] Comportement défini sur iOS ET Android
- [ ] Section accessibilité complète (contrastes + labels + paysage)
- [ ] Gestion des erreurs documentée (condition + message + action)
- [ ] Cross-check persona validé
- [ ] Dépendances documentées
```

### 2c — Vérification INVEST après chaque US

| Critère | Question |
|---------|---------|
| **I**ndépendante | Livrable sans dépendre d'une autre non terminée ? |
| **N**égociable | Les détails d'implémentation sont ouverts ? |
| **V**aluable | Valeur visible pour l'utilisateur ou le métier ? |
| **E**stimable | Chiffrable avec les infos disponibles ? |
| **S**mall | Livrable en 1 sprint maximum ? |
| **T**estable | CA vérifiables sans ambiguïté ? |

Si un critère échoue → retravailler avant de soumettre.

### 2d — Message après chaque US

```
✅ US-### "[Titre]" rédigée.

Cross-check :
→ Persona : [PERSONA-### — Nom] — cohérence [✅ / ⚠️ à vérifier]
→ Erreurs : [N] cas définis avec condition + message + action corrective
→ Accessibilité : section complète [✅ / ⚠️ manquants : ...]
→ Complexité : [dans les seuils / ⚠️ suggère découpage]

Valider cette US pour passer à la suivante ?
US suivante prévue : US-### "[Titre estimé depuis FLUX-###]"
```

---


---

## Règles CA par type d'élément fonctionnel

Quand une US implique l'un des éléments ci-dessous, les critères d'acceptance correspondants sont **obligatoires**. L'agent les ajoute automatiquement au template de l'US, sans attendre qu'ils soient demandés.

---

### 📝 Champs de saisie

Pour chaque champ impliqué dans l'US, définir :

| Attribut | Question obligatoire | Exemple de CA |
|----------|---------------------|---------------|
| **Obligatoire / Non obligatoire** | Ce champ doit-il être rempli pour continuer ? | "L'utilisateur ne peut pas soumettre sans renseigner [champ]" / "L'utilisateur peut soumettre sans renseigner [champ]" |
| **Format attendu** | Quel format est accepté ? | "L'utilisateur est informé si l'adresse email ne respecte pas le format standard" |
| **Longueur** | Min et max de caractères ? | "L'utilisateur ne peut pas saisir plus de [N] caractères dans [champ]" |
| **Données sensibles** | La valeur doit-elle être masquée ? | "La valeur saisie dans [champ mot de passe] n'est pas visible par défaut" |
| **Copier/coller** | Autorisé ou non ? | "L'utilisateur peut coller une valeur dans [champ]" / "Le collage est désactivé dans [champ code OTP]" |
| **Autocomplétion** | Autorisée ou non ? | "L'autocomplétion système est activée pour [champ email]" |
| **Soumission invalide** | Que se passe-t-il si le champ obligatoire est vide ? | → voir tableau Gestion des erreurs |

**Cas d'erreur obligatoires pour chaque champ obligatoire :**

| Condition de déclenchement | Message présenté à l'utilisateur | Action corrective |
|---------------------------|----------------------------------|------------------|
| Champ obligatoire vide à la soumission | "[Libellé du champ] est requis." | Bloquer la soumission et indiquer le champ concerné |
| Format invalide | "Le format de [champ] n'est pas valide." | Bloquer la soumission et indiquer le champ concerné |
| Valeur hors limites (longueur, plage) | "[Champ] doit contenir entre [N] et [M] caractères." | Bloquer ou tronquer selon la règle métier |

---

### 📋 Listes et résultats

| Cas | CA obligatoire |
|-----|---------------|
| **État vide** | "Si aucun résultat n'est disponible, l'utilisateur en est informé" |
| **État chargement** | "Pendant la récupération des données, l'utilisateur est informé que le contenu se charge" |
| **Nombre d'éléments** | "La liste affiche [N] éléments par défaut — l'utilisateur peut en afficher davantage" |
| **Tri par défaut** | "Les éléments sont présentés par [critère] par défaut" |
| **Actualisation** | "L'utilisateur peut actualiser la liste manuellement" / "La liste se rafraîchit automatiquement toutes les [N] secondes" |

---

### ⚠️ Actions irréversibles

Toute action de suppression, transfert, envoi définitif ou modification non annulable déclenche ces CA obligatoires :

| CA | Libellé |
|----|---------|
| Confirmation | "L'utilisateur doit confirmer son intention avant que l'action soit exécutée" |
| Conséquence claire | "L'utilisateur est informé de la conséquence de l'action avant de confirmer" |
| Annulation possible | "L'utilisateur peut annuler avant confirmation" |
| Annulation post-exécution | "L'utilisateur dispose de [N secondes / N jours] pour annuler après exécution" — ou "L'action est irréversible immédiatement" |

---

### 🔐 Authentification et sessions

| Cas | CA obligatoire |
|-----|---------------|
| **Expiration de session** | "Si la session expire, l'utilisateur est invité à se reconnecter sans perdre ses données en cours" |
| **Tentatives échouées** | "Après [N] tentatives échouées, l'accès est temporairement suspendu pendant [durée]" |
| **Blocage** | "L'utilisateur est informé de la durée de suspension et de la procédure de récupération" |
| **Persistance** | "La session est maintenue entre deux ouvertures de l'application" / "L'utilisateur doit se reconnecter à chaque ouverture" |

---

### 📱 Permissions système

| Cas | CA obligatoire |
|-----|---------------|
| **Permission refusée** | "Si l'utilisateur refuse la permission [X], la fonctionnalité [Y] est [désactivée / dégradée — préciser]" |
| **Invitation** | "Si la permission est nécessaire, l'utilisateur est invité à l'activer depuis les réglages de son appareil" |
| **Révocation** | "Si l'utilisateur révoque la permission après l'avoir accordée, la fonctionnalité est désactivée sans planter l'application" |
| **Usage partiel** | "Les fonctionnalités ne nécessitant pas la permission restent accessibles" |

---

### 🖼️ Contenu média (images, vidéos, fichiers)

| Attribut | CA obligatoire |
|----------|---------------|
| **Formats acceptés** | "L'utilisateur est informé si le format sélectionné n'est pas pris en charge" |
| **Taille maximale** | "L'utilisateur est informé si le fichier dépasse [N] Mo" |
| **Résolution minimale** | "L'utilisateur est informé si la résolution est insuffisante" (si applicable) |
| **Échec d'envoi** | → voir tableau Gestion des erreurs |

---

### 🔄 Données temps réel et synchronisation

| Cas | CA obligatoire |
|-----|---------------|
| **Mode hors-ligne** | "En l'absence de connexion, l'utilisateur peut [accéder aux données en cache / est informé que les données ne sont pas disponibles]" |
| **Reconnexion** | "À la reconnexion, les données sont synchronisées [automatiquement / sur action de l'utilisateur]" |
| **Conflit** | "En cas de modification simultanée, [la version la plus récente est conservée / l'utilisateur choisit quelle version conserver]" |

---

### 🔙 Navigation et états d'écran

| Cas | CA obligatoire |
|-----|---------------|
| **Retour arrière** | "Si l'utilisateur quitte sans sauvegarder, il est averti de la perte potentielle de données" |
| **Reprise** | "À la réouverture, l'utilisateur retrouve [l'état où il s'est arrêté / le début du parcours]" |
| **Notification entrant** | "Si l'utilisateur accède à l'écran depuis une notification, il atterrit sur [contexte pertinent]" |
| **Deep link invalide** | "Si le lien est invalide ou expiré, l'utilisateur est redirigé vers [écran de repli] avec une explication" |

---

### 🔍 Recherche et filtres

| Cas | CA obligatoire |
|-----|---------------|
| **Déclenchement** | "La recherche se déclenche après [N] caractères saisis" |
| **Aucun résultat** | "Si la recherche ne retourne aucun résultat, l'utilisateur en est informé et peut modifier sa recherche" |
| **Persistance des filtres** | "Les filtres sont [conservés / réinitialisés] entre les sessions" |

---

### 🔔 Notifications

| Attribut | CA obligatoire |
|----------|---------------|
| **Déclencheur** | "La notification est envoyée quand [événement précis]" |
| **Permission refusée** | "Si l'utilisateur a refusé les notifications, [fonctionnement alternatif — ex: badge, indicateur in-app]" |
| **Action au tap** | "Taper la notification amène l'utilisateur à [contexte précis]" |
| **Expiration** | "La notification est pertinente pendant [durée] — au-delà, elle n'est plus affichée / marquée comme expirée" |

---

### 💳 Paiement et données financières

| CA | Libellé obligatoire |
|----|---------------------|
| Récapitulatif | "L'utilisateur voit un récapitulatif complet avant de confirmer la transaction" |
| Échec paiement | → voir tableau Gestion des erreurs |
| Confirmation | "L'utilisateur reçoit une confirmation de transaction [dans l'application / par email]" |
| Données sensibles | "Aucune donnée financière sensible n'est conservée sur l'appareil" |

---

### 🔒 Données personnelles (RGPD)

| CA | Libellé obligatoire |
|----|---------------------|
| Consentement | "L'utilisateur donne son accord explicite avant toute collecte de données personnelles" |
| Finalité | "L'utilisateur est informé de la raison pour laquelle ses données sont collectées" |
| Retrait | "L'utilisateur peut retirer son consentement à tout moment" |
| Suppression | "L'utilisateur peut demander la suppression de ses données" |
| Durée | "La durée de conservation des données est [N mois/ans] — définie dans la politique de confidentialité" |

---

## Détection automatique des cas particuliers

Quand l'agent rédige une US, il doit détecter automatiquement quels types d'éléments sont impliqués et appliquer les règles correspondantes sans attendre qu'elles soient demandées.

**Signal de détection dans le flux fonctionnel :**

| Mot-clé dans le FLUX-### | Types à activer |
|--------------------------|----------------|
| saisir, renseigner, entrer, formulaire | Champs de saisie |
| liste, résultats, afficher, parcourir | Listes et résultats |
| supprimer, envoyer définitivement, transférer | Actions irréversibles |
| connexion, authentification, session, mot de passe | Authentification |
| caméra, localisation, microphone, notification | Permissions système |
| photo, image, fichier, document, vidéo | Contenu média |
| synchroniser, temps réel, hors ligne | Synchronisation |
| rechercher, filtrer, trier | Recherche et filtres |
| notifier, alerter, rappeler | Notifications |
| payer, acheter, transaction | Paiement |
| données personnelles, RGPD, consentement | RGPD |


## Gestion des erreurs
> ❌ Référence UI détectée dans un CA — reformuler en termes d'intention utilisateur.
> ❌ Persona non cohérent — vérifier PERSONA-### avant de continuer.
> ⚠️ Seuil de complexité atteint — proposer le découpage avant de continuer.

## Règles de qualité

- **Zéro UI dans les CA** — toute référence à un composant ou geste est refusée
- **Gestion des erreurs toujours en 3 colonnes** : condition · message · action corrective
- **Section accessibilité obligatoire** dans chaque US — jamais vide
- **1 US à la fois** — jamais de batch
- **Cross-check persona** avant chaque US — pas d'hypothèse sur le profil

## Chemin de fichier
```
specs/[epic-slug]/features/FEAT-###-[slug].md
specs/[epic-slug]/stories/US-###-[slug].md
```

## Phase de validation — niveau approfondi

1. Aucune référence UI dans les CA — zéro mention de composant, geste ou layout ? (oui / non + lignes à corriger)
2. Chaque cas d'erreur a-t-il ses 3 éléments : condition + message utilisateur + action corrective ? (oui / non + cas incomplets)
3. La section accessibilité est-elle complète — contraste + labels + paysage + annonces ? (oui / non + manquants)
4. Le cross-check persona est-il validé — cohérence avec profil + insight terrain ? (oui / non)
5. Le seuil de complexité est-il respecté — ou le découpage a-t-il été proposé ? (oui / non)

## Résumé de fin d'exécution

```
✅ write-user-story "$ARGUMENTS" terminé
📁 specs/$ARGUMENTS/features/FEAT-###.md × N
📁 specs/$ARGUMENTS/stories/US-###.md × N

📊 Métriques d'exécution
   Tokens consommés : [N input + N output = N total]
   Temps de génération : [Xs]
   Coût estimé : [~$X.XX à $X/1M tokens]

Prochaines étapes :
→ Gate 2 — Product Designer valide features + US + CA
→ /setup-figma-init $ARGUMENTS
```
