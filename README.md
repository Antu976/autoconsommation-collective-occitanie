# Suivi des opérations d’autoconsommation collective par région

Ce projet présente un prototype de dashboard BI permettant de suivre l’évolution des opérations d’autoconsommation collective actives par région.

L’objectif est de construire une chaîne simple de traitement et de visualisation de données :

* nettoyage des données avec Python ;
* préparation d’un fichier propre pour Power BI ;
* modélisation des données dans Power BI ;
* création d’un dashboard interactif ;
* analyse de l’évolution dans le temps et des différences entre régions.

## Contexte

L’autoconsommation collective permet à plusieurs acteurs situés dans un même périmètre géographique de partager une production locale d’électricité.

Dans ce projet, les données utilisées sont des données publiques issues d’Enedis Open Data.

Le projet est construit comme un prototype BI permettant de suivre des indicateurs liés au développement de l’autoconsommation collective en France, par exemple :

* le nombre d’opérations actives ;
* l’évolution trimestrielle ;
* la répartition par région ;
* les dynamiques territoriales, notamment en Occitanie.

## Données utilisées

Fichier brut :

data/raw/autoconsommation_region.csv

Fichier nettoyé utilisé pour Power BI :

data/processed/autoconsommation_region_clean.csv


Colonnes principales du fichier nettoyé :

trimestre_date
annee
code_region
nom_region
operations_actives
valeur_operation_manquante
periode


Les données sont agrégées par région et par trimestre.
Une ligne ne correspond donc pas à une opération individuelle, mais à une observation agrégée pour une région à une période donnée.

## Structure du projet

autoconsommation-collective-occitanie/
│
├── data/
│   ├── raw/
│   │   └── autoconsommation_region.csv
│   │
│   └── processed/
│       └── autoconsommation_region_clean.csv
│
├── notebooks/
│   ├── 01_analyse_autoconsommation.ipynb
│   └── 02_visualisations_python.ipynb
│
├── dashboard/
│   ├── autoconsommation_collective_dashboard_v1.pbix
│   └── autoconsommation_collective_dashboard_v2.pbix
│
├── images/
│   ├── dashboard_v1.png
│   ├── dashboard_synthese_v2.png
│   └── dashboard_analyse_regionale_v2.png
│
├── slides/
│
├── app.py
├── requirements.txt
└── README.md


## Traitement des données avec Python

Le notebook Python permet de :

* charger les données brutes ;
* vérifier la structure du fichier ;
* nettoyer les noms de colonnes ;
* convertir les périodes trimestrielles en dates exploitables ;
* créer des colonnes utiles pour l’analyse ;
* exporter un fichier propre pour Power BI.

Le fichier final utilisé dans Power BI est :

data/processed/autoconsommation_region_clean.csv

## Modèle de données Power BI

La version 2 du dashboard utilise un modèle de données en étoile.

Le modèle est composé de trois tables principales :

FactOperations
DimRegion
Dimdate


#Table de faits : FactOperations

La table `FactOperations` contient les observations trimestrielles par région.

Colonnes principales :

trimestre_date
code_region
operations_actives
valeur_operation_manquante


### Dimension région : DimRegion

La table `DimRegion` contient les informations liées aux régions.

Colonnes principales :

code_region
nom_region


### Dimension temporelle : Dimdate

La table `Dimdate` est créée dans Power BI avec DAX à partir de `CALENDARAUTO()`.

Elle permet de structurer l’analyse temporelle avec :

Date
Année
Mois
Trimestre
Periode
Tri périodes


### Relations du modèle

Les relations utilisées sont :

DimRegion[code_region] → FactOperations[code_region]
Dimdate[Date] → FactOperations[trimestre_date]


Les dimensions filtrent la table de faits, ce qui permet d’analyser les opérations par région et dans le temps.

## Indicateurs calculés dans Power BI

Le dashboard utilise plusieurs mesures DAX afin de transformer les données brutes en indicateurs exploitables.

Les principaux indicateurs calculés sont :

* le nombre total d’opérations actives ;
* le nombre d’opérations actives sur la dernière période disponible ;
* la variation par rapport au trimestre précédent ;
* le taux d’évolution par rapport au trimestre précédent ;
* le taux d’évolution trimestriel dans le temps ;
* le taux d’évolution trimestriel de l’Occitanie utilisé comme référence fixe.

Ces mesures permettent de distinguer une simple somme d’observations d’une lecture métier plus utile, centrée sur la dernière période disponible et sur l’évolution temporelle.

Une mesure spécifique a été créée pour conserver l’Occitanie comme territoire de référence, même lorsqu’une autre région est sélectionnée dans le rapport.

## Dashboard Power BI

Le rapport Power BI est organisé en deux pages.

### Page 1:Synthèse globale

Cette page donne une vue d’ensemble de la situation sur la dernière période disponible.

Elle contient :

* le nombre d’opérations actives sur la dernière période disponible ;
* la variation par rapport au trimestre précédent ;
* le taux d’évolution par rapport au trimestre précédent ;
* l’évolution des opérations actives dans le temps ;
* le classement des régions selon le nombre d’opérations actives.

Cette page répond à la question suivante :

> Où en est le développement de l’autoconsommation collective par région sur la dernière période disponible ?

Aperçu :

Page synthèse: (images/dashboard_synthese_v2.png)

### Page 2:Analyse régionale avec focus Occitanie

Cette page permet d’analyser la dynamique de croissance par région.

Elle contient :

* un graphique en barres permettant de sélectionner une région ;
* une courbe du taux d’évolution trimestriel pour la région sélectionnée ;
* une courbe de référence fixe pour l’Occitanie.

Cette page permet de comparer une région sélectionnée avec l’Occitanie, qui est utilisée comme territoire de référence dans l’analyse.

Elle répond à la question suivante :

> Comment évolue une région sélectionnée par rapport à l’Occitanie ?

Aperçu :

Page analyse régionale:(images/dashboard_analyse_regionale_v2.png)

## Versions du dashboard

Deux versions du dashboard sont conservées dans le projet.

dashboard/autoconsommation_collective_dashboard_v1.pbix


La version 1 correspond à une première version simple du rapport.

Aperçu de la version 1 :

Dashboard V1:(images/dashboard_v1.png)


dashboard/autoconsommation_collective_dashboard_v2.pbix


La version 2 correspond à une version plus structurée avec :

* un modèle en étoile ;
* une table de dates ;
* des mesures DAX ;
* deux pages d’analyse ;
* une comparaison région sélectionnée vs Occitanie.

## Technologies utilisées


Python
Pandas
Jupyter Notebook
Power BI
DAX
Git / GitHub


## Limites du projet

Ce projet repose uniquement sur des données publiques agrégées.

Les principales limites sont :

* absence d’informations détaillées sur les producteurs, consommateurs ou volumes d’énergie ;
* analyse basée sur le nombre d’opérations actives, et non sur la puissance installée ou l’énergie autoconsommée ;
* données agrégées par région et par trimestre ;
* absence d’analyse économique ou énergétique détaillée.

Le projet doit donc être compris comme un prototype BI démontrant une démarche d’analyse, de modélisation et de visualisation.

## Objectif professionnel du projet

Ce projet a été réalisé dans une logique de portfolio Data Analyst.

Il met en avant les compétences suivantes :

* nettoyage et préparation de données ;
* structuration d’un projet data ;
* création d’un modèle de données BI ;
* création de mesures DAX ;
* conception d’un dashboard Power BI ;
* analyse temporelle ;
* analyse comparative par région ;
* valorisation d’un cas d’usage lié à la transition énergétique.

## Source des données

Données publiques issues d’Enedis Open Data.
