**Carnet de bord**
**SEMAINE  1:**
**Lundi** : Recherches effectuées à propos de Magento, début d'installation sur la Vm via composer, clés d'authentification créées sur le compte magento (public key: username, private key: password), problèmes de dépendances php.
Au final, installation directement via le client de download du site: puis dans le dossier via putty : **composer install**, et **bin/console setup:install** comme indiqué dans la doc avec les paramètres choisis (en ayant créé une db magento sur phpmyadmin au préalable). **sudo composer update --no-plugins** pour éviter certaines erreurs. Mais toujours rien a l'affichage. A refaire a zéro mardi

**Mardi**: 
J'ai du apporter des modifications aux options d'apache pour pouvoir charger correctement magento
 Apache rewrite: 
**<Directory "/var/www/html/magento">**
	  AllowOverride All 
  **</ Directory>**
  apache restart
Après ça, d'autre soucis, le style ne se chargeait pas.
 J'ai essayé de voir si cela venait du maintenance mode, apparemment non, j'ai donc utilisé la commande `bin/magento setup:static-content:deploy`pour déployer les fichiers statiques en production.
 Toujours pas de style appliqué, après redémarrage de la Vm, plus rien ne marche.
 Installation à nouveau, via setup wizard install. Tout fonctionne à présent, ça a quand même l'air de ramer
 Installation de prestashop sur la Vm
**Mercredi :** 
J'ai trouvé un cours sur magento2 proposé par Adobe, qui couvre les fondamentaux. Je commence à suivre le tutoriel, quelques soucis de permissions pour certains fichier lorsque je veux créer un nouveau module ou changer le mode de développement, pas réussi à les résoudre encore, mais j'avance tout de même petit à petit, magento semble être un sacré morceau.

**Jeudi:** Reçu un document de Nicolas avec des instructions. Je dois récupérer une liste de produits disponible dans le site magento et les retourner en json dans un nouveau script php externe. Je dois donc interroger l'API magento depuis un script externe afin de pouvoir récupérer des données, consultation de la doc magento sur l'API et l'authentification.
Lecture de la documentation de l'API rest Magento et des tokens pour l'intégration.
Pour obtenir les tokens nécessaire à l'appel de l'api via oAuth: 
Admin page de magento -> system -> integration-> add integration-> remplir les champs : name et password, donner les accès aux données voulues dans l'onglet API. -> save -> allow -> donne Consumer Key, Consumer Secret, Access Token et Access Token Secret.
Utilisation de postman pour lancer des requêtes avant de se lancer dans le code. 
Methode : GET . 
URL :http://127.0.0.1:8080/magento2/rest/V1/products?fields=items[sku,name]&searchCriteria[pageSize]=10  => donne les noms et SKU des 10 premiers produits en bdd. (cf https://devdocs.magento.com/guides/v2.3/rest/list.html pour la liste des url REST magento) .
**Authorisation: Type : OAuth 1.0** -> rentrer les tokens reçus au préalable via l'admin page de Magento.
**Add authorization data to** : request headers
Résultat: on obtient bien un objet json avec les noms et sku des 10 premiers produits du catalague.

**Vendredi:** Le but aujourd'hui est de réussir à appeler la requête effectuée hier avec postman, mais dans un script php.
J'ai trouvé une façon de faire une requête via curl en introduisant les données nécessaire dans le headerrequest (tokens, keys, signature, etc..) 
mais j'ai toujours une erreur " **Curl failed with error #7: Failed to connect to 127.0.0.1 port 8080: Connection refused**"
Tentative de résolution de ce problème afin de voir si le code est fonctionnel.
Problème apparemment résolu, le soucis maintenant viendrait de la signature "**The signature is invalid. Verify and try again.**"/
Magento compare la signature qu'il crée et celle que j'envoie, elles sont identiques lorsque je fais cette requête par exemple : hhtp://localhost/http://localhost/magento2/rest/V1/categories, mais différentes si je  rajoute un "?" à la fin. Je n'ai toujours pas trouvé le problème, qui vient sans doute de l'échappement des caractères dans ma fonction.

**SEMAINE 2:**

