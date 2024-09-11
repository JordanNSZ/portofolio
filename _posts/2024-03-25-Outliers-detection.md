---
layout: post
title: "Détection des points extrêmes : comment les reprérer, les qualifier et les traiter ?"
date: 2024-03-25
description: La détection des so-called "outliers" est primordiale pour mener des analyses inférentielles non-biaisées. Cet article explore les méthodes de détection, de qualification et de traitement des points lointains. Une étude de cas est proposée avec l'ensemble de données Kaggle 'Airbnb'. 
---

Un **point extrême**, plus connu sous le nom d'**outlier** ou d'**anomalie**, est une observation distante des autres observations de la distribution. Ces points peuvent être de deux natures, soit ils sont le résultat de la variabilité propre au phénomène étudier, soit ils sont dus à une erreur de saisie. Dans le premier cas je parles de **points atypiques** puisqu'ils ont du sens pour le phénomène observé. Dans le second cas, je parlerais de **points aberrants** en cela qu'ils n'ont pas de sens vis-à-vis du phénomène observé.  

Nous le savons, certains paramètres statistiques (la moyenne) sont **sensibles aux valeurs extrêmes**. Ainsi, ne pas détecter, qualifier et traiter ces points peut conduire à modifier à tort les paramètres statistiques de la distribution. Dans la mesure où ces paramètres - la moyenne et l'écart-type, par exemple - sont utilisés dans de nombreuses analyses inférentielles, **nos résultats seront biaisés** puisque l'estimation faite s'éloignera significativement de la vrai valeur du paramètre à estimer. Également, ces valeurs extrêmes peuvent contraindre les modèles à s'ajuster à elles, **créant un phénomène de surapprentissage** (surajustement).

Dans ce qui suit nous allons voir **comment détecter, qualifier et traiter ces points extrêmes**. En effet, savoir qualifier la nature de l'anomalie est important afin d'adapter notre **stratégie de traitement**. Entre autres, nous verrons qu'un point atypique peut être important pour les analyses statistiques et donc qu'il peut être utile de le conserver dans vos analyses - dans l'industrie, cela peut révéler une défaillance du processus de production ; en finance et en assurance, aider à la détection de fraudes. Nous développerons donc différentes stratégies de traitement des anomalies. À la fin de l'exposer vous trouverez un arbre de décision pour la détection et le traitement des outliers en fonction de leur nature. Finalement, nous présenterons une étude de cas en nous appuyant sur les données AirBnB disponible sur le site [Kaggle.Com](https://www.kaggle.com/datasets/arianazmoudeh/airbnbopendata).

## Détection d'outliers.
La détection d'outlier est très importante avant la mise en place d'une analyse inférentielle (statistique, économétrie) ou d'un algorithme d'apprentissage automatique. En effet, la présence de points lointains peut biaiser le résultat des analyses ou impacter la performance d'un modèle (d'apprentissage automatique). Pour cette raison, leur détection ne doit pas être minorée et elle doit être réalisée dans les premières phases du projet - cf. Analyse des données exploratoire, **analyse univariée**.
La **qualification** d'un point en tant que **point extrême** -relativement aux autres points d'une variable- peut avoir **deux explications**. Soit, le point est **aberrant**, soit le poit est **atypique**. Avant de revenir sur la nature de ces points il nous faut les détecter. Pour cela plusieurs techniques existes - graphiques, numériques et inférentielles.

Pour illustrer l'exposé je vais utiliser les données Airbnb disponible sur Kaggle. Cet ensemble de données sera également utilisé lors de l'étude de cas. 

## Détection de points lointains.
La **détection d'outliers** se fait dès le début du projet d'analyse de données. Autrement dit, au moment de l'**analyse descriptive univariée**. Lors de cette analyse vous allez vous intéresser à la forme de la distribution de vos variables quantitative. Notamment, vous allez consulter les valeurs de certains paramètres statistiques clés tels que les paramètres de tendance centrale (*étendue, Q1, Q2, Q3, Q4, moyenne*) et ceux de dispertion (*variance, écart-type*). Cette première analyse de la distribution vous donnera un premier aperçu de l'état de votre distribution, par exemple, si elle est assymétrique ($moyenne - mediane \neq 0$)[^1]. De pluys, on utilisera aussi un histograme et/ou un diagramme en boîte (a.k.a. boîte à moustaches ou boxplot) pour vérifier visuellement la distribution de nos données.

