---
name: setup-figma-permissions
description: "Configure les permissions de partage des fichiers Figma — accès équipe interne, client en lecture seule, lien prototype public. Exécuté par le design-system-manager."
argument-hint: "[epic-slug]"
agent: design-system-manager
---

## Rôle
Configurer les droits d'accès sur les fichiers Figma selon les intervenants — équipe de conception en édition, client en commentaire ou lecture seule, lien prototype pour les tests utilisateur.

## ⚠️ Action prohibée — délégation obligatoire
La modification des permissions Figma (partage, droits d'accès) est une **action prohibée** pour les agents. Ce skill produit la liste des actions à effectuer et les soumet au Product Designer pour exécution manuelle.

## Prérequis
- [ ] Fichiers Figma Projet et Design System créés
- [ ] Bibliothèque DS connectée (`setup-figma-library`)
- [ ] Liste des intervenants disponible depuis le cadrage

## Processus

### Étape 1 — Identifier les intervenants depuis le cadrage

Depuis `specs/$ARGUMENTS/cadrage.md`, extraire :
- Équipe interne (Product Designer, BA, PO, Dev)
- Intervenants client (PO client, direction, valideurs)
- Testeurs externes si tests utilisateur prévus

### Étape 2 — Définir la matrice de permissions

```markdown
## Matrice de permissions — [NomProjet]

### Fichier Design System
| Intervenant | Rôle | Accès recommandé |
|-------------|------|-----------------|
| Product Designer | Équipe interne | ✏️ Can edit |
| BA / PO | Équipe interne | 💬 Can comment |
| Dev iOS / Android | Équipe interne | 👁️ Can view |
| Client | Externe | ❌ Pas d'accès direct |

### Fichier Projet (maquettes)
| Intervenant | Rôle | Accès recommandé |
|-------------|------|-----------------|
| Product Designer | Équipe interne | ✏️ Can edit |
| BA / PO | Équipe interne | 💬 Can comment |
| Dev iOS / Android | Équipe interne | 👁️ Can view |
| Client — valideurs | Externe | 💬 Can comment (phases de validation uniquement) |
| Client — direction | Externe | 👁️ Can view |

### Prototype React (fichier HTML)
| Usage | Accès | Mode |
|-------|-------|------|
| Tests utilisateur | Lien direct fichier HTML | Ouvert — aucune auth |
| Partage client | Google Drive — lien avec accès | Lecture |
| Partage public | ❌ Non recommandé | — |

### Lien de partage Figma prototype natif (si utilisé)
| Usage | Mode | Expiration |
|-------|------|-----------|
| Présentation client | Lien Anyone with link — Can view | Après validation |
| Tests utilisateur | Lien Anyone with link — Can view | Durée des tests |
```

### Étape 3 — Produire les instructions pour le Product Designer

```markdown
## Actions de partage à effectuer manuellement

⚠️ Ces actions doivent être réalisées par le Product Designer dans Figma.
Les agents ne peuvent pas modifier les permissions.

### Fichier Design System
1. Ouvrir [URL Fichier DS]
2. Cliquer "Share" en haut à droite
3. Ajouter les membres selon la matrice ci-dessus
4. Ne pas partager avec le client

### Fichier Projet
1. Ouvrir [URL Fichier Projet]
2. Cliquer "Share" en haut à droite
3. Ajouter l'équipe interne
4. Pour le client : partager uniquement pendant les phases de validation
   (Gate 7, Gate 8, Gate 9) puis retirer l'accès après validation

### À faire lors du partage prototype client
1. Générer un lien "Anyone with link — Can view" sur Figma
   OU utiliser le lien du fichier HTML prototype React
2. Inclure ce lien dans `/write-client-deliverable prototype $ARGUMENTS`
3. NE PAS donner accès Can edit au client

### Règle de sécurité
- Jamais de Can edit pour les intervenants externes
- Retirer l'accès client après chaque cycle de validation
- Ne jamais partager le fichier DS avec le client
```

## Phase de validation — niveau standard

1. La matrice de permissions couvre-t-elle tous les intervenants identifiés au cadrage ? (oui / non + intervenants manquants)
2. Le client n'a-t-il accès qu'au fichier Projet (jamais au fichier DS) ? (oui / confirmé)
3. Le Product Designer a-t-il reçu les instructions d'actions manuelles ? (oui / non)

> Ce skill produit des instructions — l'exécution est manuelle par le Product Designer.

## Résumé de fin d'exécution

```
✅ setup-figma-permissions "$ARGUMENTS" terminé
📋 Matrice de permissions : specs/$ARGUMENTS/figma-permissions.md

⚠️ Action manuelle requise — Product Designer
"Appliquer les permissions selon la matrice produite"

Prochaines étapes :
→ /write-figma-userflow $ARGUMENTS
→ /setup-figma-prototype $ARGUMENTS (connexions prototype natif)
```