**Lundi:**  Je dois réussir à résoudre le problème de signature pour arriver a effectuer toutes les requêtes. Je regarde donc le code utilisé par magento pour générer la signature, Magento utilise la librairie OAuth qui utilise Zend.
J'essaye de changer le code de création du baseString nécessaire à la signature, pour le moment rien ne marche.
J'ai tout vérifié, j'ai meme comparé ma signature avec celle générée par un site avec les memes paramètre (http://lti.tools/oauth/) et la ça colle, c'est bien la signature générée par magento et la librairie Oauth qui crée une différence avec les "?"
Journée passée à résoudre le problème sans trouver la solution.
Rappel pour le rapport de stage: exemple de recherche en anglais, (peut etre utile de mettre le tuto oauth twitter api)

**Mardi:** 
Refactoring de mon code, création d'une classe et d'un code plus propre pour l'appel de l'api, le code fonctionne comme avant, toujours la même erreur au niveau des signatures.
Changement de solution, j'ai installé l'extension OAuth : ```
**sudo apt-get update
sudo apt-get install php-oauth
sudo service apache2 restart**
Et créé un script avec  cette extension: 15 lignes de code, ça marche..

**Mercredi:**
Refactoring du code d'hier avec la classe oauth. 

**Semaine 3:**

**Lundi:**  Call avec Nicolas pour prévoir une rencontre à l'entreprise le mardi. Vérification de la compatibilité de Magento 2 avec php 5 -> incompatible. Veille technique.

**Mardi:**
Je suis allé travaillé l'après midi à l'entreprise, pour m'entretenir avec nicolas, il a pu me montrer plus en détail leur CMS MasterEdit et nous avons discuté de ce que j'ai fait pour l'API.
Nouvelle mission: créer un bouton qui servirait plus tard dans le backend de leur CMS pour permettre d'exporter les données d'une page en csv. 
J'ai donc les accès ftp au serveur du site 2exvia, acces a la base de données dans phpmyadmin, et accès a une partie du backend du site de l'agence. Nicolas m'a déjà transmis un script php ou il affiche le nom et id de toutes les images présentes en bdd de leur site. Le but pour commencer serait peut etre de réussir a faire en sorte d'en faire un fichier csv téléchargeable, et plus tard d'élargir à toutes les données du site, avec des conditions bien sur (dans quel répertoire ou rubrique on se trouve au moment ou l'on clique sur l'export).

**Mercredi:** Connexion en VPN sur leur serveur depuis chez moi. (avec VPN access Manager).
Beaucoup de lecture de doc sur comment m'y prendre pour la mission, j'ai des idées qui arrivent, mise en pratique l’après midi.
Création d'un bouton dans un form sur une autre page php qui appelle la page de download du csv.
Dans cette page j'ai créé une fonction pour exporter les données appelées par la requete sql en csv.

**Jeudi :** Création d'une nouvelle page, pour restructurer les requetes et fonctions créées, création d'une requete sql qui récupère l'id d'une rubrique et son nom, et création d'une requete pour récupérer le nom et id des produits présents dans la rubrique récupérée.
Appel de manière dynamique dans la page button.php.

**Vendredi:** Réorganisation du code et création de deux inputs permettant de rentrer l'id d'une rubrique ou le nom d'une table et d'effectuer la requête, puis d'exporter les données.
Le soucis majeur que j'ai désormais a résoudre, c'est de pouvoir récupérer tous les produits présents dans une rubrique et ses sous-rubriques, jusqu'à présent je récupère seulement les données d'une rubrique, si jamais elle contient des sous rubriques je ne récupère pas leurs produits.

**Semaine 4:**

**Lundi:** Je vais essayer de faire une requête SQL récursive afin de pouvoir avoir accès aux sous rubrique lorsque j'appelle une rubrique par son ID. Ca permettrait d'avoir accès à toutes les données d'une rubrique et de pouvoir les exporter facilement ensuite.
Création de la requete qui permet de récupérer toutes les sous rubrique existantes dans la rubrique choisie, donc cela me permet ensuite d'en extraire tous les produits, en bouclant de la meme manière sur chaque sous rubrique, afin de l'intégrer dans un array qui est intégrer dans l'export en csv.

**Mardi:** Journée à la boite, passé la matinée avec nicolas pour refactorer le code et créer des requetes récursives plus propres pour récupérer les rubriques et les produits.

**Mercredi:** Deuxieme journée  à la boite.
Nicolas m'a aidé à créer la fonction pour récupérer proprement sous forme de tableau les produits. J'ai pu refaire la fonction d'export avec ce résultat. 
Récupération des informations désirées lors de l'export pour les images via un fichier Xml, ces infos sont ensuites envoyées dans le Select de la requete.

**Jeudi:** Télétravail a nouveau, je suis inscrit sur slack et j'ai une adresse mail 2exvia désormais.
Le but maintenant sur le projet est de réussir à faire un script plus général et adaptable, qui marcherait sur tous les produits du site en backOffice, (pages, billets, images) et qui marcherait également sur n'importe quel type de nouveau produit créés. Il faut donc réussir à faire des reqûetes qui cherchent dans les tables qu'il faut selon ou l'on se trouve lors de l'export. 
Je récupère une valeur dans le fichier Xml de chaque table qui est un boolean, lorsqu'elle est true, cela signifie que les rubriques et sous rubriques existent et donc que les produits à récupérer se situent dans la table boutique_li_produits_rubriques, si false ils se trouvent également dans cette table pour les produits de niveau 0 mais également dans la table boutique_li_produit, qui recense les produits enfants d'un produit Parent (ex : 'pages' dans le backOffice du site 2exVia). Il faut donc que j'adapte les requêtes avec ces conditions la.

**Vendredi:** Finalement je retrieve tous les produits de la table me-pages,  je ne sais pas quelle condition permettrait de récupérer seulement les pages affichées en backOffice. 
SInon je suis en train de récupérer toutes les infos du Xml pour afficher un rendu plus propre en Csv a la fin.
 J'ai également  créé les conditions qui permettent de vérifier si l'on se trouve dans une table ou les produits sont dans des rubriques, ou plutot dans des produits, selon le résultat je ne fais pas les meme requetes.
 Utilisation d'expression réguliere qui permet de créer une sécurité supplémentaire lors de l'envoi de la requete

**Semaine 5:**

**Lundi:** 
	Discussion avec Nicolas, afin de mettre en place petit a petit l'export sur le site.  Avec des options possibles pour l'utilisateur (ex: avoir les En tetes dans le csv ou non, possibilité de choisir le caractère de séparation des colonnes (';' , ';' ) ou encore choisir directement un format par rapport aux anciens exports fait dans le passé). J'ai donc créé un autre répertoire en ftp, dans lequel je créé un fichier txt avec les options choisies à chaque export. Ca permet de créer un historique qui pourra servir si l'utilisateur veut aller plus vite dans le choix des options.

**Mardi:** Dorénavant au lieu de simuler l'envoi des données souhaitées via un formulaire que j'avais créé je peux désormais directement les envoyer via le site, via une requête ajax qui envoie les paramètres nécessaires a l'export Csv ($params['nomTable'], $params['idRubrique'], $params['options']).  
J'ai également ajouter des conditions dans la récupération des informations des pages Xml du backOffice, pour récupérer les infos en Francais ou en Anglais, selon si le site sur lequel on se trouve est en Fr ou En.
Je dois réfléchir à d'autre options possibles qui seraient utiles pour l'utilisateur lors de l'export. Je dois également trouver un moyen (lorsque je récupère les pages par exemple) de récupérer seulement les pages affichées en backOffice, non pas toutes les pages de Bdd.

**Mercredi:** J'ai mis en place l'export Xml en utilisant DOMDocument en php. Je boucle sur les produits que l'on récupère et sur les séparateurs récupérés par la fonction recupèreInfosXml. Je créé d'abord un champ table auquel je passse en attribut le nom de table (moins le me_).
Ensuite je créé un champ <produit> pour chaque produit récupéré, a l'intérieur duquel je créé un champ séparateur avec comme attribut nom le nom du séparateur, et a l'intérieur duquel je créé des champs titres  avec comme attribut nom les titres des infos récupérées en base de données (ex nom; idRubrique) auxquels j'appendCHild un textNode qui contient les valeurs.
Des options sont possibles pour l'XML, telle que choisir l'encodage (utf-8 ou utf-16), choisir d'exporter l'XML indenté ou non (DOMDocument::formatOutput), possibilité d'afficher ou non les séparateurs visibles dans le backOffice (ex: 'informations', 'référencement') de la même manière que dans l'export CSV.
Il me reste à changer la façon donc je sauvegarde les différentes options choisies par l'utilisateur dans le fichier txt du répertoire optionsExport. Je vais voir avec Nicolas la meilleure facon de faire pour pouvoir les exploiter au mieux plus tard. 
J'ai pour le moment créé une condition qui dans le cas ou l'utilisateur ne choisit pas un ancien modèle d'export, ses choix  seront sauvegardés dans les anciens modèles seulement si aucun des fichiers déjà existants ne contient les mêmes paramètres.
J'attends de voir avec Nicolas sur quoi je peux me lancer, je suis un peu bloqué pour avancer tant qu'on n'aura pas réellement implanter les requêtes ajax pour appeler mon script, avec toutes les options envoyées en paramètre de la requête.

**Vendredi:** Refactoring du code et implémentation de manière plus dynamique des options et de l'export.
Nicolas a modifié les requêtes ajax dans les fichiers masterEdit, et a implémenté du Js supplémentaire (avec Jquery) notamment pour toggle les élements selon si on choisit l'export Csv ou Xml. 
On recoit bien en paramètre de la requete les options choisies par l'utilisateur afin de créer le fichier dans le répertoire export.
Il reste encore à créer l'export en js qui lancera le download du fichier créé (le nom du fichier est envoyé en réponse lors du click sur exporter).
Lundi je vais pouvoir me lancer sur la recherche des valeurs multiples qui peuvent etre possible pour un produit. Ces valeurs ne se trouvant pas dans une table classique, je vais devoir faire des requêtes sql supplémentaires pour récupérer les valeurs multiples.

**Semaine 6 :**

**Lundi:** Retour à la mide pour travailler et voir des personnes de la formation.
J'ai pu avancé sur la récupération des valeurs multiples pour les produits.
Je récupère dans le xml les champs->typeinfo avec la valeur 'listemultiple', je recupère la correspondance ('champ' dans la base boutique_info_multiple qui permet de recuperertoutes  les 'valeur' pour un produit), je récupère également les champs->valeurspossibles ce qui me donne (base:me_fichiers par exemple, qui donne donc le nom de la table dans laquelle aller chercher les noms des infos multiples avec les 'valeur' récupérées dans le même temps).
Une fois la nouvelle requête sql effectuée, je récupère les différentes infos multiples pour chaque Produit s'il en existe, que je place dans un string et que j'introduis dans le tableau retourné par ma fonction recupereProduits. A l'export je place également en nom de colonnes le nom 'infosmultiples' avec un array_push tout simplement.

**Mardi:** Application sur l'export Xml également, le séparateur InfoMultiples est créé et à l'intérieur un titre avec un attributeName='infosMultiples' qui récupère les infos multiples de la requête.
Dans l'export Csv je fais un array_map(utf-8 decode) du tableau de valeurs envoyées pour qu'à l'affichage il affiche bien les accents, etc..
J'unset également les valeurs 'base' et 'champ' du tabXml dans les deux exports, pour ne pas les avoir en tant que séparateur, ces deux valeurs n'étant utiles que pour la requête mySql permettant de récupérer les infos multiples.
Je réfléchis à d'autres ajouts potentiels dans le dev qui seraient intéressants à effectuer.
J'ai de nouvelles instructions à ajouter dans le dev, la tache se complique.
Premierement il faudrait classer les produits 'pages' par rang dans la requete Sql, je vais donc devoir faire des jointures pour récupérer le rang de chacune des pages dans les table.
Il va falloir également prendre en compte la recherche et le tri dans l'export, de sorte que si l'utilisateur fait une recherche avec un mot, le résultat obtenu puisse etre exporté.
Sinon le tri servirait a changer l'ORDER BY de la requete selon le tri choisi.
Il faudrait aussi changer le nom de fichier, de sorte que les options choisies et le type d'export apparaisse dans le nom.
Autre options à rajouter serait également les produits archivés, si l'utilisateur choisit oui ou non de vouloir exporter les archives.
J'ai rajouté l'option d'exporter ou non les archives selon si l'utilisateur à cliquer sur voir les archives avant d'exporter (ie si le parametre $param['archive'] vaut 0 ou 1).

**Mercredi:**  Le nom des options dans le nom du fichier a été rajouté, j'ai également changé le nom des colonnes dans le csv, ce n'est plus le nom en bdd mais bien le nom affiché dans l'interface utilisateur (ie $champs->nom dans le fichier xml de MasterEdit).
J'ai également rajouté une condition pour vérifier si l'u
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjI3MDE0MDc1LC0xMDEwODc5NDkxLC0xNj
IwMDU2MTI5LDQxODczNjU2LC0xMjAxNDczNzA5LC0xMTk2MjI3
NzIsLTE0OTYzMDIwMzQsMTg4NTA2NzMyOSwxMDYyMTU1MTMyLC
0yMDc5MDE3NDY3LC0xMTQ0NjE5OTQwXX0=
-->