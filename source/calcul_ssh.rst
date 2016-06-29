.. highlight:: bash

Connexion SSH
=============

Il est nécessaire de posséder un compte pour accéder aux ``mathcalc`` et à
``mathcuda``. Ce compte est indépendant de celui que chaque membre du
laboratoire possède sur ``argos``. Pour en faire la demande, il suffit
d'envoyer un mail à `sysadmin
<mailto:sysadmin@math.univ-lille1.fr?subject=demande%20de%20création%20de%20compte%20sur%20mathcalc>`_.

Depuis le réseau privé du laboratoire
-------------------------------------

Ouvrir un terminal et taper la commande suivante ::

  ssh monlogin@mathcalcN

N étant le numéro du serveur voulu. Pour une connexion à ``mathcuda``, il suffit de remplacer ``@mathcalcN`` par ``@mathcuda``.

Depuis une machine Linux ou Mac OS extérieure au laboratoire
------------------------------------------------------------

Méthode avec rebond en 2 lignes sans mode graphique :
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

#.  Ouvrir un terminal et se connecter sur ``argos`` pour entrer dans le
    réseau privé du laboratoire ::

      ssh -p 8524 monlogin@argos.univ-lille1.fr

    .. Warning::

       L'option ``-p 8524`` est obligatoire (il s'agit du numero de port ssh).

    .. Note:: 

       Il n'est pas possible de lancer une application graphique sur
       **argos**. Par conséquent, cette méthode ne permet pas non plus de
       lancer des applications graphiques sur les serveurs de calcul.

#.  Une fois connecté sur ``argos``, taper la commande suivante ::

      [monlogin@cyprus ~]$ ssh monlogin@mathcalcN

   ``N`` étant le numéro du serveur voulu. Pour une connexion à ``mathcuda``,
   il suffit de remplacer ``@mathcalcN`` par ``@mathcuda``.

Méthode avec rebond en une ligne sans mode graphique :
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Il suffit d'ouvrir un terminal et taper la commande ::

  ssh -t -p 8524 monlogin@argos.univ-lille1.fr ssh monlogin@mathcalcN

``N`` étant le numéro du serveur voulu. Pour une connexion à ``mathcuda``, il
suffit de remplacer ``@mathcalcN`` par ``@mathcuda``.

.. Note::

   Il n'est pas possible de lancer une application graphique sur ``argos``.
   Par conséquent, cette méthode ne permet pas non plus de lancer des
   applications graphiques sur les serveurs de calcul.

Méthode avec modification de la liste des serveurs hôtes (avec mode graphique) :
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

#.  Étape préliminaire: inscrire les serveurs dans la liste des serveurs hôtes
    connus par votre machine. Pour cela, il faut ouvrir le fichier
    ``~/.ssh/config`` avec un éditeur de texte et ajouter les lignes suivantes ::

      Host mathcalc2
      ProxyCommand ssh -p 8524 argos.univ-lille1.fr nc %h %p
      Host mathcalc3
      ProxyCommand ssh -p 8524 argos.univ-lille1.fr nc %h %p
      Host mathcalc4
      ProxyCommand ssh -p 8524 argos.univ-lille1.fr nc %h %p
      Host mathcalc7
      ProxyCommand ssh -p 8524 argos.univ-lille1.fr nc %h %p
      Host mathcalc8
      ProxyCommand ssh -p 8524 argos.univ-lille1.fr nc %h %p
      Host mathcuda
      ProxyCommand ssh -p 8524 argos.univ-lille1.fr nc %h %p

    ``N`` étant le numéro du serveur voulu. Pour une connexion à ``mathcuda``,
    il suffit de remplacer ``@mathcalcN`` par ``@mathcuda``.

#.  Ouvrir un terminal et taper la commande ::

      ssh -X monlogin@mathcalcN

    pour se connecter au serveur ``mathcalcN``.

Depuis une machine Windows extérieure au laboratoire
----------------------------------------------------

#.  Se connecter à ``argos`` en utilisant Putty (voir la procédure
    :ref:`décrite ici <AccesWindows>`).

#.  Une fois connecté à ``argos``, entrer la commande ::

      [monlogin@cyprus ~]$ ssh monlogin@mathcalcN

    pour se connecter au serveur ``mathcalcN``.
