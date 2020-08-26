---
layout: default
title: "Développement"
permalink: /dev/
---

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

**Language: C++**

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
