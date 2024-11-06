---
layout: default
title: About Me
---

<div class="post">
	<h1 class="pageTitle"></h1>
	<p class="intro">Data scientist, passionné par la statistique et la science des données, j'utilise Python, R et SQL pour répondre aux besoins des entreprises. <br> Mon objectif est de transformer les données en informations à forte valeur ajoutée pour accompagner la prise de décision .</p>
	
	
	<h1 class="pageTitle" style="font-size: 35px;">Mon CV</h1>

	<p> Si vous souhaitez accéder à une version détaillée de mon cv, consulter ce <a href="{{ site.baseurl }}/assets/pdf/linkedin.pdf"> pdf </a> ou rendez-vous sur <a href="https://www.linkedin.com/in/jordannagadzina-sanchez"> <b>linkedin</b> </a>. </p>

    <h1 class="pageTitle" style="font-size: 35px;">Mes Projets</h1>

<h2>Statisserie.</h2>
<p> J'ai récemment entrepris le développement de ma <b>chaîne dédiée à la statistique</b>. L'objectif est de rendre disponible mes articles en statistiques, et plus largement, en analyse de données. Aussi, certains posts sont dédiés à l'usage de divers langages de programmation pour l'analyse de données tels que <b>SQL, python et R</b>. Certaines analyses statistiques telles que l'Anova à un facteur ou les tests d'homogénéité sont réalisées à la fois avec python et avec R.


<h2>Data scraping - Extraction de données à partir de centaines de fichiers pdf.</h2>
<p> Ce projet permet d'<b>extraire les données textuelles et tabulaires</b> contenues dans un <b>document pdf</b> de plusieurs centaines de pages. Entre autres choses, il explore la variété des méthodes offertes par le module <b>pdfPlumber</b> (python) pour l'extraction d'informations depuis un document pdf tel qu'une facture. En outre, il itère sur un grand nombre de documents afin de recueillir les informations demandées. De plus, à titre d'exemple, le dictionnaire de la base de données European Social Survey (8ème édition, 446 pages) est utilisé afin d'extraire les informations nécessaires à la transformation et au nettoyage des données permettant de faciliter de futures analyses. </p> 
<li> compétences : extraction de données depuis un pdf.</li>
<li> outils : python, pdfPlumber, regular expressions (RegExp).</li>
		 
<h2>Extraction de données d'annonces immobilières - ELT et ETL.</h2>
<p> Mon intention est d'extraire et rendre disponible des données exploitables pour diverses analyses inférentielles et exploratoires ainsi que pour la rédaction de rapports et de tableaux de bords. Pour ce faire j'ai déployé <b>deux processus d'extraction et d'ingestion de données</b> : ETL et ELT. </p>

<h4> Extraction</h4>
<p>L'<b>extraction des données</b> est effectuée avec le module <b>selenium de python</b>. Notamment, le programme demande de renseigner deux paramètres : la ville et le nombre d'annonces immobilières à extraire. Afin de prévenir les problèmes de détection, j'utilise un algorithme de <b>rotation d'adresse IP</b> permettant le changement de l'adresse IP à chaque nouvelle page extraite.</p>
  
<ul><h5>Chargement et transformation - Processus ELT.</h5></ul>
<p> Une fois les données extraites, l'utilisation d'un <b>ORM (SQLAlchemy)</b> permet le chargement des données dans un <b>entrepôt de données PostgreSQL</b>. </p>
<p> Finalement, <b>Data Build Tool</b> (DBT) est utilisé pour transformer et nettoyer les données au sein de l'entrepôt posgtreSQL. </p>
<li> compétences : extraction de données depuis un site web ; manipulation et transformation de données.</li>
<li> outils : python, BeautifulSoup, selenium, RegExp, SQLAlchemy, postgreSQL, DBT.</li>
		    
<ul><h5>Transformation et chargement - Processus ETL.</h5></ul>
<p> Une fois les données extraites, l'utilisation du module <b>pandas de python</b> permet de transformer et nettoyer les données. </p>
<P> Enfin, les données sont chargées dans <b>BigQuery</b>, la solution cloud d'entrepôt de données de <b>Google Cloud Plateform</b>. </p>

<li> compétences : extraction de données depuis un site web ; manipulation et transformation de données.</li>
<li> outils : python, BeautifulSoup, selenium, RegExp, GCP, BigQuery, SQL.</li>


</div>
