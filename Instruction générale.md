# Description générale
  
- Application web responsive avec des fonctionnalités et rendu différents en fonction du device utilisé
	- Si mobile ( iphone ) 
		- rendu identique à une application native iPhone. 
		- Prévoir PWA pour l'installation "sur l'écran d'accueil"
		- Fonctionnalités limitées ( précisées dans la suite de la demande )
	- Si desktop ( safari / macos )
		- rendu moderne, exploiter la taille de l'écran plus grande
		- l'ensemble des fonctionnalités
- Nom de l’application : Baštouille
- Nom du projet : app-bastouille
- Next.js / React
- Shadui / Tailwindcss
- ESLint + Prettier
- Données stockées en base de données postgresql. Prevoir l’installation de postgresql 
- Prévoir un fichier readme.md qui explique tous les détails et les étapes du projet : installation, lancement, modification de l’identité visuel, etc, etc 
- Le projet sera géré dans VSC et avec git sur GitHub
- Postgres
	- base de donnée déjà créée pour le projet -> nom = bastouille
	- utilises uniquement les minuscules pour le nom des type, champs, tables, etc... 
- Identité visuelle
	- 2 themes seront proposés ( noms: Soleil du Sud / Lavande et Romarin
	- Prévoir un ThemeProvider et un ThemeSwitcher pour pourvoir changer de theme et de mode ( clair ou sombre )
	- Les deux themes contiennent un mode clair et un mode sombre
	- Les deux themes ont été constuits et testés.
	- Ils seront intégrés une fois le projet constuit ( fichier global.css )

# Fonctionnalités

## Gestion des cultures 
- création 
- modification
- liste
- suppression
- recherche par le nom

### Modele culture
- id: uuid, generer par postgres, clé primaire
- nom: string
- img: string ( nom du png d’illustration )
- categorie: énumération, valeurs possibles: fruit, légume, aromatique, fleur, par défault -> legume
- mode_recolte: énumération, valeur possibles: poids, poids_unite, par défault -> poids

## Gestion des recoltes
- ajout
- liste
- modification
- suppression
- filte par année, par categorie, par mois ( filtres cummulatifs )

### Modele Recolte
- id: auto incrementé, clé primaire
- id_culture: lien vers la culture récoltée
- date et heure: format DD/MM/YYYY HH:mm
- poids : le de la recolte en gramme ( pour tous les modes de recoltes )
- quantite: le nombre d'unité recolté ( uniquement pour le mode de recolte poids_unite
- meteo: les données meteo réélles
	- meteo.temperature
	- meteo.humidite
	- meteo.vent
	- meteo.indice_uv
	- meteo.qte_pluie

## Visualisation des statistiques de recolte
- Voir le cumul des recoltes pour l'année selectionnée
- liste des années possibles à determiner en fonction des recoltes
- pouvoir afficher l'ensemble des cultures connues, ou uniquement celles pour lequelles des recoltes sont enregistrées
- Comparer les récoltes d'une culture choisie par année
- Voir le cumul des recoltes par mois pour une cultures
- Voir une synthese: cumul des recoltes pour l'année en cours ( pas de selection ) et uniquement pour les cultures avec recoltes enregistrées

## Structure générale de l'application
### Zone supérieure ( barre de titre )
- Affichage du nom de l'application au centre
- En fonction de l'écran affiché, une autre information peut être affichée à la place du nom de l'application 
- En fonction de l'écran affiché, un lien de navigation pour revenir en arrière peut être affiché à gauche
- Affichage à droite d'une icone "parametrage" permettant d'acceder à l'écran paramètrage

### Zone centrale
- affichage du contenu d'un écran
- peut être scrollable mais uniquement entre la zone supérieure et inférieure 

### Zone inférieure dédiée à la navigation
- Accueil ( mobile et desktop )
- Culture ( desktop )
- Recolte
	- mobile: uniquement l'ajout d'une nouvelle recolte
	- desktop: l'ensemble des fonctionnalités: ajout, suppression, modification 
- Statistique
	- mobile: uniquement la synthese
	- desktop: l'ensemble des fonctionnalités

## Ecran accueil
- Aficher les dernières recoltes. 
- La toute dernière recolte est affiché en détail -> 1/3 de la zone
- Les 3 suivantes sont affichées en mode liste

## Ecran culture
- Affiche la liste des cultures disponibles
- Chaque élement de la liste: img+nom+cumul des recoltes de l'année
- Sur clique d'un élement de la liste, on rentre dans l'écran détail

### Ecran détail culture
- Affichage de toutes les données d'une culture 
- l'image s'affiche en 256x256
- Un bouton permet d'ajouter une recolte
- Possibilité de modifier le nom, la catégorie et le mode de récolte
- Si une des données est modifiée afficher un bouton mettre à jour

## Ecran Recolte
-	Choisir la culture dans une liste déroulante ( img + nom )
-	Saisie le poids en gramme 
-	Saisir le nombre d’unité ( si mode_recolte = poids_unite )
-	Affiche la date et l’heure courante avec possibilité de modifier
-	Affiche les informations méteos en temps réel recupéré depuis un site meteo
-	Bouton ajouter

## Ecran statistique
-	Afficher la liste ( liste de selection ) des années disponibles ( en fonction des recoltées réalisées )
-	Sélectionner une année
-	Afficher un tableau avec le cumul des recoltes pour l’année 
	- Col1 : img+nom de la culture
	- Col2 : poids cumulés des recoltes ( affiché en Kg )
	- Col3 : nombre d’unité pour les cultures avec mode_recolte=poids_unite

## Ecran Parametrage
- Toggle pour changer de theme: "Soleil du Sud" et "Lavande et Romarin"
- Toggle pour changer de mode: mode clair / mode dark / mode system