C'est très bien tout ça, mais quel rapport avec nos outliers ? C'est simple, ces paramètres de distribution et ces deux graphiques vont nous aider à détecter les outliers.

Concernant la mise à l'échelle logarithmique :
https://www.claret.ca/fr/publications/utiliser-des-echelles-logarithmiques-pour-les-graphiques-de-prix-dactions/
http://www.parisschoolofeconomics.eu/docs/tenand-marianne/modeles-en-log.pdf

### Méthodes graphiques.
Bibliographie:
https://www.jmp.com/fr_fr/statistics-knowledge-portal/exploratory-data-analysis/histogram.html

Par exemple, si vous éditez un histogramme vous pouvez avoir un premier aperçu de la distribution des données et de la présence d'outliers. En outre, des barres peuvent être distantes du reste de la distribution ou vous pouvez simplement constater la présence de valeurs qui étirent les queues de l'histogramme. Observez plutôt l'histogramme suivant.

On remarque nettement la présence d'outliers dans la queue inférieure de la distribution. Il existe deux points qui se distingue particulièrement des autres. Qui plus est, les queues de la distribution semblent étirées par quelques observations. Nous allons pouvoir vérifier ce point grâce à un diagramme en boîte.

### Méthodes par critère/numériques.
Le diagramme en boîte ou boîte à moustaches, communément appelé boxplot, est une solution graphique qui permet de visualiser la distribution d'une variable quantitative et d'isoler les outliers. En effet, lorsque vous éditez un diagramme en boîte vous observez parfois **des points qui sont à l'extérieur des moustaches**. Ces points sont des outliers. *Qu'est ce qui permet à un boxplot de définir la longueur des moustaches ?* C'est l'écart interquartile. Plus précisémment, **le critère de Tukey**. Ainsi, on considérera qu'un point est extrême dès lors qu'il se situe à plus ou moins 1.5 fois l'écart interquartile.

Vérifions l'analyse faite à la lecture de l'histogramme.

On peut bien confirmer la présence de nombreux outliers d'après le critère de Tukey. D'une part, deux points sont extrêmes dans la partie inférieure de la distribution, et, d'autre part, de nombreux points étirent anormalement la distribution des données.

#### Le critère de Tukey.
Une fois votre boxplot éditer, vous observez un certain nombre d'outliers. Pour les retrouver, il vous suffit de rechercher l'ensemble des points situés à plus ou moins 1.5 fois l'écart interquatile de la distribution. Soit,

$$ cutoff = 1.5*(Q3-Q1) $$

on obtient alors, $lower_bound = Q1 - cutoff$ et $upper_bound = Q3 + cutoff$. Tous les points situés au-dessus de l'upper_bound ou en-dessous du lower_bound seront considérés comme outliers.
Cette méthode présente l'avantage de s'appliquer à des échantillons dont la distribution n'est pas Normale.

#### Z score
Le fameux **z-score**! Il est partout décidément. En même temps, qu'est ce qu'il est pratique ! Pratique mais fragile... l'usage d'un tel score repose sur l'hypothèse d'une **distribution normale de vos données**. Aussi son usage doit être maitrisé pour ne pas conduire à des conclusions erronées. Pour rappel le score Z permet de **décrire un individu statistique en fonction de son écart au centre de gravité de la distribution, et ce, en unité d'écart type**. Ainsi, puisque vos données sont normalement distribuées il vous indique où se situe le point sur une distribution normale, de sorte que si un point est égale à 2, un peu plus de 95% des valeurs sont inférieur à lui (en valeur absolu). On va donc pouvoir fixer un **seuil au-delà duquel les valeurs seront jugées comme lointaines** du reste de la distribution car peu probable d'être observées normalement.

