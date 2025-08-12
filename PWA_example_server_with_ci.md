
# 📱 PWA.md – Exemple rempli (Serveur + Multi-utilisateur + CI/CD automatisé)

---

## 🔧 1. Objectif de l'application

L'application permet à deux jardiniers (moi et mon épouse) de suivre nos récoltes saisonnières, de gerer les différentes cultures, d’ajouter des photos lors de la recolte, de consulter des statistiques, de suivre les dates de semis, repiquage, plantation des différentes cultures. Les données sont sauvegardées côté serveur dans des fichiers JSON, avec possibilité d’évolution vers une base de données. Le projet intègre un système CI/CD complet pour versionnage automatique et déploiement fluide.
L'application doit être multilangue avec prise en compte du Francais et du Serbe dès la premiere version.

Nom de l'application: Baštouille
---

## 🧑‍🌾 2. Profils utilisateurs

- Jardinier 1 (moi)
- Jardinier 2 (mon épouse)
- Les informations d'un utilisateur sont persistées côté serveur ( API )
Au premier lancement de l'application, selectionner l'utilisateur dans une liste avec enregistrement de l'id dans LocalStorage. 
Les prochaines lancements, l'utilisateur est recuperer depuis LocalStorage puis appel serveur
le nom, la langue, le mode d'affichage ( clair, sombre, auto ) seront associés à l'id de l'utilisateur
---

## 📱 3. Écrans principaux

### Accueil
- Résumé des récoltes recentes
- La plus récente doit être mise en evidence 
  - l'image de culture plus grande
  - Sous l'image, la quantité et la date et l'heure
- Affichage des suivantes. Le nombre sera limité par la taille de l'écran
  - l'image de culture miniature, la quantité, la date et l'heure
### Culture
- Affichage de la liste complete des cultures ( prévoir le scroll )
  - L'image miniature, le nom
- Sur clique d'un element de la liste entrez dans un écran détail
  - lien pour revenir vers la liste
  - Image de la culture, la moitié de l'écran
  - Sous l'image, le nom
  - UUID: culture.id
  - Catégorie: culture.categorie
  - Mode de récolte: culture.moderecolte
  - Bouton "Editer"
  - Sur clique sur le bouton Editer passer l'écran en mode édition
    - Le nom, catégorie et mode de récolte passe en mode éditable
    - Le bouton "Editer" devient "Mettre à jour"
    - Sur clique sur le bouton "Mettre à jour", 
      - mettre à jour la culture côté serveur
      - afficher le message de success ou d'erreur
### Récolte ( ajout )
- Affichage de la liste complete des cultures
  - l'image miniature, le nom
- juste au-dessu de la liste afficher une zone permettant de rechercher une culture en particulier, le filtre doit s'activer dès la premère lettre saisie
- Une fois la culture sélectionné, basculer dans le mode saisie de recolte
  - lien pour revenir à la liste
  - l'image centrée de la culture selecitonnée ( 1/3 de l'écran )
  - Sous l'image le nom de la culture
  - afficher la date et l'heure ( pas de possibilité de modifier )
  - afficher les informations méteo réelle ( temperature, humidité, pression, vent, indice uv )
  - en fonction du mode de recolte ( unité ou poids )
    - si unité
      - afficher une zone pour la saisie de la quantité récolté ( obligatoire )
    - si poids ( grammes )
      - afficher une zone pour la saisie de la quantité récolté, à côté de la zone afficher "grammes" ( obligatoire )
      - afficher une case à cocher ( activer par défaut ) et une liste déroulante avec les images des différentes récipient. Si case à cocher activée, le choix doit être obligatoire 
  - Le nécessaire pour pouvoir prendre une photo de la recolte avec la camera de l'iphone. Non obligatoire pour la saisie
  - Bouton "Ajouter"
  - Sur clique sur le bouton "Ajouter"
    - Completer les données saisie avec l'id utilisateur
    - Si case à cocher activée, soustraire le poids du recipient selectionné du poids saisie
    - Enregistrer les données côté serveur
    - Afficher un message de succes ou d'erreur 
    - Revenir sur l'écran de recoltes avec la liste des cultures
### Historique
- afficher une selection de l'année. La liste des années disponibles doit être calculée côté serveur en fonction des recoltes saisies
- Sur selection d'une année
  - afficher un tableau avec 2 colonnes ( culture et cumul )
    - colonne1: afficher la liste unique de toutes les cultures recoltées pour l'année sélectionnée
      - Image miniature + nom
    - colonne2: le cumul des recoltes de la culture
      - si mode de recolte en unité
        - afficher le nombre d'unité cumulé
      - si mode de recolte en poids
        - afficher le poids cumulé. Si le cumul < 1000 indiquer les grammes, sinon convertir en kilogrammes et indiquer Kg
  
