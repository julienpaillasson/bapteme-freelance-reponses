# Feedback3

---

## Evaluation d'ensemble

Bon travail ! Tu as besoin de consolider quelques notions pour gagner du temps, notamment à propos des routes et du traitement des formulaires. Voici quelques axes de perfectionnement et bonnes pratiques pour aller plus loin.

---

### Session

N'oublie pas d'initialiser la session utilisateur, en haut du fichier Public/index.php.

Elle te servira à observer l'état de l'utilisateur et à gérer les redirections, notamment lors de la déconnexion de celui-ci.

```php
    session_start();
```

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

*Bonnes pratiques*
> Crée une route par type de requête.
> Pense à placer tes routes dans un fichier dédié app/routes.php (et à require ce fichier dans index.php).

---

### Models

Pense à ajouter dans ta classe CoreModel les attributs (id, status par exemple) et méthodes communes à toutes les classes (save, update, insert...) et à utiliser le caractère abstrait de la classe pour mutualiser le code redondant.

*Gestion du temps* 
> Tu n'as pas eu le temps de traiter le module AppUser, ni de traiter les rôles. N'hésite pas à écrire ta première classe et la dupliquer, pour seulement modifier les détails des autres classes selon leurs attributs. N'hésite pas à dupliquer le code des projets précédents, tel qu'indiqué dans la consigne (projet oShop). 

---

### Controllers

*CoreController*
> C'est le premier controller que tu devrais créer, puisqu'il va être utilisé par toute l'app.
> A ne pas confondre avec le MainController, qui correspond à l'accueil du site.
> Pense à inclure les éléments partagés par toutes les pages (header, footer...) dans la fonction show() de ton CoreController, pour t'assurer de les afficher partout.

Voir ci-dessous, la structure html générale est incluse à la fin de la méthode show(), cela implique dans cet exemple de créer les templates header et footer en plus des templates de chaque vue. 
``` php
    protected function show(string $viewName, $viewVars = [])
    {
        // On globalise $router car on ne sait pas faire mieux pour l'instant
        global $router;

        // Comme $viewVars est déclarée comme paramètre de la méthode show()
        // les vues y ont accès
        // ici une valeur dont on a besoin sur TOUTES les vues
        // donc on la définit dans show()
        $viewVars['currentPage'] = $viewName;

        // définir l'url absolue pour nos assets
        $viewVars['assetsBaseUri'] = $_SERVER['BASE_URI'] . 'assets/';
        // définir l'url absolue pour la racine du site
        // /!\ != racine projet, ici on parle du répertoire public/
        $viewVars['baseUri'] = $_SERVER['BASE_URI'];

        // On veut désormais accéder aux données de $viewVars, mais sans accéder au tableau
        // La fonction extract permet de créer une variable pour chaque élément du tableau passé en argument
        extract($viewVars);
        // => la variable $currentPage existe désormais, et sa valeur est $viewName
        // => la variable $assetsBaseUri existe désormais, et sa valeur est $_SERVER['BASE_URI'] . '/assets/'
        // => la variable $baseUri existe désormais, et sa valeur est $_SERVER['BASE_URI']
        // => il en va de même pour chaque élément du tableau

        // $viewVars est disponible dans chaque fichier de vue
        require_once __DIR__ . '/../views/layout/header.tpl.php';
        require_once __DIR__ . '/../views/' . $viewName . '.tpl.php';
        require_once __DIR__ . '/../views/layout/footer.tpl.php';
    }
```

*Valeurs par défaut des formulaires :*
>  Pour pouvoir afficher des valeurs préremplies de manière dynamique, il faut les récupérer dans une méthode de Controller, qui sera associée à une route en GET.

Exemple avec l'ajout d'érudiant, on récupère d'abord tous les enseignants :
``` php
    public function add()
    {
        $teachers = Teacher::findAll();
        $this->show(
            'student/add-update',
            [
                'teachersList' => $teachers,
                'student' => new Student(),
            ]
        );
    }
```

---

### Views

*Structure de la page*
> Comme vu plus haut, pense à inclure les éléments partagés par toutes les pages (header, footer...) dans la fonction show() de ton CoreController, pour t'assurer de les afficher partout (voir ci-dessus) et éviter de dupliquer le code html.

*Bouton retour*
> De la même manière que tu le fais avec l'attribut "action" de tes formulaires, tu peux associer une route à l'attribut "href" des boutons.

Exemple : 
``` html
    <a href="<?= $router->generate('student-list') ?>" class="btn btn-success float-right">Retour</a>
```

*Affichage des formulaires*
> Pense à remplacer les options d'input par les valeurs dynamiques reçues dans la requête en GET

Exemple :
``` html
    <div class="form-group">
        <label for="teacher">Prof</label>
        <select name="teacher" id="teacher" class="form-control">
            <option value="0">-</option>
            <?php foreach ($teachersList as $currentTeacher) : ?>
            <option value="<?= $currentTeacher->getId() ?>"<?php if ($currentTeacher->getId() == $student->getTeacherId()) : ?> selected<?php endif ?>><?= $currentTeacher->getFirstname() ?> <?= $currentTeacher->getLastname() ?> - <?= $currentTeacher->getJob() ?></option>
            <?php endforeach ?>
        </select>
    </div>
```

