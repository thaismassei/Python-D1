# La répartition du temps de parole entre les hommes et les femmes à la radio et à la télévision française


## Auteur

Thaïs Massei <thais.massei@psemail.eu>

---------------------------------------------------------------------------------------------------------------------------------

## Les données utilisées : 

### La base principale : temps de musique et temps de parole des hommes et des femmes à la TV et à la radio

Site d'origine : <https://www.data.gouv.fr/fr/datasets/temps-de-parole-des-hommes-et-des-femmes-a-la-television-et-a-la-radio/>

Fichier lourd : la base de données étant assez lourde, elle peut être importée en la téléchargeant directement depuis la source (voir début du notebook).

### Les bases annexes complémentaires

* Web Scraping avec Beautiful Soup et avec la fonction read_html de pandas : récupération des données d'un tableau sur la page Wikipédia <https://fr.wikipedia.org/wiki/Liste_des_cha%C3%AEnes_de_t%C3%A9l%C3%A9vision_en_France>. Les colonnes intéressantes pour l'analyse ont été isolées : le nom des chaînes ainsi que leur genre. 15 chaînes correspondent entre la base de données principales et les chaines reportées dans le tableau Wikipédia. Cela a permis de regrouper les chaînes par catégorie: Documentaire, Généraliste, Généraliste musical, Information, Sport.

* Base d'audience: <https://www.data.gouv.fr/fr/datasets/545215a9c751df25e7edf3d6/>

Sur data.gouv, une base de données sur le taux d'audience des chaînes de télévision par année a été récupérée. Cette base a permis d'enrichir notre analyse en couplant l'évolution du taux de parole des femmes sur les chaînes tv avec leur taux d'audience. 15 chaînes correspondent également entre la base de données initiale et cette nouvelle base. 

## Les packages utilisés :

* `numpy`
* `pandas`
* `matplotlib.pyplot`
* `bs4`
* `seaborn` 
* `prophet`: librairie open source de prévisions de données temporelles. Elle permet de décomposer les séries temporelles en plusieurs composants (modèle additif). Au sein de ce package, `fbprophet.diagnostics` a également été utilisé.  

## Contenu du projet : 

Le notebook est organisé comme suit : il comprend une description de la base de données, une analyse de cette dernière à l'aide de plusieurs métriques, différentes visualisations, un web scraping, un matching des deux bases de données, ainsi qu'un exercice de modélisation qui consiste à prédire le taux d'audience d'une chaine pour les mois qui suivent l'arrêt de la base de données.  

### 1. Imports des bibliothèques

### 2. Structure des données : quelques statistiques descriptives

Cette partie vise à importer et à présenter la base de données principale afin d'en comprendre la structure à travers des opérations basiques de comptage, d'affichage d'échantillon de la base. Une conversion de la colonne `date` au format date et la création d'une colonne `year` sont également effectuées.

### 3. Analyse descriptive des données

Cette partie explore les différentes possibilités offertes par la base de données en réalisant plusieurs agrégations qui permettent de changer de niveaux de granularité. L'étude est à la fois thématique et temporelle. On réalise des visualisations tout au long de cette partie pour illustrer les résultats trouvés en explorant la base de données principale. Des classements par chaines sont également effectués. Ces derniers peuvent être testés avec les chaînes de votre choix. 

Méthodologie : la présence de nombreux sous-*dataframes* vient du fait que j'ai choisi de recalculer les ratios `women_expression_rate`, `men_expression_rate`, `music_rate` et `speech_rate` à chaque fois pour chaque différent `group_by` réalisé. 

#### 3.1 Calcul des ratios

Introduction de la fonction ratio qui sera appliquée à plusieurs sous dataframes. Cette  fonction a pour objectif de  calculer pour un tableau donné la  part de parole féminine et masculine ainsi que le taux de musique. 

*Méthodologie*: Afin de donner la bonne pondération à chaque ligne lors d'une agrégation le ratio est recalculé à chaque fois. Le ratio féminin et masculin sont calculés par rapport au temps de parole total (hors musique donc), tandis que le temps de musique est pris sur le  temps total d'émission. 

#### 3.2 Agrégation à l'année et par chaine

#### 3.3 Agrégation par chaine seulement 

#### 3.4 Agrégation par chaîne et distinction public/privé

#### 3.5 Étude de l'évolution temporelle du temps de parole des femmes à différents niveaux

Dans  cette  partie, j'étudie l'évolution du taux d'expression des femmes au fil des années. 

#### 3.6 Sur quelles thématique les femmes parlent-elles le plus? 

Dans cette partie, j'analyse les sujets sur lesquels les femmes sont amenées à parler, à la fois à la radio et à la télévision. Pour cela, j'ai choisi de grouper les chaînes et stations selon différents critères : le pourcentage de diffusion de musique des stations radio et le genre des chaînes télévisées.

3.6.1 Les stations radio musicales sont-elles plus féminines ou masculines en terme de temps de parole ?

Pour y répondre, je commence par déterminer le niveau à partir duquel une chaîne est considérée comme musicale. 

3.6.2 Étude du taux d'expression des femmes selon le type des chaînes de télévisions

Après avoir réparti les chaînes par types à partir de données récupérées sur internet par webscraping, des visualisations similaires aux précédentes sont réalisées mais par type de chaîne cette fois-ci.

#### 3.7 Taux de parole des femmes en fonction des audiences

Intérêt pour la relation entre taux d'audience de chaînes télévisées et taux d'expression des femmes sur ces chaînes. 

### 4. Modélisation : Forecast

Je choisis de prédire l'évolution mensuelle du women expression rate pour une chaine donnée et pour un nombre de mois après la date de la dernière information certaine à l'aide du Package Prophet. 

#### 4.1 Préparation des données conformément au modèle

L'input est une base de donnée à deux colonnes : `ds` et `y`.
- `y` (target) est la colonne numérique sur laquelle on veut obtenir le forecast.
- `ds` (datastamp column) est la colonne qui représente une date ou un instant.

#### 4.2 Modèle : time series forecasting avec Prophet

Méthodologie: Prophet est une une bibliothèque (Python et R) de prévision de séries chronologiques basée sur un modèle additif où les tendances non linéaires sont ajustées à la saisonnalité annuelle notamment. La fonction `forecast` qui affiche la prédiction permet de tester la prédiction pour toutes les chaines/stations de votre choix et pour les périodes de prédictions souhaitées, les exemples choisis étant pour une période de 24 mois et une de 36 mois. 

#### 4.3 Evaluation du modèle 

Dans cette partie est évaluée la performance de prédiction du modèle à l'aide des outils proposés par Prophet. 

