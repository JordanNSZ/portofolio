---
layout: post
title: "Détection des points extrêmes : comment les reprérer, les qualifier et les traiter ?"
date: 2024-03-25
description: La détection des so-called "outliers" est primordiale pour mener des analyses inférentielles non-biaisées. Cet article explore les méthodes de détection, de qualification et de traitement des points lointains. Une étude de cas est proposée avec l'ensemble de données Kaggle 'Airbnb'. 
---

## Detection d'outliers en économétrie ou algorithme d'apprentissage. 
La détection d'outlier est très importante avant la mise en place d'une analyse inférentielle ou d'un algorithme d'apprentissage automatique. En effet, la présence de points lointains peut biaiser le résultat des analyses ou rendre inconsistent un modèle d'apprentissage automatique. Pour cette raison, leur détection ne doit pas être minorée et être réalisée dans les premières phases du projet - cf. Analyse des données exploratoire, analyse univariée. 

> La principale motivation à leur détection est qu’ils peuvent étirer la moyenne ou l’écart-type à tort apportant ainsi un biais à l'estimation des paramètres d’intérêt.

La qualification d'un point en tant que point lointain -relativement aux autres points d'une variable- peut avoir deux explications. De deux choses l'une, le point est aberrant, dans quel cas il faudra le remplacer par une valeur adéquate compte tenu de notre connaissance métier ou utiliser une méthode d'imputation conservatrice à l'égard de la distribution des données. Si toutefois, aucune méthode n'est pertinente, alors il faudra supprimer ce point aberrant des données. Enfin, si le point est extrême, vos connaissances métiers seront clés pour le detecter et le traiter. En effet, un point extrême peut être, dans certains contextes, une source d'information importante : par exemple, dans le cadre de la détection de fraude, les points extrêmes sont rares et source d'information. Aussi, la conservation de ces points dans l'analyse est primordiale. 


Le dessin de l'exposé est désormais clair. Il s'agira dans un premier temps de définir les méthodes usuelles de détection de points lointains ; méthodes à la fois graphique, numérique et inférentielle. Puis, nous exposerons les chemins décisionnelles à appliquer en fonction du caractère de l'outlier - i.e. aberrant ou extrême. Enfin, nous présenterons une étude de cas en nous appuyant sur les données AirBnB disponible sur le site [Kaggle.Com](https://www.kaggle.com/datasets/arianazmoudeh/airbnbopendata). 


## Détection de points lointains.
La détection d'outliers se fait dès le début du projet d'analyse de données. Autrement dit, au moment de l'analyse descriptive univariée. Lors de cette analyse vous allez vous intéresser à la forme de la distribution de vos variables via une analyse numérique et graphique. Numérique puisque vous allez consulter les valeurs de certains paramètres statistiques clés tels que les paramètres de tendance centrale (étendue, Q1, Q2, Q3, Q4, moyenne) et ceux de dispertion (variance, écart-type). Cette première inspection numérique vous donnera un premier aperçu de l'état de votre distribution, par exemple, si elle est assymétrique ($moyenne - mediane \neq 0$. Pour rappel, si le coefficient est inférieur à $0$, asymétrie à gauche ; si le coefficient est supérieur à $0$, asymétrie à droite). De plus, il est usuel de compléter cette information numérique d'une inspection graphique de la distribution. Généralement on utilise un histograme et/ou un diagramme en boîte (a.k.a. boîte à moustaches ou boxplot). En ce qui concerne les variables qualitatives, l'inspection univariée se fera numériquement par la recherche de déséquilibres interclasses et graphiquement par une représentation des proportions de chaque classe via un diagramme en barre. 

C'est très bien tout ça, mais quel rapport avec nos outliers ? C'est simple, la présence d'outliers peut impacter la distribution de nos données et donc biaiser nos analyses (descriptives et inférentielles) à venir. Aussi, la capture et le traitement de ces points extrêmes est très importante. 

