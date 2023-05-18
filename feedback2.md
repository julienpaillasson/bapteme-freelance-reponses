# Feedback2

---

## Evaluation d'ensemble

Bon travail ! Tu as besoin de consolider quelques notions pour gagner du temps, notamment à propos des routes. Voici quelques axes de perfectionnement et bonnes pratiques pour aller plus loin.

---

### Routes 

Il manque pas mal de routes à ton projet, notamment toutes celles concernant les formulaires. Tu devrais revoir cette partie du cours pour mieux te familiariser avec les types de route. 

Tu peux aussi jeter un œil à la doc d'[Altorouter](https://altorouter.com/usage/mapping-routes.html).

*Bonne pratique*
> Pense à placer tes routes dans un fichier dédié app/routes.php (et à require ce fichier dans index.php).

---

### Models

Pense à ajouter dans ta classe CoreModel les attributs (status par exemple) et méthodes communes à toutes les classes (save, update, insert) et à utiliser le caractère abstrait de la classe pour mutualiser le code redondant.

*Gestion du temps* 
> Tu n'as pas eu le temps de traiter le module AppUser, ni de traiter les rôles. N'hésite pas à écrire ta première classe et la dupliquer, pour seulement modifier les détails des autres classes selon leurs attributs. N'hésite pas à dupliquer le code des projets précédents, tel qu'indiqué dans la consigne (projet oShop). 

---

### Controllers

*Bonne pratique*
> Attention à l'indentation de ton code. Très important pour permettre une relecture plus fluide, aussi bien pour toi que pour les autres développeurs qui liront ton code. Ton IDE / éditeur de texte propose sûrement des options de configuration ou des extensions pour auto-indenter lors de la sauvegarde par exemple. D'une manière générale, prends le temps de bien configurer ton poste de travail, tu gagneras beaucoup de temps pour développer !

---

### Views

Dans l'ensemble, tes découpages du code html sont corrects, mais manquent parfois un peu de précision. Par exemple, le bouton d'ajout a sauté sur tes listes de profs et d'étudiants. Du coup, tu as placé la route d'ajout sur le bouton de modification, ce qui est un peu maladroit d'un point de vue UI. 

*Astuce*
> Si tu n'es pas à l'aise pour repérer les éléments html, copie tout le code html dans ta vue, affiche-la et retire les éléments dont tu n'as pas besoin progressivement. Attention à bien retirer les balises correspondantes en haut et en bas du code. Pour cela, il faut que l'indentation soit impeccable !
