# La méthode javascript fetch()

## Présentation

La méthode fetch() de javascript est une méthode de l'API Fetch.
Cette API permet de réaliser des requêtes HTTP. 
Pour ça, elle définit deux types d'objets, *Request*, qui permet de réaliser une requête, et *Response*, qui détermine ce que l'on peut recevoir en retour.

La méthode fetch() sert à récupérer cette requête formatée selon la norme de l'objet Request et à interpréter ce qu'elle retourne. C'est une méthode asynchrone, c'est-à-dire qu'elle interragit avec le serveur distant et attend en retour une *promise* sous la forme de l'objet Response.

A la différence de la méthode jQuery.ajax(), la méthode fetch() retournera toujours quelque chose, même en cas d'erreur http (404 ou 500 par exemple). 

## Exemple dans ton code
Sur le formulaire de signin par exemple, en ajoutant un attribut id "signin-form" sur la balise form et un attribut onsubmit pour capturer l'événement de soumission du formulaire :

``` html
    <form 
        id="signin-form"
        action="<?= $router->generate('signin_post') ?>" 
        method="post"
        onsubmit="fetchForm()"
    >
```

``` javascript
// on récupère les données du formulaire
const formData = new FormData(document.getElementById("signin-form"));

// on crée une méthode pour afficher un message en fonction du résultat de la requête
async function fetchForm() {
    // puis on passe les données dans la requête en post sur la page user/signin au moyen de l'attibut body
    const request = new Request("user/signin", {
        method: "POST",
        body: formData
    })
    try {
        const response = await fetch(request);
        if (response.ok) { // on vérifie que la réponse du serveur http se trouve dans l'intervalle 200-299
            alert("Le formulaire a bien été envoyé.");
        } else {
            throw new Error("Erreur de réponse serveur.");
        }
    } catch (error) {
        console.error("Error:", error);
    }
}
```

Pour approfondir, voici le lien vers la doc de l'[API Fetch sur le MDN]("https://developer.mozilla.org/fr/docs/Web/API/Fetch_API").
