---
name: write-prototype-react
description: "Génère un prototype React interactif depuis les frames Figma d'un flow complet — partageable via URL. Deux niveaux selon la phase : cliquable simple (après écrans principaux) ou interactif complet (après écrans secondaires). Exécuté par le ui-designer."
argument-hint: "[feature-slug]/[flow-slug]"
agent: ui-designer
---

## Rôle
Générer un prototype React web depuis les frames Figma d'un flow complet — pour valider l'expérience de manière interactive avec le client, sans attendre l'implémentation native.

## Niveaux de fidélité

### Niveau 1 — Cliquable simple
**Quand :** après les écrans principaux (Gate 7)
**Contenu :** navigation entre écrans, contenu réel, pas d'animations
**Usage :** validation rapide du flux avec le client

### Niveau 2 — Interactif complet
**Quand :** après les écrans secondaires (Gate 8)
**Contenu :** navigation + transitions + états dégradés (loading, empty, error) + micro-interactions
**Usage :** présentation finale, validation complète de l'expérience

## Agents consommateurs
UI Designer  · Product Designer 

## Prérequis
- [ ] Frames Figma du flow disponibles (écrans principaux au minimum)
- [ ] `get_design_context` accessible via MCP Figma
- [ ] Niveau de fidélité choisi (1 ou 2)

## Gestion des erreurs
> ⚠️ Utiliser `get_metadata` pour la structure puis `get_screenshot` pour le rendu visuel.

## Processus de génération

### Étape 1 — Lire les frames du flow via MCP Figma

```
get_metadata [URL page Screens] → identifier les frames du flow
get_design_context [URL frame 1] → contenu structuré (React + Tailwind)
get_design_context [URL frame 2]
[... pour chaque frame du flow]
get_variable_defs [URL] → tokens couleurs, typo, spacing
```

### Étape 2 — Extraire la structure de navigation

Depuis les specs d'écran et le user flow, construire le graphe de navigation :

```javascript
const flows = {
  "S-01-login": {
    next: { "tap_connexion": "S-02-dashboard", "tap_oubli": "S-03-reset" },
    prev: null
  },
  "S-02-dashboard": {
    next: { "tap_profil": "S-04-profile" },
    prev: "S-01-login"
  }
}
```

### Étape 3 — Générer le prototype React

**Structure du projet :**
```
prototype-[flow-slug]/
├── index.html          ← standalone, aucune dépendance serveur
└── (tout inline)
```

Le prototype est un **fichier HTML unique** avec React et Tailwind en CDN — zéro installation, partageable immédiatement.

**Template de base :**

```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
  <title>Prototype — [NomFlow] — [NomProjet]</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/react/18.2.0/umd/react.production.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/react-dom/18.2.0/umd/react-dom.production.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-standalone/7.23.2/babel.min.js"></script>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    /* Tokens du design system depuis get_variable_defs */
    :root {
      --color-primary: [valeur depuis tokens];
      --color-secondary: [valeur];
      --spacing-md: [valeur];
      /* ... */
    }
    /* Viewport mobile centré sur desktop */
    body { background: #f0f0f0; display: flex; justify-content: center; }
    .device-frame {
      width: 393px; /* iPhone 14 Pro */
      height: 852px;
      overflow: hidden;
      position: relative;
      background: white;
      box-shadow: 0 20px 60px rgba(0,0,0,0.3);
      border-radius: 48px;
      margin: 20px 0;
    }
    @media (max-width: 430px) {
      .device-frame { width: 100vw; height: 100vh; border-radius: 0; margin: 0; }
    }
  </style>
</head>
<body>
<div id="root"></div>
<script type="text/babel">
const { useState, useEffect } = React;

/* ── TOKENS ── */
const tokens = {
  colors: { /* depuis get_variable_defs */ },
  spacing: { /* depuis get_variable_defs */ }
};

/* ── NAVIGATION ── */
const flows = { /* graphe de navigation extrait des specs */ };

/* ── ÉCRANS ── */

// Niveau 1 : chaque écran = composant React fidèle au design Figma
// Niveau 2 : + états loading/empty/error + transitions CSS

const S01Login = ({ navigate }) => (
  <div className="flex flex-col h-full">
    {/* Contenu depuis get_design_context */}
    <button onClick={() => navigate('S-02-dashboard')}
      className="...">
      Se connecter
    </button>
  </div>
);

/* [Autres écrans du flow] */

/* ── TRANSITIONS (Niveau 2 uniquement) ── */
const transitions = {
  push: 'translateX(100%) → translateX(0)',
  pop: 'translateX(0) → translateX(100%)',
  modal: 'translateY(100%) → translateY(0)'
};

/* ── APP SHELL ── */
const App = () => {
  const [currentScreen, setCurrentScreen] = useState('S-01-login');
  const [history, setHistory] = useState([]);
  const [transition, setTransition] = useState(null);

  const navigate = (screenId, type = 'push') => {
    setTransition(type);
    setHistory(h => [...h, currentScreen]);
    setCurrentScreen(screenId);
  };

  const goBack = () => {
    if (history.length > 0) {
      setCurrentScreen(history[history.length - 1]);
      setHistory(h => h.slice(0, -1));
    }
  };

  const screens = {
    'S-01-login': <S01Login navigate={navigate} />,
    /* autres écrans */
  };

  return (
    <div className="device-frame">
      {/* Barre de progression du flow */}
      <div style={{height: 3, background: '#eee'}}>
        <div style={{
          height: '100%',
          width: `${(Object.keys(screens).indexOf(currentScreen) + 1) /
            Object.keys(screens).length * 100}%`,
          background: 'var(--color-primary)',
          transition: 'width 0.3s'
        }}/>
      </div>
      {screens[currentScreen]}
    </div>
  );
};

ReactDOM.render(<App />, document.getElementById('root'));
</script>
</body>
</html>
```

