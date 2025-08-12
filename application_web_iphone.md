# Spécifications techniques du projet

## ⚙️ Stack Technique

# Spécifications de l'application web

## 🎯 Objectif

Créer une application web locale (et potentiellement cloud plus tard) pour une utilisation sur iPhone et MacOS.

---

## ⚙️ Architecture souhaitée

### 📌 Backend
- Node.js avec **Express**
- API REST exposées pour lecture/écriture de fichiers JSON (pas de base de données à ce stade)
- Serveur accessible depuis le réseau local

### 📌 Frontend
- **React** comme framework principal
- **React Router** pour la navigation multi-pages
- **Tailwind CSS** pour le style (design moderne et rapide à prototyper)
- **Vite** pour le build et le serveur de développement
- Compatible avec affichage mobile (iPhone) + rendu proche d'une app native

---

## 📦 Arborescence projet attendue

```
/client
  ├── src/
  │   ├── pages/          # Écrans (Accueil, Cultures, Historique…)
  │   ├── components/     # Composants réutilisables
  │   ├── assets/         # Images (PNG/SVG), icônes
  │   ├── App.jsx
  │   ├── main.jsx
  ├── index.html
  ├── vite.config.js

/server
  ├── data/               # Données persistées en JSON
  ├── public/             # Contiendra les fichiers buildés du frontend
  ├── routes/             # Fichiers API Express
  ├── server.js           # Point d’entrée backend

package.json (à la racine)
```

---

## 📋 Besoins spécifiques

- API REST (GET, POST, PUT, DELETE) sur les ressources gérées. Prévoir pour chaque API un GET sur un id.
- Données stockées dans des fichiers `.json`
- Application responsive (iPhone en priorité, mais peut être utilisée sur mac avec Safari)
- Icônes/illustrations personnalisées (PNG ou emoji dans un premier temps)
- Navigation bas de page type application mobile
- Préférence pour une structure modulaire et simple à maintenir
- Fichier `.http` pour tester les routes avec l’extension REST Client dans VS Code
- Produire des logs détaillées ( avec le contenu pour tous les verbes HTTP ) pour chaque appels
---

## 🧪 Commandes de développement

- `npm run clean` → Supprimer les anciens fichiers build
- `npm run prepare` → Build frontend + copie dans le backend
- `npm start` → Lancer le serveur local
- `npm restart` → clean + prepare + start

---

Merci de me générer le **squelette complet du projet** (backend + frontend), prêt à être lancé avec les commandes ci-dessus.


## 🎨 Design - Guide d’utilisation de Tailwind CSS

### 1. Palette de couleurs

- Couleur principale : `#3b7f4b` (vert nature)
- Couleur secondaire : beige clair (fonds)
- Couleur d’accent : orange (alertes)
- Thème clair et sombre
- Couleurs de base définies pour boutons, liens, titres

### 2. Typographie

- Police par défaut : sans-serif (`font-sans`)
- Titre principal : `text-2xl` `font-bold`
- Sous-titres : `text-xl` `font-semibold`
- Texte courant : `text-base`
- Interlignes standards

### 3. Espacements & marges

- Padding : `p-4` pour les blocs
- Marges internes : `mb-4`, `mt-6`
- Espacement entre éléments `gap-4`

### 4. Bordures & arrondis

- Bordure par défaut : `border border-gray-200`
- Coins arrondis : `rounded-xl`
- Ombres : `shadow-md`

### 5. Composants réutilisables

- Boutons (primaire, secondaire)
- Cartes (culture, récolte)
- Champs de formulaire stylés
- Barre de navigation fixe

### 6. Animations & transitions

- Hover : `hover:scale-105`, `hover:bg-green-600`
- Transitions : `transition-all duration-200`
- Apparition douce : `fade-in`, `slide-up`

### 7. Responsive design

- Mobile first, principalement iPhone, mais possible utilisation avec Safari sur un mac
- Breakpoints : `sm`, `md`, `lg`
- Grilles et navigation mobile optimisées

### 8. Illustrations & icônes

- Format : PNG-24 512x512 transparent
- Stockées dans `assets/cultures/`
- Style : flat, lisible, inspiré des emojis

