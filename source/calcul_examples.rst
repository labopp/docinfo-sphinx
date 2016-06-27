.. highlight:: bash

Exemples de scripts Torque
==========================

.. _ExamplesTorque:

Commandes PBS
-------------

Afin de spécifier certains paramètres relatives à l'exécution d'un
calcul, on utilise l'entame ``#PBS``. Il existe une `liste
exhaustive <http://www.clusterresources.com/torquedocs21/usersmanual.shtml>`_
de paramètres à spécifier de cette façon. Nous résumons ici les plus
couramment utilisées :

- Pour spécifier le nom du job (ici ``monjob``) ::

    #PBS -N monjob

  Valeur par défaut : donnée par le serveur Torque.

- Pour spécifier une file d'attente en particulier (ici ``q7jours``) ::

    #PBS -q q7jours

  Valeur par défaut : q1jour.

- Pour spécifier un temps maximal d'exécution (au format hh:mm:ss) ::

    #PBS -l walltime=01:00:00

  Valeur par défaut : la durée maximale d'exécution associée à la file d'attente spécifiée.

- Pour spécifier les ressources souhaitées :

  - Pour spécifier la mémoire vive nécessaire pour l'ensemble dun job (exprimée en bytes (b), kilobytes (kb), megabytes (mb), ou en gigabytes (gb)) ::

      #PBS -l mem=1gb

  - Pour spécifier la mémoire vive nécessaire par processeur ::

      #PBS -l pmem=1gb

  - Pour spécifier le nombre de noeuds de calcul utilisés ::

      #PBS -l nodes=2

    Valeur par défaut : 1 noeud.

  - Pour spécifier le(s) nom(s) du(des) noeud(s) de calcul souhaité(s) (ici ``mathcalc3`` et ``mathcalc4``) ::

    #PBS -l nodes=mathcalc3+mathcalc4

  Valeur par défaut : c'est le serveur Torque qui choisit.

  - Pour spécifier le nombre de processeurs par noeud souhaité (ici 2 processeurs) ::

      #PBS -l ppn=2

    Valeurs par défaut : 1 processeur par noeud.

  - On peut concaténer ces différents paramètres en spécifiant par exemple le nombre de processeurs selon le serveur sur lequel on lance les calculs.
    Par exemple, pour lancer un calcul sur 3 processeurs de ``mathcalc3`` et 4 processeurs de ``mathcalc7`` en précisant que l'on alloue une mémoire vive pouvant aller jusqu'à 1Gb par processeur et que la durée maximale autorisée est d'une heure, on écrit ::

      #PBS -l nodes=mathcalc3:ppn=3+mathcalc7:ppn=4,pmem=1gb,walltime=01:00:00

- Pour spécifier le fichier ASCII qui va contenir tout ce qui s'afficherait dans le terminal en mode frontal ::

    #PBS -o output.dat

  Valeur par défaut : monjob.out, monjob étant le nom du job que l'on spécifie.

- Pour spécifier le fichier ASCII qui va contenir les éventuels messages d'erreur ::

    #PBS -e error.dat

  Valeur par défaut : monjob.err, monjob étant le nom du job que l'on spécifie.

- Pour fusionner les fichiers de sortie et d'erreurs ::

    #PBS -j oe

  Valeur par défaut : désactivé.

- Pour envoyer des alertes mail :

  - Lorsque l'exécution du job commence ::

      #PBS -m b

  - Lorsque l'exécution du job est terminée ::

      #PBS -m e

  - Lorsque l'exécution du job est interrompue ::

      #PBS -m a

  - On peut combiner ces paramètres d'alerte mail. Par exemple, pour envoyer un mail au début et à la fin de l'exécution, on précise ::

      #PBS -m be

  - Préciser le destinataire ::

      #PBS -M prenom.nom@math.univ-lille1.fr

    Par défaut, le serveur Torque n'envoie pas d'alertes mail.

Exemples de scripts Torque
--------------------------

Dans les scripts Torque présentés ci-dessous, les lignes surlignées en
bleu correspondent à des instructions Unix à modifier pour adapter le
script à un autre calcul.

Exemple 1 : exécuter un code séquentiel
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

On souhaite compiler et exécuter en mode batch le programme `HelloWorld <files/HelloWorld.f90>`_ écrit en Fortran 90.
C'est un calcul très court, requierant très peu de mémoire.
On se contentera donc de préciser dans le script Torque le nom du job, le serveur utilisé, les fichiers de sortie et d'erreur, ainsi que les alertes mails ::

  #!/bin/bash
  ### On specifie le nom du job
  #PBS -N HelloWorld
  ### On specifie le serveur sur lequel on souhaite lancer le calcul, mathcalc4 par exemple
  #PBS -l nodes=mathcalc4
  ### On precise le nom des fichiers de sortie et d'erreur
  #PBS -o sortie.dat
  #PBS -e erreur.dat
  ### On precise l'adresse mail à laquelle seront envoyees les alertes mail
  #PBS -M toto@math.univ-lille1.fr
  ### On souhaite une alerte mail au debut et à la fin de l'execution, ainsi qu'en cas d'interruption du job
  #PBS -m bae
   
  ### On se place dans le repertoire où le fichier HelloWorld.f90 est situe
  cd ~/test_Torque/test_HelloWorld
  ### On compile le code Fortran
  gfortran HelloWorld.f90 -o HelloWorld
  ### On lance l'executable
  ./HelloWorld

