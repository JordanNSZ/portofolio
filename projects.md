---
layout: default
title: Projects
---

<div class="post">
	<h1 class="pageTitle"></h1>

	<p class="intro">Vous trouverez sur cette page mes projets récents en ingénierie et science des données. <b>Bonne lecture !</b>
	<p> Si vous avez des questions ou des commentaires,  <a href="mailto:jordan.nagadzina.sanchez@gmail.com">contactez-moi</a>.</p>

<h2>Projets en cours</h2>
<h3>Prédiction du prix des loyers dans les cinq plus grandes villes de France</h3>

<p> Ce projet fait suite au projet d'extraction de données d'annonces immobilières. Mon objectif est de développer une application (API REST) afin de rendre disponible la prédiction du prix des loyers dans les cinq plus gandes villes françaises. En outre, ce modèle prendra en compte des éléments structurels - extraits des annonces - et des éléments contextuels tels que la mise en place de politique d'encadrement des loyers. Finalement, l'application sera destinée à fournir une interface cartographique afin de visualiser - pour un quartier ou un arrodissement - le prix moyen des loyers au mètre carré ainsi que d'autres indicateurs. </p>
<p></p>
<p></p>
<p></p>
<h2>Projets récents</h2>
<h3>Statisserie.</h3>
<p> J'ai récemment entrepris le développement de ma <b>chaîne dédiée à la statistique</b>. L'objectif est de rendre disponible mes articles en statistiques, et plus largement, en analyse de données. Aussi, certains posts sont dédiés à l'usage de divers langages de programmation pour l'analyse de données tels que <b>SQL, python et R</b>. Certaines analyses statistiques telles que l'Anova à un facteur ou les tests d'homogénéité sont réalisées à la fois avec python et avec R.

<h3>Data scraping - Extraction de données à partir de centaines de fichiers pdf.</h3>
<p> Ce projet permet d'<b>extraire les données textuelles et tabulaires</b> contenues dans un <b>document pdf</b> de plusieurs centaines de pages. Entre autres choses, il explore la variété des méthodes offertes par le module <b>pdfPlumber</b> (python) pour l'extraction d'informations depuis un document pdf tel qu'une facture. En outre, il propose d'itérer sur un grand nombre de documents afin de recuillir les informations demandées. À titre d'exemple, le dictionnaire de la base de données European Social Survey (8ème édition, 446 pages) est utilisé afin d'extraire les informations nécessaires à la transformation et au nettoyage des données permettant de faciliter de futures analyses. </p> 
<li> compétences : extraction de données depuis un pdf.</li>
<li> outils : python, pdfPlumber, regular expressions (RegExp).</li>
		 
<h3>Extraction de données d'annonces immobilières - ELT et ETL.</h3>
<p> Mon intention est d'extraire et rendre disponible des données exploitables pour diverses analyses inférentielles et exploratoires ainsi que pour la rédaction de rapports et de tableaux de bords. Pour ce faire j'ai déployé deux processus d'extraction et d'ingestion de données : ETL et ELT. </p>

<h4> Extraction</h4>
<p>L'<b>extraction des données</b> est effectuée avec le module <b>selenium de python</b>. Notamment, le programme demande de renseigner deux paramètres : la ville et le nombre d'annonces immobilières à extraire. Afin de prévenir les problèmes de détection, j'utilise un algorithme de <b>rotation d'adresse IP</b> permettant le changement de l'adresse IP à chaque nouvelle page extraite.</p>
  
<h5>Chargement et transformation - <b>Processus ELT</b>.</h5>
<p> Une fois les données extraites, l'utilisation d'un <b>ORM (SQLAlchemy)</b> permet le chargement des données dans un <b>entrepôt de données PostgreSQL</b>. </p>
<p> Finalement, <b>Data Build Tool</b> (DBT) est utilisé pour transformer et nettoyer les données au sein de l'entrepôt posgtreSQL. </p>
<li> compétences : extraction de données depuis un site web ; manipulation et transformation de données.</li>
<li> outils : python, BeautifulSoup, selnium, RegExp, SQLAlchemy, postgreSQL, DBT.</li>
		    
<h5>Transformation et chargement - <b>Processus ETL</b>.</h5>
<p> Une fois les données extraites, l'utilisation du module <b>pandas de python</b> permet de transformer et nettoyer les données. </p>
<P> Enfin, les données sont chargées dans <b>BigQuery</b>, la solution cloud d'entrepôt de données de <b>Google Cloud Plateform</b>. </p>

<li> compétences : extraction de données depuis un site web ; manipulation et transformation de données.</li>
<li> outils : python, BeautifulSoup, selnium, RegExp, GCP, BigQuery, SQL.</li>


