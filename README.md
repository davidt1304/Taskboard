# TaskBoard — Guide d'installation

## Architecture

TaskBoard est une **Progressive Web App (PWA)** — un fichier HTML unique qui s'installe comme une app native sur Windows 11 et iOS, fonctionne offline, et synchronise les données entre appareils via un GitHub Gist privé.

```
taskboard/
├── index.html       ← L'app complète (HTML/CSS/JS)
├── manifest.json    ← Manifeste PWA (installabilité)
├── sw.js            ← Service Worker (offline)
├── icon-192.png     ← Icône PWA
├── icon-512.png     ← Icône PWA
└── README.md        ← Ce fichier
```

---

## Étape 1 — Héberger sur GitHub Pages

### 1.1. Créer un dépôt GitHub

1. Aller sur https://github.com/new
2. Nom : `taskboard` (ou autre)
3. Visibilité : **Public** (requis pour GitHub Pages gratuit)
4. Cliquer **Create repository**

### 1.2. Pousser les fichiers

```bash
cd taskboard
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/VOTRE_USER/taskboard.git
git push -u origin main
```

### 1.3. Activer GitHub Pages

1. Aller dans **Settings** → **Pages**
2. Source : **Deploy from a branch**
3. Branch : `main` / `/ (root)`
4. Cliquer **Save**
5. Attendre ~1 min → votre app est sur `https://VOTRE_USER.github.io/taskboard/`

---

## Étape 2 — Configurer la synchronisation

La sync utilise un **GitHub Gist privé** comme stockage. Vos données restent chez GitHub, sous votre contrôle.

### 2.1. Créer un Personal Access Token (classic)

> **Pourquoi un token classique ?** L'API Gist n'est pas entièrement couverte par les fine-grained tokens. Le token classique avec le seul scope `gist` est la méthode fiable.

1. Aller sur https://github.com/settings/tokens/new
2. **Note** : `TaskBoard Sync`
3. **Expiration** : choisir selon votre préférence (90 jours recommandé, renouvelable)
4. **Select scopes** : cocher uniquement **gist** (Create gists)
5. Cliquer **Generate token**
6. **Copier le token** (il commence par `ghp_` et ne sera plus visible)

### 2.2. Configurer dans l'app

1. Ouvrir TaskBoard
2. Cliquer sur **⚙ Configurer la synchronisation** en bas de la sidebar
3. Coller votre token
4. Laisser le champ Gist ID vide → l'app créera automatiquement un Gist privé
5. Cliquer **Enregistrer & Sync**

### 2.3. Sur le deuxième appareil

1. Ouvrir la même URL
2. Configurer la sync avec le **même token** et le **même Gist ID**
3. Cliquer **Enregistrer & Sync** → les données se chargent

> Le Gist ID s'affiche dans les paramètres après la première sync. Notez-le.

---

## Étape 3 — Installer comme app

### Windows 11 (Edge ou Chrome)

1. Ouvrir `https://VOTRE_USER.github.io/taskboard/`
2. Cliquer l'icône **⊕** (installer) dans la barre d'adresse
3. Ou : Menu ⋯ → **Installer TaskBoard**
4. L'app apparaît dans le menu Démarrer et la barre des tâches

### iOS (Safari)

1. Ouvrir l'URL dans **Safari**
2. Taper l'icône **Partager** (carré avec flèche)
3. Choisir **Sur l'écran d'accueil**
4. Nommer et confirmer
5. L'app apparaît comme une icône sur l'écran d'accueil, en plein écran

---

## Fonctionnalités

- **Vue Board** : Kanban avec drag & drop entre colonnes
- **Vue Liste** : tableau triable avec changement de statut rapide
- **Projets** : organisation par projet avec code couleur
- **Priorités** : Haute / Moyenne / Basse
- **Statuts** : À faire → En cours → En attente → Terminé
- **Offline** : fonctionne sans connexion, sync au retour
- **Responsive** : adapté mobile et desktop

---

## Sécurité

- Le token GitHub est stocké dans le `localStorage` de votre navigateur uniquement
- Le Gist est **privé** (visible uniquement avec votre token)
- Aucune donnée ne transite par un serveur tiers
- Le token a uniquement le scope `gist` (pas d'accès à vos repos)