### Étape 4 — Niveau 2 : ajouter les états et micro-interactions

Si niveau 2 — ajouter pour chaque écran :

```javascript
// État loading
const S02Dashboard = ({ navigate }) => {
  const [loading, setLoading] = useState(true);
  useEffect(() => { setTimeout(() => setLoading(false), 1500); }, []);

  if (loading) return <LoadingState />;
  return <DashboardContent navigate={navigate} />;
};

// État empty
const EmptyState = ({ title, subtitle, cta, onCta }) => (
  <div className="flex flex-col items-center justify-center flex-1 p-8 text-center">
    <div className="text-4xl mb-4">📭</div>
    <h3 className="font-semibold text-lg mb-2">{title}</h3>
    <p className="text-gray-500 mb-6">{subtitle}</p>
    <button onClick={onCta} className="...">{cta}</button>
  </div>
);

// Transitions CSS
const ScreenTransition = ({ children, type }) => (
  <div style={{
    animation: type === 'push'
      ? 'slideIn 0.3s ease-out'
      : type === 'modal'
        ? 'slideUp 0.3s ease-out'
        : 'none'
  }}>
    {children}
  </div>
);
```

### Étape 5 — Bannière prototype

Toujours afficher une bannière non-intrusive en bas :

```jsx
const PrototypeBanner = () => (
  <div style={{
    position: 'fixed', bottom: 0, left: 0, right: 0,
    background: 'rgba(0,0,0,0.85)', color: 'white',
    padding: '8px 16px', fontSize: 12, textAlign: 'center',
    zIndex: 9999
  }}>
    🔗 Prototype — [NomProjet] · [Flow] · v[X.X] · {new Date().toLocaleDateString('fr')}
  </div>
);
```

### Étape 6 — Sauvegarder et documenter

Sauvegarder dans :
```
design/[feature-slug]/prototype/
├── prototype-[flow-slug]-v1-clickable.html    ← Niveau 1
└── prototype-[flow-slug]-v2-interactive.html  ← Niveau 2
```

Documenter l'URL de partage dans la spec :
```markdown
## Prototype
- Version : [1 — Cliquable / 2 — Interactif]
- Fichier : `design/[feature-slug]/prototype/[nom].html`
- Flow couvert : [FLUX-###]
- Écrans : [S-XX, S-XX, S-XX]
- Partagé au client : [Oui / Non]
- Date : [YYYY-MM-DD]
```

## Phase de validation — niveau standard

Avant de passer au skill suivant, réponds à ces questions :

1. Tous les écrans du flow sont-ils présents dans le prototype — aucun manquant ? (oui / non + écrans manquants)
2. La navigation fonctionne-t-elle dans les deux sens — avancer et reculer ? (oui / non)
3. La bannière prototype est-elle visible pour signaler au client qu'il s'agit d'un prototype ? (oui / non)

> Réponds point par point. Si tout est validé, le skill se termine et les prochaines étapes s'affichent.
> Si un point nécessite une correction, le skill reprend depuis l'étape concernée.

## Résumé de fin d'exécution

```
✅ write-prototype-react "$ARGUMENTS" terminé
📁 design/$ARGUMENTS/prototype/prototype-[flow-slug]-v[N].html

Prochaines étapes :
→ Partager le fichier HTML au client (email, lien Drive, etc.)
→ /write-client-deliverable $ARGUMENTS (document client avec URL prototype)
→ /review-figma-scope $ARGUMENTS (validation PO)
```