Concernant la mise à l'échelle logarithmique : 
- https://www.claret.ca/fr/publications/utiliser-des-echelles-logarithmiques-pour-les-graphiques-de-prix-dactions/
- http://www.parisschoolofeconomics.eu/docs/tenand-marianne/modeles-en-log.pdf

### Méthodes graphiques.
Bibliographie:
- https://www.jmp.com/fr_fr/statistics-knowledge-portal/exploratory-data-analysis/histogram.html

Par exemple, si vous éditez un histogramme vous pouvez avoir un premier aperçu de la distribution des données et de la présence d'outliers. Par exemple, des barres peuvent être distantes du reste de la distribution ou simplement les valeurs situés au niveau de la queue de l'histogramme. 

Supposons désormais l'usage d'une variable catégorielle. Comment visualiser un outliers ? En vous référant aux différentes modalités que peut prendre la variable : si un ou plusieurs individus prennent une valeur qui ne fait pas parti des possibles alors vous êtes en présence d'un point aberrant ; remplacer ce point par une valeur pertinente compte tenu de votre connaissance du métier o ude la abse de donnée - e.g. erreur de saisie - sinon utiliser une méthode d'imputation. Est-il possible d'observer chez une variable qualitative un point extrême ? Oui, toutefois, puisque c'est une variable qualitative, ce point fait parti du champ des possibles et doit être traité dans vos analyses, tout en gardant à l'esprit la nature particulière de ce point. 

### Méthodes par critère/numériques.
Par exemple, lorsque vous éditez un diagramme en boîte vous observez parfois des points qui sont à l'extérieur des moustaches. Ces points sont des outliers. Qu'est ce qui permet à un boxplot de définir un point comme loitain ? C'est l'écart interquartile. Plus spécifiquement, on considère qu'un point est lointain dès lors qu'il se situe à plus ou moins 1.5 fois l'écart interquartile - i.e. à l'extérieur des limites de nos fameuses moustaches. 

#### 1.5 fois l'écart interquartile.
Une fois votre boxplot éditer, vous observez un certain nombre d'outliers. Pour les retrouver, il vous suffit de rechercher l'ensemble des points situés à plus ou moins 1.5 fois l'écart interquatile de la distribution. Soit, 

$$ cutoff = 1.5*(Q3-Q1) $$

on obtient alors, $lower_bound = Q1 - cutoff$ et $upper_bound = Q3 + cutoff$. Tous les points situés au-dessus de l'upper_bound ou en-dessous du lower_bound seront considérés comme outliers. 

Cette méthode présente l'avantage de s'appliquer à des échantillons dont la distribution n'est pas Normale. 

#### Z score
Le fameux z-score! Il est partout décidément. En même temps, qu'est ce qu'il est pratique ! Pratique mais fragile... l'usage d'un tel score repose sur l'hypothèse d'une distribution normale de vos données. Aussi son usage doit être maitrisé pour ne pas conduire à des conclusions erronées. Pour rappel le score Z permet de décrire un individu statistique en fonction de son écart au centre de gravité de la distribution, et ce, en unité d'écart type. Ainsi, puisque vos données sont normalement distribuées il vous indique où se situe le point sur une distribution normale, de sorte que si un point est égale à 2, un peu plus de 95% des valeurs sont inférieur à lui. On va donc pouvoir fixer un seuil au-delà duquel les valeurs seront jugées comme lointaines. 

