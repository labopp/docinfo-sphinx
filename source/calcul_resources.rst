Ressources en Calcul
====================

Le laboratoire met à disposition des moyens de calculs permettant de réaliser des simulations numériques de problèmes issus des mathématiques, souvent en interaction avec la physique, la biologie, la finance... L'objectif principal de ces moyens de calcul n'est pas de rivaliser avec les super-ordinateurs du `Top 500 <http://www.top500.org/>`__ mais de permettre à ceux qui le souhaitent de développer et de tester leurs codes de simulations avant un éventuel portage sur des machines beaucoup plus puissantes.

Tout membre du laboratoire souhaitant bénéficier de ces ressources de calcul peut faire une demande d'ouverture de compte en envoyant un email à `sysadmin <mailto:sysadmin@math.univ-lille1.fr?subject=demande%20de%20création%20de%20compte%20sur%20mathcalc>`__.

Il est rappelé que **la machine Argos n'est pas destinée au calcul** même si certains outils comme Matlab ou Maple y sont présents. Argos est destinée à l'ensemble des membres du laboratoire afin de permettre un accès à Internet et à des outils informatiques tels que LaTeX, Thunderbird etc...

Présentation des ressources
---------------------------

Les ressources proposées sont constituées de 5 serveurs de calcul nommés ``mathcalc2``, ``mathcalc3``, ``mathcalc4``, ``mathcalc7`` et ``mathcalc8``. Bien qu'il s'agisse de 5 serveurs différents, la partition ``/home`` est commune à l'ensemble de ces machines. Cependant, elles disposent chacune d'un répertoire séparé ``/scratch`` destiné à stocker des fichiers inutilisés afin de délester la partition ``/home``.

.. csv-table::
    :header: "Serveur","Nombre de CPUs","Nombre de coeurs","Cadence de chaque coeur","Taille du cache","Espace disque","Mémoire vive"

    "mathcalc2","8 x Intel Xeon X5680","12","3.33 GHz","12 Mo","402 Go","32 Go"
    "mathcalc3","8 x Intel Xeon X5355","8","2.66 GHz","4 Mo","??","13 Go"
    "mathcalc4","8 x Intel Xeon X5460","8","3.16 GHz","6 Mo","476 Go","30 Go"
    "mathcalc7","12 x Intel Xeon X5680","24","3.33 GHz","12 Mo","502 Go","52 Go"
    "mathcalc8","12 x Intel Xeon X5680","24","3.33 GHz","12 Mo","502 Go","54 Go"

Le serveur ``mathcuda`` embarque une carte graphique `Nvidia Tesla C2050 <http://www.nvidia.fr/object/product_tesla_C2050_C2070_fr.html>`__ (448 coeurs CUDA cadencés à 1.15 GHz, 3 Go GDDR5) entièrement dédié au développement de codes programmés en langage CUDA© et au calcul sur processeur graphique. Le répertoire ``/home`` de ``mathcuda`` est indépendant de celui des ``mathcalc``.

Le répertoire ``/home`` des serveurs de calcul est indépendant de celui d'Argos.

Ressources logiciels
--------------------

L'ensemble des serveurs de calcul du laboratoire fonctionnent actuellement avec le système d'exploitation `Ubuntu 12.04.3 Server Edition <http://www.ubuntu.com/server>`__.

- Compilateurs : gfortran 4.6.3, ifort (Intel-Fortran), gcc 4.6.3, gcc34, icc (Intel-C), g++ 4.6.3, g++34, mpicc, mpif90, python, python2.7, javac, ...
- Librairies : Intel MKL, FFTW 3, Suitesparse 3.6.1-4, Scalapack 1.7.5, Silo 4.6.1, HDF5 1.8.5, ...
- Logiciels : Matlab 2012, Scilab 5.3.1, gmsh 2.5.1, R 2.15.2, sage 5.11, visit 2.6.3, FreeFem++, gimp 2.6, gnuplot 4.4, octave 3.2.4, paraview 3.14.1, ...

Utilisation des ressources
--------------------------

Afin de lancer un calcul, il est possible de le faire «en frontal», c'est-à-dire en tapant directement la commande associée dans le terminal comme par exemple

$ gcc HelloWorld.c -o HelloWorld$ ./HelloWorld   HelloWorld

Cependant, si l'utilisateur décide de lancer un calcul dont il ne connaît ni le temps d'exécution ni la mémoire requise, ni l'espace disque requis, cela peut provoquer de sérieux problèmes sur les serveurs si le calcul lancé s'avère particulièrement gourmand.

.. Warning:: Seuls les calculs de l'ordre de quelques minutes, requérant peu de mémoire, et non répétitifs peuvent être lancés en mode frontal. Les autres calculs doivent être lancés en :ref:`mode batch <BatchMode>`.