Exemple 2 : exécuter un code séquentiel avec gestion de fichiers de résultats
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

On souhaite résoudre l'équation de Poisson 2D avec une méthode de
différences finies d'ordre 2 sur mailage cartésien. Le code séquentiel
pour résoudre un tel problème est disponible à `ce
lien <files/Poisson_BCGStab.tar.gz>`_. Si on considère un maillage fin,
le calcul peut s'avérer coûteux en temps de calcul, en mémoire vive et
en espace disque. Il faut donc que le calcul soit lancé depuis un
répertoire ``/scratch``. Le script Torque pour lancer un tel job est
donc le suivant ::

  #!/bin/bash
  ### On specifie le nom du job
  #PBS -N Poisson_BCGStab
  ### ### On specifie la file d'attente
  #PBS -q q7jours
  ### On specifie la duree maximale du job (48 heures ici), le serveur choisi et la memoire vive maximale necessaire
  #PBS -l walltime=48:00:00,nodes=mathcalc7,mem=1gb
  ### On precise le nom des fichiers de sortie et d'erreur
  #PBS -o sortie.dat
  #PBS -e erreur.dat
  ### On precise l'adresse mail à laquelle seront envoyees les alertes mail
  #PBS -M toto@math.univ-lille1.fr
  ### On souhaite une alerte mail au debut et à la fin de l'execution, ainsi qu'en cas d'interruption du job
  #PBS -m bae
  ### On se place dans le repertoire où les sources du codes sont situees
  cd ~/test_Torque/test_Poisson
  ### On copie le contenu du repertoire dans un repertoire qu'on cree au prealable dans /scratch
  mkdir /scratch/monlogin/test_Poisson
  cp -r . /scratch/monlogin/test_Poisson/
  ### On se place dans le repertoire nouvellement cree
  cd /scratch/monlogin/test_Poisson
  ### On compile le code
  make
  ### On execute le code
  ./poisson.seq

Exemple 3 : exécuter un code parallèle avec gestion de fichiers de résultats
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Un dernier exemple est consacré à l'exécution d'un calcul parallèle où
chaque processeur peut être amené à générer des fichiers. Ici, nous nous
contenterons d'un `HelloWorld <files/HelloWorldMPI.f90>`_ programmé en
MPI-Fortran dans lequel chaque processeur écrit son propre fichier.
Etant donné qu'il est possible d'impliquer plusieurs serveurs pour ce
genre de calcul, l'idée consiste à ::

  #!/bin/bash
  ### On specifie le nom du job
  #PBS -N HelloWorldMPI
  ### On specifie la file d'attente
  #PBS -q q1jour
  ### On specifie la duree maximale du job (24 heures ici)
  #PBS -l walltime=24:00:00
  ### On specifie la memoire vive, les serveurs, et le nombre de processeurs pour chaque serveur
  #PBS -l nodes=mathcalc3:ppn=4+mathcalc4:ppn=3,mem=2gb
  ### On precise le nom des fichiers de sortie et d'erreur
  #PBS -o sortie.dat
  #PBS -e erreur.dat
  ### On precise l'adresse mail à laquelle seront envoyees les alertes mail
  #PBS -M toto@math.univ-lille1.fr
  ### On souhaite une alerte mail au debut et à la fin de l'execution, ainsi qu'en cas d'interruption du job
  #PBS -m bae
  ### On se place dans le repertoire où les sources du codes sont situees
  cd ~/test_Torque/test_HelloWorldMPI
  ### On compile le code
  mpif90 -o HelloWorldMPI HelloWorldMPI.f90
  ### On execute le code
  mpirun -np 7 ./HelloWorldMPI
  ### On copie l'ensemble du contenu vers un repertoire /scratch à travers une connexion SSH
  ssh mathcalc4 "mkdir /scratch/monlogin/test_HelloWorldMPI"
  ssh mathcalc4 "cp -r ~/test_Torque/test_HelloWorldMPI/* /scratch/monlogin/test_HelloWorldMPI/"
  ### On detruit les fichiers de resultats dans le /home
  rm -rf *.dat

#.  Compiler et exécuter le code dans le ``/home``,
#.  Déplacer les fichiers produits dans un répertoire ``/scratch`` :
    cette étape est un peu délicate car il est difficile de savoir à l'avance quel serveur va effectuer le calcul, donc on ne sait pas dans quel ``/scratch`` les fichiers de résultats seront déposés à la fin du job. Pour être certain de ce répertoire, on fait le transfert de fichiers avec la commande ``scp``.
