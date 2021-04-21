---
layout: default
title: "Développement"
permalink: /dev/
---

# Développement chez Winespace:

* Interface web de CRUD (Create Read Update Delete) développée avec Ruby on Rails
* Construction d'une API python permettant l'analyse de commentaires de dégustation
  (œnologie) grâce à des outils de traitement automatique de la langue (TAL):
  * Utilisation de la bibliothèque [Stanza](https://stanfordnlp.github.io/stanza/)
  * Utilisation de la bibliothèque [Flask](https://flask.palletsprojects.com/en/1.1.x/)



# Nemo

Voir la [page de Nemo](https://team.inria.fr/memphis/nemo/)

Nemo est un outil de simulation numérique destiné à la modélisation de problèmes
en mécanique des fluides. Le but de Nemo est de permettre à l'équipe
[Memphis](https://team.inria.fr/memphis/) de disposer d'un socle commun pour la
simulation de problèmes de CFD.

## Contributions dans Nemo:

* Implémentation de méthodes d'interpolation
* Calculs de gradient à l'aide d'une méthode volume finis
* Test de mise à l'échelle sur un cluster de calcul local
  [Plafrim](https://www.plafrim.fr/).

## Dépendances de Nemo

**Langage: C++**

* [CMake](https://cmake.org/download/)
* [Open MPI](https://www.open-mpi.org/)
* [Eigen](http://eigen.tuxfamily.org/index.php?title=Main_Page)
* [PETSc](https://www.mcs.anl.gov/petsc/download/index.html)
* [Bitpit](http://optimad.github.io/bitpit/)
* [googletest](https://github.com/google/googletest)

# CEPS (Cardiac ElectroPhysiology Simulator)

Voir la [page de CEPS](https://carmen.gitlabpages.inria.fr/ceps/)

CEPS est un outil de simulation numérique destiné la modélisation en
électrophysiologie cardiaque. Le but de CEPS est de permettre à utilisateur
d'implémenter "facilement" de nouvelles méthodes numériques ainsi que de
nouveaux modèles physiques.

## Contributions dans CEPS:

* Implémentation de modèles ioniques
* Implémentation d'un [modèle
  bicouche](https://hal.inria.fr/hal-01132905/document) [1]
* Implémentation de modèles d'assimilation de données (Filtrage de Kalman,
  observateurs de Luenberger) à l'aide de la bibliothèque logicielle
  [Verdandi](http://verdandi.sourceforge.net/)

## Dépendances de CEPS:

**Langage: C++**

* [CMake](https://cmake.org/download/)
* [Open MPI](https://www.open-mpi.org/)
* [Eigen](http://eigen.tuxfamily.org/index.php?title=Main_Page)
* [PETSc](https://www.mcs.anl.gov/petsc/download/index.html)
* [CxxTest](https://cxxtest.com/)
* [ParMETIS](http://glaros.dtc.umn.edu/gkhome/metis/parmetis/)
  ou [PTScotch](https://www.labri.fr/perso/pelegrin/scotch/)
* [VTK](https://vtk.org/)
* [HDF5](https://www.hdfgroup.org/downloads/hdf5/)

# Références:

1. Simon L., J. Bayer, Y. Coudière, J. Henry, H. Cochet, P. Jais, E. Vigmond, A
   bilayer model of human atria: mathematical background, construction, and
   assessment, in EP-Europace, 2014.