Mais quel valeur seuil choisir ? Puisque vos données sont normalement distribuées - au moins suivent approximativement une loi normale (cf. Théorème central limite ; pour un bref rappel, consultez cet [article](http://www.jybaudot.fr/Probas/tcl.html)) - vous savez que près de 68% (95%, 99.7) des valeurs de votre distribution sont situées à plus ou moins un (deux, trois) écart-type de la moyenne. Alors, il semble judicieux de choisir un seuil au moins égal à deux pour juger qu'une valeur est lointaine de la distribution. Ce "lointain" se définit comme l'ensemble des valeurs dont la probabilité d'être observé est faible. En fixant un seuil de 2 écart-type, le "lointain" sera l'ensemble des valeurs ayant une probabilité d'être observé inférieur à 5%.  Attention toutefois au placemeent de ce seuil, beaucoup d'article préconise de fixer une valeur de 3. 

Formellement, 

$$ Z = \frac{X - \bar{x}}{\sigma} $$
où, $\bar{x}$ et $\sigma$ sont la moyenne et l'écart-type de l'échantillon, respectivement. 

Quelques prudences à adopter lors du calcul du score Z : 
1. Vérifier la normalité de la distribution ; À ce sujet, lisez mon [article](https://github.com/JordanNSZ/statisserie/tree/main/Teststatistiques).
2. Le choix du seuil est décisif et impactera forcément le résultat. Il dépend du niveau de sensibilité souhaité ;
3. Le score suppose un "lointain" par rapport à la moyenne. Cela peut être problématique puisque la moyenne est sensible aux outliers : c'est un peu le serpent qui se mord la queue cette histoire. 


#### Robust Z score. 
Bibliographie.
- https://medium.com/@pelletierhaden/modified-z-score-a-robust-and-efficient-way-to-detect-outliers-in-python-b8b1bdf02593
- https://medium.com/towards-data-science/the-ultimate-guide-to-finding-outliers-in-your-time-series-data-part-1-1bf81e09ade4
-
Ce dernier point de prudence m'amène à vous proposer une autre mesure du "lointain" d'une distribution : le score Z modifié/robuste. Personnelement, je suis très prudent quant à l'usage de tehcnique quantitative qui repose sur la moyenne de la distribution ou plus généralement sur la distribution elle-même. En effet, ces techniques sont peu robustes en cela qu'elles sont très sensible aux outliers (les revoilà ceux-là!) et aux conditions d'application. Pour cette raison, il existe très souvent (pour ne pas dire toujours) une alternative robuste. Par exemple, dans le cadre des tests d'homogénéité de deux moyennes, une alternative au test t de Student existe (ou au test de Welch pour les connaisseurs ; pour les autres c'est par [ici](https://github.com/JordanNSZ/statisserie/tree/main/Tests%20d'homog%C3%A9n%C3%A9it%C3%A9)), le test de Wilcoxon-Mann-Withney. Ce dernier est robuste puisqu'il ne suppose pas de distribution des échantillons et qu'il s'intéresse à comparer chacune des distributions par leur médiane. En effet, la médiane à cela de beau qu'elle est difficilement tirée par les valeurs extrêmes donc plus robuste. 

Vous me voyez venir ? Le score Z robuste est simplement une alternative qui prend en compte la médiane plutôt que la moyenne. Mais, me direz-vous : si on prend en compte la médiane, il est abérrant de mesurer l'écart à celle-ci qui plus est en unité d'écart type ? Bien évidement ! On va donc définir une mesure de la variabilité de notre échantillon qui soit robuste, à savoir l'écart absolu à la médiane (a.k.a. MAD pour Median Absolut Deviation). Il s'agit de la distance moyenne entre les données et la médiane. Ainsi, on va pouvoir calculer l'écart individuel à la médiane en unité d'écart absolu à la médiane... complexe dit comme ca, voyons la formule. 

Pour trouver l'écart absolu médian, voici les étapes à suivre :

1. Calculez la médiane des données.

2. Soustrayez la valeur à la médiane.

3. Prenez la valeur absolue de cette différence.

4. Calculez la médiane de cette différences absolue.


Formellement, 

$$ MAD = med(| x_{i}-med(X) |) ,$$

avec $x_{i}$ la valeur d'un individu statistique pour la variable $X$. Nous avons le matériel nécessaire pour le calcul du score Z modifié.


Voici la formule :

$$ Z = 0.6745 \frac{(x_{i} - med(X))}{MAD}.$$

Vous observez le coefficient $0.6745$ qui permet d'approximer un équivalent médiane de l'écart-type. Je m'explique : en multipliant l'écart à la médiane en unité de MAD par le coefficient $\frac{1}{1.4826}$ on s'assure que la MAD sera approximativement équivalente (au moins asymptotiquement) à l'estimateur standard de l'écart-type. Ce processus va nous permettre de fixer un seuil en nous appuyant sur les quantiles de la distribution normale centrée-réduite et d'interpréter le score Z modifié comme le score Z. Par exemple, si un individu pour une variable donnée obtient un score de 2, plus de 95% des individus seront inférieur à lui. 


1. Les données peuvent ne pas être normalement distribuées ;
2. Les quantiles de la loi Normale centrée-réduite peuvent être utilisée pour fixer une valeur seuil ;
3. Le score Z robuste et le score Z sont comparables puisque définis sur la même échelle ;
4. Toutefois, les deux scores peuvent établir différent outliers. Cela est du au fait que la médiane est moins sensible aux valeurs extrêmes, donc il se peut que davantage de points se révèlent être des outliers avec le score Z modifié. 

Jusqu'alors nous avons évoqué les méthodes graphiques et numériques de détection d'outliers. Il subsite toutefois un doute quant à la significativité statistiques de ces métriques. Pour l'exprimer autrement, peut-on, pour un score Z donné, évaluer sa significativité statistique ?

### Méthode inférentielle.
La significativité statistique du score Z d'un point peut être évalué à l'aide d'un test d'hypothèse. Rappelons qu'un test d'hypothèse demande la mise en place d'un jeu d'hypothèses, d'une statistique de test et d'un risque de première espèce. 

Le jeu des hypothèses :
Hypothèse nulle - H0 : Il n'y a pas de valeur aberrante dans l'ensemble de données.
Hypothèse alternative - H1 : Il y a exactement une valeur aberrante dans l'ensemble de données. 

La statistique de test : 
$$ Z = \frac{X - \bar{x}}{\sigma}. $$

Il est clair que cette statistique de test suit une loi de Student à N-1 degrés de liberté. Une loi de student est le raport d'une variable normalement distribuée et la racine carré d'une variable distribuée d'après une loi du Khi-2. Une loi du Khi-2 est le rapport entre le carré d'une variable normalement distribuée et son nombre de degré de liberté $k$. 

Règle de décision : 
Si la probabilité d'observer la statistique de test sachant l'hypothèse nulle est supérieure en valeur absolue au seuil de significativité fixé $\alpha$, alors on pourra conclure que l'outlier est statistiquement significatif.

#### Significativité statistique du score Z maximum (ou minimum) - le test de Grubbs a.k.a. test résiduel normalisé maximum. 
    Bibliographie.
        - http://www.sediment.uni-goettingen.de/staff/dunkl/software/pep-grubbs.pdf 
        - https://www.statisticshowto.com/grubbs-test/
Néanmoins, il peut y avoir des scores Z lointain en bas et en haut de la distribution. Autrement dit, un score Z peut être considéré comme outlier si, en valeur absolue, il est supérieur à tous les autres. Il faut donc tester la significativité statistique du plus grand score en valeur absolue. 

Pour ce faire, un test existe : le test résiduel normalisé maxium, plus connu sous le nom de test de Grubbs. Grubbs est l'auteur de ce test statistique qui permet de mettre en lumière la significativité statistique du plus grand score en valeur absolue. Ce test est donc utiliser pour **détecter un unique outlier** à la fois. De plus, ce test suppose les données normalement distribuées. 

Avant d'appliquer ce test il faut donc vérifier la normalité de la distribution de l'échantillon ou que les données peuvent être approximées par une loi Noramle. 

Le jeu des hypothèses est le suivant : 
- H0 : Il n'y a pas de valeur aberrante dans l'ensemble de données.
- H1 : Il y a exactement une valeur aberrante dans l'ensemble de données.

La statistique de test est la suivante :
$$ G = \frac{max_{i\in N} |X_{i} - \bar{X}|}{s}$$
avec $s$ l'écart-type de l'échantillon et $\bar{X}$ sa moyenne. 

Règle de décision :
Si la statistique $G$ se situe au-dessus de la valeur critique $G_{critique}$ pour un risque $\alpha$ donné, on concluera à la présence significative d'un outlier. 

La valeur critique de G se calcul comme suit :

$$G_{critique} = \frac{N-1}{\sqrt{N}} \sqrt{\frac{t²_{\frac{\alpha}{2N}, N-2}}{N-2 + t²_{\frac{\alpha}{2N}, N-2}}}$$
avec $t²_{\frac{\alpha}{2N}}$ le valeur critique supérieure de la loi de student à N-2 degrés de liberté. 


## Traitement des points lointains. 
La présence de points lointains peut biaiser nos analyses. Particulièrement, ils étirent la moyenne ou l’écart-type à tort apportant ainsi un biais au calcul des estimateurs d’intérêt. Il faut donc les traiter et ce traitement dépendra de la nature du point. De deux choses l'une, soit le point est de nature aberrante -e.g. erreur de saisie-, soit il est de nature extrême -e.g. carectéristiques rare mais néanmoins réelles. Dans les deux cas, le point peut connaitre deux traitements : imputation ou suppression. L'affectation de l'un ou l'autre des traitements  nécessite une analyse descriptive préalable (univariée ou multivariée) afin de comprendre la nature de l'abérration ou de la spécificité. 

### Traitement des points aberrants.
#### Exploration de données pour imputation. 
##### Des cas particuliers : la connaissance métier. 
Par exmeple, une donnée aberrante pourrait être d'observer une valeur de zéro pour le CA d'un hotel. Peut être cette hotel est-il associé à cette valeur zéro puisqu'il n'a toujours pas réalisé de vente sur le site. Dans quel cas, cette valeur est certe extrême et aberrante mais elle décrit un phénomène important : les hotels inscrit sur la plateforme dont l'annonce n'a toujours pas donnée lieu à réservation ; soit parce qu'ils sont nouveaux, soit parce qu'ils peinent à attirer la demande client. Peu importe, il faudra donc conserver la donnée et vérifier la colinéarité avec une variable telle que la date d'inscription sur la plateforme ou le nombre de clients.

Autre exemple : lors du traitement de variables catégorielles de la base de données European Social Survey (8ème édition - 2016) j'ai observé des modalités qui ne faisaient pas parties du champs des possibles. Entre autres, j'observais un nombre important d'individus avec la modalité $7777$ alors qu'elle n'existait pas. Egalement, un grand nombre d'individus disposaient de la modalité $777$ pour qualifier les individus ayant refusé de répondre. Il était donc clair que la modalité $7777$ était une erreur de saisie ; j'ai donc pu imputer les données par moi-même. 

##### Des cas indescriptibles. 
Par exemple, supposons que votre variable quantitative relate un prix par nuité pour des logements Airbnb. L'un de ces biens a un tarif de 1 dollar par nuité. Il est évident que ce tarif ne fait pas sens. C'est pourquoi nous devons en déterminer les raisons. Peut-être l'annonce de ce bien airbn n'est pas aboutie, dans quel cas les variables "commentaire" ou "date de dernière location" seront potentiellement vides. Cette étape nous permet d'appréhender nos données et de mieux les comprendres. Néanmoins, dans le cadre d'une valeur aberrante il est parfois très difficile de comprendre la source de l'erreur pour imputer les données par nous même. 
Si nous ne parvenons pas à identifier la nature de l'erreur pour la remplacer par une valeur adéquate alors il faudra choisir : supprimer ou imputer la valeur à l'aide d'une méthode statistique -e.g. moyenne, médiane, etc. 

#### Suppression des outliers.
Dans le cas où les valeurs aberrantes ne seraient pas torp nombreuses et que les données fournies par les individus concernés n'apportent pas d'informations particulières, vous pouvez supprimer ces points de votre base de données. Toutefois, la suppression d'individus statistiques de votre base de données peut être délicate, notamment si vous n'avez pas beaucoup de données ou que les données concernées sont porteuses d'informations particulières pouvant être valorisées lors de vos analyses.  

Dans le cas où les valeurs aberrantes seraient nombreuses, une méthode d'imputation peut être idéale afin de remplacer ces valeurs. 
#### Imputation des outliers.
##### Imputation par paramètres de tendance centrale. 
Il est courant d'observer des travaux (même scientifiques) dans lesquels les valeurs manquantes et/ou les valeurs aberrantes sont remplacé par des paramètres de position. C'est une grave erreur et, je dois le dire, cette erreur à motiver ce travail. Encore une fois, gardez à l'esprit que la détection et le traitement des valeurs lointaines est nécessaire pour ne pas biaiser nos analyses : les outliers tirent la moyenne et l'écart-type, deux paramètres largement utilisés lors d'analyses inférentielles. Autrement dit, si vous chercher à diminuer l'impact de vos outliers sur votre distribution statistique, quelle idée d'aller à nouveau impacter celle-ci en imputant vos outliers avec la moyenne ou la médiane ? Il est donc clair qu'une imputation de valeurs aberrantes par la moyenne ou la médiane demande de vérifier l'impact qu'elle aura sur votre distribution. Combien même je ne recommande pas cette méthode, elle peut être intéressante à mettre en place afin d'arbitrer l'ampleur de l'impact d'une autre méthode d'imputation -e.g. les k plus proches voisins. (Bien sûr, cela vaut pour l'imputation des données manquantes). 


##### Imputation par KNN.
Une méthode d'imputation des données relativement protectrice à l'égard de la distribution est celle des K-Nearest Neighboors (KNN). Cette méthode remplace la valeur aberrante par la moyenne de ces k plus proches voisins. Ces k plus proches voisins sont ceux pour lesquelles ont observe des valeurs semblables hors valeurs manquantes. 

Pour résumer, trois méthodes sont à votre disposition pour traiter un point aberrant : trouver/comprendre la source de l'erreur, supprimer les points en question, changer leur valeur via une méthode d'imputation statistique. 


### Traitement des points extrêmes. 
En ce qui concerne le traitement des points extrêmes, la méthode d'imputaiton est à proscrire. De plus, puisque vous avez qualifié ces points d'extrêmes, vous en connaissez la raison -la source de cette extremité n'est plus à chercher. Aussi, compte tenu de votre connaissance métier, il vous faudra choisir entre retirer ces points de votre analyse ou les intégrer. En effet, leur intérêt pour l'analyse pêut être primordiale ou inexistant. Si vous réaliser une régression pour comprendre l'impcat du sport sur le poids chez les étudiants, il se peut que vous observiez des sportifs professionnels dont le temps consacré par semaine à la pratique est très élevé. Ces individus peuvent certes être jugés comme des points extrêmes mais ne doivent pas être remplacés , ni supprimés de votre base de données. Puisque l'objet de votre analyse inférentielle n'est pas de comprendre l'impact du sport chez les sportif de haut niveau mais plutôt chez les étudiants en générale, il faudra considérer de mener l'analyse sans et avec ces individus (ou une analyse séparé pour chacun des deux groupes). Cela vous permettra d'appréhender l'impact de ces points extrêmes sur votre régression. Particulièrement, si vous intégrez ces points, vous devez vous assurer qu'ils ne sont pas des points leviers, dans quel cas, ils entrainent un sur-ajustement du modèle et donc une estimation de l'impact du sport sur le poids qui serait trompeuse. 

#### Arbre de décision pour le traitement des valeurs aberrantes. 
![arbre de décison](/assets/img/decision_tree.png)

Merci d'avoir lu cette note ! J'espère qu'elle vous a plu. À bientot !














