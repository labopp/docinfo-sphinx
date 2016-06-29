Pages personnelles
==================

Pour créer une page personnelle, 2 méthodes très différentes sont possibles:

-  sans HTML, édition simplifiée dans Drupal directement avec l'éditeur
   intégré: on s'authentifie sur le site, on crée un contenu (menu *Contenu* >
   *ajouter du contenu* -> *Basic Page*), on l'enregistre, on note l'URL, et
   on la copie dans la son profil dans le champs « site/page web ».

-  on crée des pages HTML standard: dans l'espace utilisateur « labo », avec
   son compte labo, on crée si besoin un répertoire nommé ``public_html``.
   Dans ce répertoire on place sa/ses page(s) html, dont une se nomme
   obligatoirement ``index.html``. Une fois les pages créées, la commande
   ``put_www`` (un script maison qui simplifie l'usage de rsync) prend le
   contenu de ce répertoire pour le copier intégralement dans l'espace
   équivalent sur le serveur web.
