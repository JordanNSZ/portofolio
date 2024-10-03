---
layout: post
title: "Initialiser un environnement virtuel python avec l'interface en ligne de commande (bash)."
date: 2024-04-05
categories: ["Linux", "Bash"]
tags: ["Python", "Environnement virtuel", "venv"]
description: "Cet article décrit les étapes de l'ininitalisation et de la gestion d'un environnement virtuel python." 
---
# BASH : ENVIRONNEMENT VIRTUEL PYTHON.
Il est d’usage de créer un **environnement virtuel** pour chaque projet. Cela permet, de télécharger des packages spécifiques (versions particulières) pour le déroulement du projet. Également, cela permet aux participants du projet de pouvoir accéder facilement à l’environnement de travail pour installation locale sans risque de ne pas avoir localement la bonne version de package.

Cette note vise à introduire la **création**, la **suppression** et la **modification** des environnements virtuels Python avec l'interface de ligne de commande BASH. 

## Création d’un environnement virtuel. 
Vous aurez besoin du module venv de python ; vous devez l'installer via pip ou conda :

```bash
pip install venv
```
Ou via le répertoire apt :
```bash
sudo apt install python3-venv
```
Une fois cette installation faite, placez-vous dans le répertoire courant du projet – ou créer le, au besoin.

```bash
cd Bureau
mkdir mon_env_virt
```
puis créer votre environnement virtuel (EV) comme ceci : 

```bash
python3 -m venv env
source env/bin/activate 
deactivate (pour désactiver l’EV)
```
La première ligne de code demande à python de créer un environnement virtuel (-m venv) nommé "env". Par convention, on renomme les EV "env", cela rend l'activation, la desactivation et le changement d'EV plus simple. 
La seconde ligne permet d'activer cet environnement virtuel nommé "env". Une fois cette ligne de code exécutée vous remarquerez que votre chevron est précédé du nom de l'EV : ‘(env) ~’. Si c'est le cas, félicitation vous venez de créer et activer votre premier EV. 


L'environnement virtuel permet de créer un environnement de programmation propre à chaque projet afin, par exemple, d'éviter les conflits de packages et assurer à toutes les parties prenantes du projet de pouvoir répliquer les scripts sur leur machine, et ce sans erreurs.

Lorsqu'on crée un environnement virtuel on peut donc installer nos packages comme on a l'habitude de le faire :

```bash
pip install pandas>2.8.1
pip install selenium
...
```
Jusque là rien de bien méchant. Toutefois, comme on l'a mentionné, l'intérêt de l'environnement virtuel est aussi de pouvoir faire profiter nos collègues des modules nécessaire à la réalisation du projet. Il convient donc de **créer un fichier qui référencera l'ensemble de ces packages** et leur version. Ce fichier doit être renommé "**requirements.txt**". 
Commençons par la création du fichier de stockage des noms et versions des modules :

```bash
touch requirements.txt
```
Maintenant que ce fichier est créé,  vous pouvez y inscrir les noms des packages. Nous avons installé deux modules dans notre environnement virtuel : pandas et selenium, nous allons les ajouter à notre fichier "requirements.txt". Si vous souhaitez connaitre l'ensemble des paquets - et leurs dépendances - installer dans votre environnement virtuel saisissez la commande suivante : 

```bash
pip freeze

# ou 
# pip list
```
Alors pour remplir votre fichier requirements.txt, rien de plus simple :

```bash
pip freeze > requirements.txt
```

Désormais, vous disposez d’un fichier ‘**requirements.txt**’ contenant l’intégralité des paquets nécessaires au lancement du projet : pour installer les paquets il vous suffit de verser son contenu au sein de l’environnement.

```bash
pip install -r requirements.txt
```
Voilà, votre environnement virtuel est créé et prêt à l'emploi. 

> Notes. Vous n’avez **pas besoin de spécifier** à votre venv **les bibliothèques standard**s python comme **os** ou **re**.

## Supprimer un environnement virtuel. 
Si vous souhaitez supprimer un environnement virtuel existant, voici comment faire :

```bash
rm -r env/
```

**Bon environnement virtuel !**
