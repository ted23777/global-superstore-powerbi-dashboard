# Global-superstore-powerbi-dashboard

Analyse de la performance commerciale d'un distributeur international sur la période 2012-2015 : croissance, rentabilité, impact des remises et qualité de service logistique.

Résultats
1. La croissance vient du volume, pas de la valeur. Le chiffre d'affaires progresse de 90 % entre 2012 et 2015 (2,26 M$ → 4,30 M$), porté par un nombre de commandes en hausse de 96 %. Le panier moyen recule de 3 % sur la même période (500 $ → 485 $).

2. Les remises au-delà de 20 % font perdre 815 K$ de marge. La marge bascule en négatif dès la tranche 21-30 %. Les ventes remisées au-delà de 20 % représentent 15 % du chiffre d'affaires mais coûtent 814 682 $ de marge : sans elles, la marge totale de la période serait supérieure de 55 %. Au-delà de 30 % de remise, chaque dollar vendu coûte 51 cents.

3. La seule sous-catégorie déficitaire est rentable… quand elle n'est pas remisée. Tables perd 64 083 $ sur 757 042 $ de chiffre d'affaires. Vendue sans remise, elle dégage pourtant 23,09 % de marge, au-dessus de la moyenne du catalogue. Mais 54 % de ses commandes sont remisées à plus de 20 %, contre 26 % tous produits confondus. Son déficit relève de la politique commerciale, pas du produit.

4. Le process d'expédition respecte les priorités annoncées. 1,8 jour de délai moyen pour les commandes critiques contre 6,5 pour les commandes en priorité basse, soit un rapport de 1 à 3,6. Aucune commande critique ou haute n'emprunte l'expédition standard.


# Contexte

Ce projet reconstitue la démarche d'un analyste BI recevant un extrait de données de ventes brut : modélisation, création des indicateurs, puis construction d'un rapport destiné à une direction commerciale.

L'objectif n'est pas la complexité technique du modèle mais la lisibilité du résultat - un décideur doit pouvoir lire la page de synthèse en dix secondes et identifier les leviers d'action sur les pages suivantes.

 # Données

Jeu Global Superstore, disponible publiquement sur Kaggle. Trois tables :

- Orders	51 290	Lignes de commande
- Returns	1 079	Commandes retournées
- People	24	Responsables commerciaux par région

Période couverte : janvier 2012 à décembre 2015

![alt text](<resultats/Schéma de la modélisation de données.png>)

# Stack

Power BI Desktop · Power Query (M) · DAX

# Démarche

1. Préparation dans Power Query Typage explicite des colonnes, suppression des champs inutilisés (Row ID, Postal Code), dédoublonnage de la table Returns sur Order ID. Création de trois colonnes calculées : le délai de livraison (Ship Date - Order Date), une tranche de remise en cinq paliers, et une colonne d'index garantissant l'ordre d'affichage de ces paliers.

2. Modélisation Modèle en étoile simplifié autour de la table de faits Orders, avec une table de dates dédiée créée en DAX (CALENDAR), marquée comme table de dates et reliée à Order Date. Relations un-à-plusieurs unidirectionnelles, sans filtre croisé bidirectionnel. Les mesures sont regroupées dans une table dédiée, et les colonnes numériques brutes sont masquées dans la vue rapport pour interdire toute agrégation implicite.


3. Mesures DAX Toutes les agrégations passent par des mesures explicites. Le détail commenté figure dans le fichier mesures_dax

Une difficulté méritait attention : la mesure de marge perdue. Une première version filtrait la table de faits ligne à ligne, ce qui additionnait toutes les ventes individuellement déficitaires, y compris celles de sous-catégories globalement rentables, soit un résultat quinze fois trop élevé. La version retenue itère sur les valeurs distinctes de Sub-Category et ne retient que celles dont la marge agrégée est négative.

4. Construction du rapport Quatre pages : la synthèse, une vue sur les produits, une vue sur les remises et une vue sur la logistique.


## LES DIFFERENTES PAGES DU DASHBOARD

# Vue page synthèse
![alt text](<resultats/image-2.png>)

# Vue page remises
![alt text](<resultats/image-3.png>)

# Vue page produits
![alt text](<resultats/image-4.png>)

# Vue page logistique
![alt text](<resultats/image-5.png>)
