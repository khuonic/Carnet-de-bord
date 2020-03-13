
<p><strong>Carnet de Bord :</strong></p>
<p>E-shop LBB</p>

**Semaine du 24 Février :** 
	-> Mise en place de Symfony sur nos VM, visionnage de plusieurs tutoriels symfony sur les controllers, les entités et twig.
	-> Création du groupe de travail avec benjamin, marion et william.
	-> Brainstorming pour rassembler les idées sur le site, mise en place d'une maquette sur moqup, création de la base de données avec symfony et doctrine, mise en place du projet sur GitHub.
	-> Création de quelques produits et ajout dans la base de donnés des produits crées.
	Plusieurs étapes doivent être respectées : -ajout des catégories, des sous catégories, du prix de chaque produit, ajout du produit avec son price id et category id, ajout de l'article avec son product_id , ajout de l'image et son article_id
**Semaine du 2 mars :**
	-> Création d'une migration avec les insertions en base de données
	-> modification de la table product et création d'une nouvelle table permettant d'avoir 	plusieurs catégories pour un produit.
	-> Création du controller product et de sa vue, création globale de la vue et ajout des fonctionnalités petit à petit via symfony.
Injection de dépendance pour appeler la classe product, afin de pouvoir appeler la classe article, price et articleImage et de pouvoir les afficher dans la vue.
	-> Création de la homepage avec la navbar de catégories, création de slideshow avec comme contenu les derniers produits ajoutés en base de données.
	-> Création d'un service symfony(contenant la fonction permettant d'appeler les catégories pour le header).
	-> Mise en place du changement d'image de manière dynamique sur la page produit, réflexion sur la possibilité d'implémenter de l'ajax sur les select de la page produit, afin de rendre la page la plus dynamique possible.
	-> Débug du yarn encore watch, afin de pouvoir build en continu, injection d'une option poll dans le fichier webpack config.js de symfony, commande  a utiliser dorénavant => yarn encore dev --wath --watch--poll.
	-> Mise en place du menu de sous-catégorie en hover, et des pages catégories avec lien pour les sous-catégories sur chaque page.
	-> Création du Controller et vue d'authentification, avec le script permettant de se connecter avec google.
	-> Générer des soumissions de form sans refresh de la page, avec des soucis sur le .submit (preventDefault ne marchait pas comme on le souhaitait). Remplacement du onsubmit par un onchange sur les select.
	-> Problème sur la requete ajax, requete symfony vide, pour régler le problème on a utiliser URLSearchParameters (voir doc JS).
	-> Concrètement, il est désormais possible de récupérer l'Id de l'article selon les différents critères de taille, device et color, et on génère un message selon le stock restant, ainsi que l'affiche du bouton d'ajout au panier.
	-> Modification de la bdd, création du tag_category reliant les tag aux catégories, création de la table cart (remplaçant orders_artices) permettant d'intégrer la quantité d'articles choisis dans le panier.
	-> Création du chemin vers les pages catégories, et affichage de celles-ci via la base de données. 
	Mise en place d'un renquirement pour le chemin de la page.
	->Création du controller cart et d'une méthode permettant l' Ajout de toast dynamique lors de l'ajout au panier, avec récupération des informations en ajax de l'article ajouté (titre image, taille device, couleur, quantité). 
	-> Ajout d'images dans la table catégorie pour chaque sous catégorie, pour les sous catégories, création des pages sous catégorie, pour le moment vide, et lien vers ces pages.
	->Création du  Slider permettant de récupérer les 9 derniers articles créés (via une fonction réutilisable) pour l'afficher en homepage.
->Création du formulaire d'authentification, via les formulaires symfony.
-> Mise en place du bundle easyadmin qui permet de générer rapidement une interface admin cohérente et fonctionnelle.
**Semaine du 09/03 :**
-> Création de la page du panier et des éléments associés (). 
-> Finition du champ de recherche des sous catégories via les tags, avec autocomplétion des tags de la base de données
-> Création de la page sous-catégories avec les filtres associés
-> Filtre slider sur les prix (nouislider)
-> Problème sur les url de chaque route réglé, via le fichier routes.yaml : toutes les routes des controllers sont insérées dans le fichier
-> Page de connection et d'inscription
-> Page d'ajout d'une adresse au profil en récupérant l'user via security.yaml
-> Création du formulaire de contact
-> Création de plus de produits et insertion en bdd via easyadmin
->  Création de la page profil, avec plusieurs vues (possibilité de changer l'email et l'abonnement à la newsletter, possibilité de changer de mot de passe, affichage des différentes adresses du compte 
-> Modification de la table article image en bdd et des fonctions qui lui étaient liées
-> Page profil : adresse-> possibilité d'ajouter jusqu'a 5 adresses, de les modifier ou les supprimer au besoin, une seule adresse par défault est possible. Sécurisation de l'accès au nouveau password, possible seulement si l'ancien mot de passe a été renseigné au préalable.
-> Début de la création des reviews sur un produit, lié a l'account, affichage des reviews.
-> Mise en place des droits d'acces sur les pages selon le role utilisateur (ROLE_USER ou ROLE_ADMIN), notamment pour le backoffice easyadmin, dorénavant accessible que pour le ROLE _admin, et la page de profil uniquement accessible pour le ROLE_USER
-> Fo







<blockquote>
<p>Written with <a href="https://stackedit.io/">StackEdit</a>.</p>
</blockquote>

<!--stackedit_data:
eyJoaXN0b3J5IjpbNDY2NDE3NzEyLC00NzMyMTUwOTAsLTExMz
Y5MTU2NzIsNTU2Njc0MDMxLDEyOTEzNDEzMjIsLTE3NTk3MTAz
MTIsLTEzMDYwNzk4OTAsMTk2NzIzMzAwNyw3MDgxOTQ1OCwxND
QyNzQ5NjMzLDI5MDQ2OTgwOSwyMDgzODgwNTgzLC0xNjYzODEx
OTEyLC0xOTMwNTA0MzU2LDE4MDc4NDQ4MzcsLTEwODg4NTk3OT
QsLTEzMDIzMTE2NzQsMjA1MjQwNDY4OSwyMTMyNzk2MTAzLC02
Njc5MDU1MTFdfQ==
-->