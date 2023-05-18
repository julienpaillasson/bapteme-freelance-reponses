# Feedback1

---

## Evaluation d'ensemble

Bravo pour ton excellent boulot ! Voici quelques axes de perfectionnement et bonnes pratiques pour aller plus loin.

---

### Routes 

Pense à placer tes routes dans un fichier dédié app/routes.php (et à require ce fichier dans index.php).
Pour plus de clarté dans la lecture des routes, tu peux les regrouper par page plutôt que par fonction.

---

### Models

Pense à ajouter dans ta classe CoreModel les attributs (status par exemple) et méthodes communes à toutes les classes (save, update, insert) et à utiliser le caractère abstrait de la classe pour mutualiser le code redondant.

Pour tes traitements de formulaires (même sur un back-office auquel on accède par login et dont les accès sont verrouillés selon le rôle), n'oublie pas de préparer les requêtes SQL pour plus de sécurité (méthodes insert et update) et de lier les valeurs préparées, avant de les exécuter. Voir l'exemple ci-dessous, issu de la correction (méthode insert du Model Student). Ca permet d'éviter les injections SQL.

``` php
    $pdoStatement = $pdo->prepare($sql);

    $pdoStatement->bindValue(':firstname', $this->firstname, PDO::PARAM_STR);
    $pdoStatement->bindValue(':lastname', $this->lastname, PDO::PARAM_STR);
    $pdoStatement->bindValue(':status', $this->status, PDO::PARAM_INT);
    $pdoStatement->bindValue(':teacher_id', $this->teacher_id, PDO::PARAM_INT);

    $ok = $pdoStatement->execute();
```

*ProTip Gestion du temps* 
Tu n'as pas eu le temps de terminer le model AppUser. N'hésite pas à écrire ta première classe et la dupliquer, pour seulement modifier les détails des autres classes selon leurs attributs, car le code est ici quasi identique.

---

### Controllers

Pour gagner quelques lignes dans ton code et éviter de déclarer des variables inutiles quand tu appelles une méthode relative à une autre classe, au lieu de faire ceci :
``` php
    $teacher = new Teacher();
    $teachers = $teacher->findAll();
```

tu peux utiliser cette notation :
``` php
    $teachers = Teacher::findAll();
```

---

### Views

Bonne pratique : au-delà du contrôle des routes, modifier l'affichage de la navigation en fonction des rôles. Ainsi, l'utilisateur n'est pas tenté d'accéder à des pages qui lui sont interdites. Il n'y a qu'en tapant directement l'url de la page dans le navigateur qu'il peut rencontrer l'erreur 403.

Exemple à placer dans le template de la navigation, avant un élément de la liste réservé aux utilisateurs admin :
``` html
     <?php if (isset($_SESSION['userRole']) && $_SESSION['userRole'] == 'admin') : >
```
