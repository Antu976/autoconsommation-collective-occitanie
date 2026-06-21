# Suivi des opérations d’autoconsommation collective par région

Ce projet présente un prototype de dashboard BI permettant de suivre l’évolution des opérations d’autoconsommation collective actives par région.

L’objectif est de construire une chaîne simple de traitement et de visualisation de données :

- nettoyage des données avec Python ;
- préparation d’un fichier propre pour la BI ;
- création d’un dashboard interactif avec Power BI ;
- analyse de l’évolution dans le temps et des différences entre régions.

## Contexte

L’autoconsommation collective permet à plusieurs acteurs situés dans un même périmètre géographique de partager une production locale d’électricité.

Dans ce projet, les données utilisées sont des données publiques issues d’Enedis Open Data.  
Elles ne proviennent pas d’Enercoop ni d’Elocoop.

Le projet est construit comme un prototype BI transposable à un outil de suivi d’indicateurs, par exemple pour analyser :

- le nombre d’opérations actives ;
- l’évolution trimestrielle ;
- la répartition par région ;
- les dynamiques territoriales, notamment en Occitanie.

## Données utilisées

Fichier brut :

```text
data/raw/autoconsommation_region.csv