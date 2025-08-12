
# 📱 PWA.md – Exemple mis à jour (version online + backend API)

---

## 🔧 1. Objectif de l'application

L'application permet à un jardinier amateur de suivre ses récoltes et cultures via une interface mobile accessible depuis son iPhone, avec enregistrement en ligne, stockage dans une base de données distante, et consultation des statistiques. L'app est en ligne dès la première version.

---

## 🧑‍🌾 2. Profils utilisateurs

- Jardinier particulier
- Aucun rôle administrateur

---

## 📱 3. Écrans principaux

- Accueil : résumé global des récoltes
- Ajouter une récolte : formulaire d’enregistrement d’une récolte
- Historique : liste filtrable des récoltes
- Statistiques : graphiques de production
- Cultures : gestion complète des plantes cultivées (CRUD)
- Paramètres : préférences utilisateur

---

## 🧭 4. Navigation

- Barre de navigation présente sur toutes les pages
  - Accueil
  - Récoltes (ajouter)
  - Historique
  - Statistiques
  - Cultures
  - Paramètres

---

## 📦 5. Données à gérer (modèles)

### Récolte
- id
- cultureId (référence à Culture)
- quantité (nombre ou unité)
- date (ISO string)
- photo (URL)

### Culture (Plante)
- id
- nom (ex: "Tomate")
- image (URL ou base64)
- catégorie (fruit, légume, aromatique, fleur)
- typeDeRecolte (gramme | unité)

### Utilisateur
- id
- nom
- langue
- thème préféré

---

## 🔐 6. Authentification

- Pas d’authentification pour la v1
- Données stockées sur un serveur en ligne (backend Express)

---

## 🧰 7. Fonctionnalités PWA

- [x] Fonctionne hors-ligne (cache de pages)
- [x] Ajout à l’écran d’accueil
- [ ] Notification push (plus tard)
- [x] Prise de photo depuis l’appareil
- [x] Synchronisation avec un serveur distant (Node.js API)

---

## 🌐 8. Stack technique souhaitée

- Frontend : React + Vite
- Backend : Node.js + Express
- Base de données : PostgreSQL ou SQLite (v1 hébergée)
- Stockage photo : en base64 (temporairement) ou dossier uploads

---

## 🧪 9. Cas d’usage typique (user flow)

1. L’utilisateur ouvre l’app depuis l’écran d’accueil de l’iPhone
2. Il navigue vers “Récoltes”
3. Il sélectionne une culture existante (ex: "Fraise")
4. Il prend une photo, saisit “500g”, sélectionne la date
5. Il valide → les données sont envoyées à l’API Express
6. Il peut ensuite consulter ses stats dans “Historique” ou “Statistiques”

---

## 📁 10. Détail optionnel

- Référence UI : style iOS natif (Framework7 / Tailwind)
- Inspirations UX : "Notes" Apple + "Calendrier"
- Chaque culture peut être :
  - modifiée : changer nom, image, type
  - supprimée (après confirmation)
  - visualisée en détail (dans un écran dédié)
