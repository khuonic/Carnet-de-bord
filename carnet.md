
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
	-> Débug du yarn encore watch, afin de pouvoir build en continu, injection d'une option poll dans le fichier webpack config.js de symfony, commande  a utiiser dorénavant => yarn encore dev --wath --watch--poll.
	-> Mise en place du menu de sous-catégorie en hover, et des pages catégories avec lien pour les sous-catégories sur chaque page.
	









<blockquote>
<p>Written with <a href="https://stackedit.io/">StackEdit</a>.</p>
</blockquote>

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIxMjk3NTAzMTksMjkwNDY5ODA5LDIwOD
M4ODA1ODMsLTE2NjM4MTE5MTIsLTE5MzA1MDQzNTYsMTgwNzg0
NDgzNywtMTA4ODg1OTc5NCwtMTMwMjMxMTY3NCwyMDUyNDA0Nj
g5LDIxMzI3OTYxMDMsLTY2NzkwNTUxMSw4MDIzMzgwMjddfQ==

-->