### Statistiques

### Paramètres
- Afficher le nom de l'utilisateur ( sans pouvoir le changer )
- Afficher l'UUID de l'utilisateur ( sans pouvoir le changer )
- Afficher la liste des langues possibles en selectionnant celle déjà configurée pour l'utilisateur. Si pas configuré, considerer Francais par défaut
- Afficher la liste des modes d'affichage possibles en selectionnant celui déjà configurée pour l'utilisateur. Si pas configuré, considerer mode clair par défaut
- Si l'utilisateur modifie la langue ou le mode, enregistrer les informations côté serveur, avant tout changement d'écran
- Indiquer si la mise à jour a pu se faire ou pas. 
- Afficher un element de séparation 
- Afficher la liste des récipients:
  - L'image associée
  - Le poids en gramme associé
---

## 🧭 4. Navigation
- Barre supérieure ( en haut de l'écran ) présente sur toutes les pages
  - Cette barre contient le nom de l'application et la version de l'application
- Barre de navigation présente sur toutes les pages, en bas de l'écran
  - Accueil
  - Cultures
  - Récoltes (ajouter)
  - Historique
  - Statistiques
  - Paramètres
- La zone centrale de l'écran est disponible pour affichage des différentes écrans. 
---

## 📦 5. Données à gérer (modèles)

### Récipient
- id ( de type UUID )
- Nom
- Image ( illustration )
- Poids du recipient en gramme
  
### Culture
- id ( de type UUID )
- Nom 
- Image ( illustration )
- Categorie: fruit / légume / aromatique / fleur
- Mode de récolte: en poids -> gramme (ex: tomate 500g ) ou en unité ( ex: ail 3 )
  
### Récolte
- id ( uuid )
- id utilisateur
- id culture  
- quantité (nombre)
- date & heure (format DD/MM/YYYY HH:mm)
- photo (URL/base64)
- les informations méteo ( temperature, humidité, pression, vent, indice uv )
- id utilisateur ayant réalisé la saisie

### Utilisateur
- id ( uuid )
- nom
- couleur thème (optionnel)
- langue

---

## 🔐 6. Authentification

- Pas d'authentification serveur
- Sélection manuelle de l'utilisateur sur l'app
- Association des actions à un identifiant utilisateur

---

## 🧰 7. Fonctionnalités PWA

- [x] Ajout à l’écran d’accueil
- [ ] Offline partiel (non prioritaire)
- [ ] Notifications push
- [x] Prise de photo via mobile
- [x] Synchronisation complète avec un serveur
- [x] Données stockées en JSON sur le serveur

---

## 🌐 8. Stack technique souhaitée
### 📌 Backend
- Node.js avec **Express**
- API REST exposées pour lecture/écriture de fichiers JSON (pas de base de données à ce stade)
- Serveur accessible depuis le réseau local

### 📌 Frontend
- **React** comme framework principal
- **React Router** pour la navigation multi-pages
- **Tailwind CSS** pour le style (design moderne et rapide à prototyper)
- **Vite** pour le build et le serveur de développement
### 📌 Persistance
- Persistance : fichiers JSON (`data/utilisateur1.json`), extensible vers SQLite ou PostgreSQL

---

## 🧪 9. Cas d’usage typique

1. L’utilisateur ouvre l’app depuis son iPhone
2. Il choisit “Moi” ou “Mon épouse”
3. Il clique sur “+” et prend une photo de sa récolte
4. Il entre les infos (plante, quantité, date)
5. La récolte est envoyée et stockée dans `data/mon_utilisateur.json`
6. L’autre utilisateur voit la récolte depuis son propre appareil

---

## 🔁 10. Intégration CI/CD automatisée

L’application inclut :
- Versioning automatique via `semantic-release`
- Génération automatique du `CHANGELOG.md`
- Tagging Git + création de release GitHub
- Pipeline GitHub Actions sur `main`
- Convention de commit (`feat:`, `fix:`, etc.) validée avec `commitlint` et `husky`
- Déploiement automatisé possible vers Vercel, Netlify ou un VPS

---

## 📁 11. Détail pour le réndu ( style, ui, etc.. ) de l'application

- Compatible iOS Safari (capture, ajout à l’écran d’accueil, offline simplifié)
- S'agissant d'une application de jardin, utiliser la couleur verte ( trouver une ton vert agréable )
- Adapter le mode clair et sombre par rapport au ton général de l'application 
- S'inspirer, au niveau rendu, de l'application youtube ( barre supérieur avec le titre, barre de navigation en bas )
- Je fourni les images d'illustration des cultures. C'est des png-24, transparent, taille 512x512
- Je fourni également les illustration pour les recipients. C'est des png-24, transparent, taille 512x512
- 
