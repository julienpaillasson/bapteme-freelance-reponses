# Feedback4

---

## Evaluation d'ensemble

Bon travail ! Tu as besoin de consolider quelques notions pour gagner du temps, notamment à propos du modèle MVC. Voici quelques axes de perfectionnement et bonnes pratiques pour aller plus loin.

---

### Copier /coller et commentaires

Attention, dans tes copier / coller, à bien veiller à changer tous les éléments pour qu'ils correspondent au projet. De même lorsque tu dupliques un commentaire. S'il n'est pas exécuté, le commentaire fait tout de même partie intégrante de ton code, en ce qu'il le rend plus lisible et compréhensible pour un futur lecteur (peut-être toi-même plus tard). La maintenance future du code dépend en partie de la qualité des commentaires.

---

### Routes 

Tu as choisi de passer sur tes routes de formulaires plusieurs options de type, ce qui permet à première vue d'écrire moins de code. Cependant, il vaut mieux séparer afin de donner à chaque route une seule responsabilité (affichage des données reçues via la requête en GET ou traitement des données envoyées via la requête en POST). En plus, ça te permet d'associer chaque route à la méthode correspondante et de suivre la convention de nommage vue en cours.

Exemple :
``` php
    $router->map(
        'GET',
        '/students/add',
        [
            'method' => 'add',
            'controller' => $controllersNamespace . 'StudentController'
        ],
        'student-add'
    );
    $router->map(
        'POST',
        '/students/add',
        [
            'method' => 'addPost',
            'controller' => $controllersNamespace . 'StudentController'
        ],
        'student-add-post'
    );
```

Tu peux jeter un œil à la doc d'[Altorouter](https://altorouter.com/usage/mapping-routes.html) ici.

N'hésite pas aussi à jeter un œil à la classe vendor/benoclock/Dispatcher.php pour comprendre comment les routes sont traitées. Ca te permettra de comprendre d'où viennent tes erreurs d'appel de classe.

*Bonnes pratiques*
> Crée une route par type de requête.
> Pense à placer tes routes dans un fichier dédié app/routes.php (et à require ce fichier dans index.php).

---

### Models

*Classe abstraite*
> Pense à ajouter dans ta classe CoreModel tous les attributs (status par exemple) et méthodes communes à toutes les classes (delete) et à utiliser le caractère abstrait de la classe pour mutualiser le code redondant.

*Interactions avec la base de données*
> N'oublie pas de préparer les requêtes SQL pour plus de sécurité (méthodes insert et update) et de lier les valeurs préparées, avant de les exécuter. Voir l'exemple ci-dessous, issu de la correction (méthode insert du Model Student). Ca permet d'éviter les injections SQL.

``` php
    $pdoStatement = $pdo->prepare($sql);

    $pdoStatement->bindValue(':firstname', $this->firstname, PDO::PARAM_STR);
    $pdoStatement->bindValue(':lastname', $this->lastname, PDO::PARAM_STR);
    $pdoStatement->bindValue(':status', $this->status, PDO::PARAM_INT);
    $pdoStatement->bindValue(':teacher_id', $this->teacher_id, PDO::PARAM_INT);

    $ok = $pdoStatement->execute();
```

---

### Controllers

*CoreController*
> C'est le premier controller que tu devrais créer, puisqu'il va être utilisé par toute l'app.
> A ne pas confondre avec le MainController, qui correspond à l'accueil du site.

*ErrorController*
> Il est aussi important de créer rapidement le controller d'erreur, qui te sert à rediriger vers la page erreur 404. D'autant plus que tu l'appelles avec public/index.php en utilisant le module Dispatcher, ce qui cause une erreur PHP (la classe est introuvable) et empêche par conséquent l'affichage du site.

```php
    $dispatcher = new Dispatcher($match, '\App\Controllers\ErrorController::err404');
```


---

### Views

*Découpage html*

> Attention au découpage du html, à ne pas dupliquer des éléments. Par exemple, le bloc de texte suivant apparait dans tes templates home.tpl.php et nav.tpl.php. Tu l'aurais vu bien sûr si tu avais pu afficher le site en supprimant les erreurs de classe manquante.

```html
    <div class="container my-4">
        <p class="display-5">
            Bienvenue dans le backOffice <strong>d'une école 100% en ligne formant des développeurs Web</strong>...
        </p>
    </div>
```

*Affichage de la navigation*
> Tu utilises bien la superglobale $_SESSION pour observer l'état de l'utilisateur et afficher le lien de déconnexion dans la nav. Dans ce cas, il est toutefois préférable d'afficher la vue de connexion et uniquement celle-ci dans le cas où aucun utilisateur n'est connecté. Dans la correction, ceci est réalisé dans le CoreController dans la méthode checkAuthorization().

``` php
    public function checkAuthorization($roles = [])
    {
        // Si le user est connecté
        if (!empty($_SESSION['connectedUser'])) {
            // Alors on récupère l'utilisateur connecté
            $connectedUser = $_SESSION['connectedUser'];

            // Puis on récupère son role
            $userRole = $connectedUser->getRole();

            // si le role fait partie des roles autorisées (fournis en paramètres)
            if (in_array($userRole, $roles)) {
                // Alors on retourne vrai
                return true;
            } else { // Sinon le user connecté n'a pas la permission d'accéder à la page
                // => on envoie le header "403 Forbidden"
                // Puis on affiche la page d'erreur 403
                $this->err403();
                // Enfin on arrête le script pour que la page demandée ne s'affiche pas
                exit;
            }
        } else { // Sinon, l'internaute n'est pas connecté à un compte
            // Alors on le redirige vers la page de connexion
            // Pour cela on a besoin du router
            global $router; // toujours moche, mais on fait avec :(
            header('Location: ' . $router->generate('appuser-signin'));
            exit;
        }
    }
```


