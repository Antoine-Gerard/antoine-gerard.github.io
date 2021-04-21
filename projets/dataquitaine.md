---
layout: default
title: Projets
permalink: /projets/dataquitaine
---

# Le traitement Automatique des Langues au service du vin

![wia](/assets/images/winespace.png){:width="180"}

## Télécharger la présentation

PDF: [télécharger](/pdf/Dataquitaine.pdf)

## Résumé

### Introduction
Winespace souhaitait mener à bien un projet de recommandation de vins similaires basé sur l'étude de commentaires de dégustation. Pour cela, une collaboration entre Winespace et Inria a été mise en place. Côté Inria, elle impliquait les centres de recherche de Bordeaux et Paris, en particulier les équipes Transfert, Innovation et Partenariat et Service Expérimentation et Développement de Bordeaux et l'équipe ALMAnaCH de Paris.

### Méthodologie
Pour ce projet, nous avons mis en place des outils relevant du Traitement Automatique des Langues (TAL) appliqués au domaine du vin. Nous avons alors réalisé un algorithme de compréhension automatique des commentaires de dégustation, dans le but de permettre la mise en correspondance de vins dont les commentaires montrent qu'ils ont des caractéristiques communes, notamment à des fins de recommandation. Deux difficultés émanent alors de ce projet. La première est le caractère hétérogène des commentaires de dégustation, dont le contenu, le vocabulaire et le style diffèrent d'un nologue à l'autre. La deuxième difficulté est l'absence de données annotées : aucun commentaire d'nologue n'a été manuellement annoté pour y repérer les termes pertinents ou l'associer à des informations structurées. Réaliser de telles annotations serait coûteux, et le nombre élevé de termes rares rendrait la tâche ardue et le volume d'annotation à produire très élevé. En revanche, la disponibilité d'une première version d'ontologie du domaine, fournie par Winespace, permet de déployer des technologies symboliques et statistiques ne reposant pas sur l'apprentissage automatique. C'est donc ce choix que nous avons effectué. Il a pour autre avantage de fournir des modèles analysables, dont les résultats sont donc explicables, mais également améliorables (par exemple par l'amélioration progressive de l'ontologie).

### Originalité / perspective
Ce projet avait pour objectif la construction d'un système de recommandations de vins similaires basé sur les commentaires de dégustation en entrée. Après avoir réalisé une analyse sémantique des commentaires, chaque vin a été représenté par un vecteur « profil » permettant d'encoder tous les concepts présents dans celui-ci : arômes, acidité, amertume, ... L'algorithme mis en place est alors une preuve de concept prometteuse. L'amélioration de l'ontologie fournie par Winespace, ainsi que l'utilisation de nouvelles techniques d'analyse sémantique permettront sûrement de perfectionner cet outil, et ainsi permettre à Winespace d'analyser plus facilement les commentaires de dégustation. Cette preuve de concept ouvre également des perspectives prometteuses sur une meilleure compréhension du goût du vin.

## Revoir le live:
<iframe width="560" height="315" src="https://www.youtube.com/embed/sJ58fGOrv_I" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

# À propos - Winespace
Winespace est une start-up bordelaise développant des solutions d'aide à la vente pour les vendeurs de vins. Ces solutions reposent sur deux piliers : l'expertise "vin" de notre équipe (œnologie, sommellerie, marketing et commerce du vin, etc.) et des compétences poussées en informatique et en Intelligence Artificielle.

**Site:** [wia.wine](https://wia.wine)

# À propos de l'orateur

<img src="/assets/images/antoine_gerard.png" style="float:left;display:block;width:150px;padding:10px">

<div class="speaker_text" style="width:300px;">
<div class="speaker_title" style="font-size:22px;font-weight:900;margin-top:30px;">
  Antoine Gérard
</div>
<div class="speaker_subtitle" style="font-size:12px; margin-top: 10px;color:#4d81a6">
  Responsable R&amp;D
	<a href="https://www.linkedin.com/in/antoine-g%C3%A9rard-b8b10b125/" target="_blank">
	  <img src="/assets/images/linkedin.jpg" style="width:35px;padding:0px">
	</a>
</div>
</div>