Mais quel valeur seuil choisir ? Puisque vos données sont normalement distribuées - au moins approximativement (cf. Théorème central limite ; pour un bref rappel, consultez cet [article](http://www.jybaudot.fr/Probas/tcl.html)) - vous savez que **près de 68% (95%, 99.7%) des valeurs de votre distribution sont situées à plus ou moins un (deux, trois) écart-type de la moyenne**. Il semble donc judicieux de choisir un seuil au moins égal à deux pour juger qu'une valeur est lointaine de la distribution. Ce "lointain" se définit comme l'ensemble des valeurs dont la probabilité d'être observé est faible. En fixant un seuil de 2 écart-type, le "lointain" sera l'ensemble des valeurs ayant une probabilité d'être observé inférieur à 5%.  Attention toutefois au placemeent de ce seuil, beaucoup d'article préconise de fixer une valeur de 3.

Formellement le score-Z est égal à,

$$ Z = \frac{X - \bar{x}}{\sigma} $$
où, $\bar{x}$ et $\sigma$ sont, respectivement, la moyenne et l'écart-type de l'échantillon.

Nous avons utiliser le score Z pour détecter d'éventuelles anomalies dans notre distribution. Il en ressort qu'un certain nombre d'observations ont un score supérieur à 2. Notamment, les deux observations situées dans la queue inférieure gauche de la distribution. Lorsque le seuil est fixé à 3, seules ces deux observations sont suspectées d'anomalie.

Quelques prudences à adopter lors du calcul du score Z :
1. Vérifier la normalité de la distribution ; À ce sujet, lisez mon [article](https://github.com/JordanNSZ/statisserie/tree/main/Teststatistiques).
2. Le choix du seuil est décisif et impactera forcément le résultat. Il dépend du niveau de sensibilité souhaité ;
3. Le score suppose un "lointain" par rapport à la moyenne. Cela peut être problématique puisque la moyenne est sensible aux outliers : c'est un peu le serpent qui se mord la queue cette histoire.

#### Robust Z score.

Bibliographie.
- https://medium.com/@pelletierhaden/modified-z-score-a-robust-and-efficient-way-to-detect-outliers-in-python-b8b1bdf02593
- https://medium.com/towards-data-science/the-ultimate-guide-to-finding-outliers-in-your-time-series-data-part-1-1bf81e09ade4

Ce dernier point de prudence m'amène à vous proposer une autre mesure du "lointain" d'une distribution : le score Z modifié/robuste.

Personnelement, je suis très prudent quant à l'usage de tehcnique quantitative conditionnelle à la distribution des données - i.e. **tests paramétriques**. En effet, ces techniques sont peu robustes en cela qu'elles sont très sensible aux outliers (les revoilà ceux-là!) et aux défauts de normalités - i.e. asymétrie, par exemple. Pour cette raison, et puisqu'il existe très souvent (pour ne pas dire toujours) **une alternative robuste**, il peut être prudent d'utiliser une **alternative non-paramétrique** en complément. Par exemple, dans le cadre des tests d'homogénéité de deux moyennes, une alternative au test t de Student existe, le test de Wilcoxon-Mann-Withney. Ce dernier est robuste puisqu'il ne suppose pas de distribution des échantillons et qu'il compare les distributions d'après les rangs de leurs observations.

Vous me voyez venir ? Le **score Z robuste** est simplement une alternative qui prend en compte la médiane plutôt que la moyenne. Mais, me direz-vous : si on prend en compte la médiane, mesurer l'écart à celle-ci qui plus est en unité d'écart type n'a pas de sens ? Bien évidement ! On va donc définir une mesure de la variabilité de notre échantillon qui soit robuste, à savoir l'**écart absolu à la médiane** (**MAD** pour Median Absolut Deviation). Il s'agit de la **distance moyenne entre les données et la médiane**. Ainsi, on va pouvoir calculer l'écart individuel à la médiane en unité d'écart absolu médian à la médiane... complexe dit comme ca, voyons la formule.

Pour trouver l'**écart absolu à la médiane**, voici les étapes à suivre :

1. Calculez la médiane des données.

2. Soustrayez la médiane à la valeur.

3. Prenez la valeur absolue de cette différence.

4. Calculez la médiane de cette différences absolue.

> La MAD est donc définie comme la médiane des écarts absolus par rapport à la médiane des données.

Formellement,

$$ MAD = med(| x_{i}-med(X) |) ,$$

avec $x_{i}$ la valeur d'un individu statistique pour la variable $X$. Nous avons le matériel nécessaire pour le calcul du score Z modifié.

Voici la formule du score Z modifié :

$$ Z = 0.6745 \frac{(x_{i} - med(X))}{MAD}.$$

Vous observez le coefficient $0.6745$ qui permet d'**approximer un équivalent médiane de l'écart-type**. Je m'explique : en multipliant l'écart à la médiane en unité de MAD par le coefficient $\frac{1}{O.67449} \approx 1.4826$ on s'assure que la MAD sera approximativement équivalente (au moins asymptotiquement) à l'estimateur standard de l'écart-type pour une distribution normale. Ce processus va nous permettre de **fixer un seuil** en nous appuyant sur les **quantiles de la distribution normale centrée-réduite** et d'interpréter le score Z modifié comme le score Z. Par exemple, si un individu pour une variable donnée obtient un score de 2, plus de 95% des individus seront inférieur à lui en valeur absolue.

Avantages du score Z modifié/robuste :
1. Les données peuvent ne pas être normalement distribuées ;
2. Les quantiles de la loi Normale centrée-réduite peuvent être utilisée pour fixer une valeur seuil ;
3. Le score Z robuste et le score Z sont comparables puisque définis sur la même échelle ;
4. Toutefois, les deux scores peuvent établir différent outliers. Cela est du au fait que la médiane est moins sensible aux valeurs extrêmes, donc il se peut que davantage de points se révèlent être des outliers avec le score Z modifié. 

Dans l'exemple qui suit nous avons calculé le score Z modifié. Le nombre de points extrêmes est concordant avec les résultats du score Z. Il semblerait donc que nos analyses graphiques soient pertinentes.

Jusqu'alors nous avons évoqué différentes méthodes de détection d'outliers sans évoquer la réalisation de tests statistiques. La prochaine section présente un test statistique très connu en détection d'anomalies, le **test de Grubbs**.

### Méthode inférentielle - le test de Grubbs.
La significativité statistique du score Z d'un point peut être évalué à l'aide d'un test d'hypothèse. Il se peut qu'un outlier soit présent à chacune des extrémités de la distribution. Aussi, il convient de établir la **significativité statistique du score Z le plus élevé** en valeur absolue. Pour ce faire, on peut utiliser le test de Grubbs (1950) ; il est utilisé pour **détecter un unique outlier** à la fois. De plus, ce test suppose les données normalement distribuées.

Rappelons qu'un test d'hypothèse demande la mise en place d'un jeu d'hypothèses, d'une statistique de test et d'un risque de première espèce.
Le jeu des hypothèses :
**Hypothèse nulle - H0** : Il n'y a pas de valeur aberrante dans l'ensemble de données.
**Hypothèse alternative - H1** : Il y a exactement une valeur aberrante dans l'ensemble de données (le maximum ou le minimum). 

La **statistique de test** : 
$$ Z = \frac{max_{i \in N}(x_i - \bar{x_i})}{\sigma}. $$

Il est clair que cette statistique de test suit une **loi de Student à N-1 degrés de liberté**. Une loi de student est le rapport d'une variable normalement distribuée et la racine carré d'une variable distribuée d'après une loi du Khi-2. Une loi du Khi-2 est le rapport entre le carré d'une variable normalement distribuée et son nombre de degré de liberté $k$.

Règle de décision :
- Dans le cadre d'un test bilatéral.
Si la probabilité d'observer la statistique de test sous l'hypothèse nulle est supérieure en valeur absolue au seuil de significativité fixé $\alpha / 2$, alors on pourra conclure que l'outlier est statistiquement significatif.

#### Le test de Grubbs - application.
Bibliographie.
- http://www.sediment.uni-goettingen.de/staff/dunkl/software/pep-grubbs.pdf 
- https://www.statisticshowto.com/grubbs-test/

Avant d'appliquer ce test il faut vérifier la normalité de la distribution de l'échantillon ou nous assurer que les données peuvent être approximées par une loi Noramle. DAns notre cas, les données contiennent suffisament d'observation pour appliquer le Théorème Central Limite.

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
avec $t²_{\frac{\alpha}{2N}, N-2}$ le valeur critique supérieure de la loi de student à N-2 degrés de liberté pour un risque $\alpha$ donné.

D'après le test que nous avons conduit on peut bien conclure à la présence d'un outlier dans notre ensemble de données. Il s'agit du point minimum de la distribution. Toutefois, jusqu'alors, nous avons suspecté la présence de nombreux outliers, il nous faudrait donc une procédure itérative afin de détecter chacun d'entre eux. Une procédure itérative du test de Grubbs est développé dans le document d'application fournit avec cet article. Vous pouvez le [consultez ici]().

## Traitement des points lointains. 
La présence de points lointains peut biaiser nos analyses. Particulièrement, ils étirent la moyenne ou l’écart-type à tort apportant ainsi un biais au calcul des estimateurs d’intérêt. Il faut donc les traiter et ce traitement dépendra de la nature du point. De deux choses l'une, soit le point est de nature aberrante -e.g. erreur de saisie-, soit il est de nature atypique -e.g. carectéristiques rare mais néanmoins réelles. Dans les deux cas, le point peut connaitre deux traitements : imputation ou suppression. L'affectation de l'une ou l'autre des méthodes nécessite une analyse descriptive préalable (univariée ou multivariée) afin de comprendre la nature de l'abérration ou de la spécificité.

### Traitement des points aberrants.
#### Exploration de données pour imputation. 
##### Des cas particuliers : la connaissance métier.
Dans de nombreux projets scientifiques nous sommes confronté à la présence d'anomalies. Particulièrement, j'ai rencontré différent cas d'école important lors de l'analyse des données de l'Enquête Sociale Européenne. J'aimerai revenir sur deux exemples qui permettront d'illustrer la nécessité de recourir à une analyse descriptive approfondie afin de qualifier la nature du point aberrant. 

Premièrement, plusieurs individus déclarait employer $7'777$ personnes. Toutefois, après vérification ces individus étaient, pour certains, sans emplois ou retraités. Il s'est avéré que la valeur '777' permettait de cibler les individus n'ayant pas répondu. En outre, la connaissance métier - la connaissance de la base de données - et le recours à une analyse descriptive approfondie ont permis d'établir la nature de ces points extrêmes : il s'agissait de points aberrants, dont la valeur n'avait aucun sens. Il a donc suffit de remplacer cette valeur par la valeur cible '777'.

Deuxièmement, une variable décrit le temps passé en minute et par jour à consulter les actualités et les journaux. Cette variable présentait également des valeurs anormalement hautes : plusieurs centaines d'individus déclaraient passer au moins 1000 minutes par jour à consulter les actualités et journaux de leur pays ; le maximum de la distribution était à 1428 minutes. Ce chiffre est un non sens complet : 1428 minutes équivaut à 23,8 heures. Soit ils sont bionique, soit quelque chose cloche. J'opte pour la seconde option ;) Il n'en demeure pas moins qu'il faut comprendre d'où peut provenir cette erreur de saisie pour pouvoir la traiter. Après quelques recherches, et vu la généralisation du problème, j'en conclus qu'il s'agit d'une erreur de transfère de données entre les questionnaires et la base de données. En effet, dans les questionnaires, la réponse devait être complété sous le format "HH-MM". De plus, vu la quantité d'individus concernés et l'impossibilité de décrire la façon dont ces erreurs de saisies sont intervenues, je décide d'imputer les données. Combien même la base de données comporte plus de 37 000 individus, il est préférable de ne pas perdre l'information que pourrait apporter ces données.

##### Des cas indescriptibles. 
Par exemple, supposons que votre variable quantitative relate un prix par nuité pour des logements Airbnb. L'un de ces biens a un tarif de 1 dollar par nuité. Il est évident que ce tarif ne fait pas sens. C'est pourquoi nous devons en déterminer les raisons. Peut-être l'annonce de ce bien airbn n'est pas aboutie, dans quel cas les variables "commentaire" ou "date de dernière location" seront potentiellement vides. Une analyse descriptive approfondie ne nous permet pas dans ce cas précis d'apporter plus d'informations. L'annonce semble être régulièrement visité et le bien régulièrement loué. Autrement dit, on ne connait pas la cause de cette absurdité.

Aussi, dans le cadre d'une valeur aberrante il est parfois très difficile de comprendre la source de l'erreur pour imputer les données par nous même.
Si nous ne parvenons pas à identifier la nature de l'erreur pour la remplacer par une valeur adéquate alors il faudra choisir : supprimer ou imputer la valeur à l'aide d'une méthode statistique -e.g. moyenne, médiane, etc ou algorithme non-supervisé.

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

[^1]:Pour rappel, si le coefficient est inférieur à $0$, asymétrie à gauche ; si le coefficient est supérieur à $0$, asymétrie à droite.