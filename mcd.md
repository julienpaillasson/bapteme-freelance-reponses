# Correction MCD

---

## Evaluation d'ensemble

Bon travail, avec une petite fragilité sur la notation des cardinalités, à consolider. 

---

### Entités

Parfait, tu as bien représenté l'ensemble des entités (utilisateurs, produits, adresses, commandes) concernant les données du projet.

---

### Associations

Parfait aussi, on retrouve bien l'ensemble des associations demandées dans le cahier des charges.

---

### Cardinalités

Bon travail dans l'ensemble, avec toutefois une erreur sur la nature de la relation entre les utilisateurs et les produits. En effet, il est stipulé qu'un utilisateur peut (ce n'est donc pas obligatoire) cliquer sur j'aime pour un ou plusieurs produits. Chaque produit peut donc être "liké" par 0, 1 ou plusieurs utilisateurs. Il s'agit ici d'une relation "many to many" que l'on va représenter par les notations 0, N de chaque côté de l'association. 

Ici, tu as décrit une relation "one to one", comprenant forcément un élément et pas plus.

Pour approfondir, tu peux jeter un œil à cette ressource [cardinalités](https://www.base-de-donnees.com/cardinalites/).

