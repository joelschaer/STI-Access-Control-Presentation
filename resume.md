# Chap. 8, Attacking Access Controls

## Vulnérabilité  commune

Il y a trois grandes catégorie de contrôle d'accès sont :
  - verticale :
    division des fonctionnalités d'une appplication suivant différents rôles
  - horizontale :
    accès à des sous-ensembles plus large de ressources du même type 
  - dépendant du contexte :
    accès des utilisateurs est limité à ce qui est autorisé dans l'état actuel de l'application
Ces trois catégories s'entremêlent dans un application réelle.

Les trois types d'attaques reliées à ces contrôles d'accès sont :
  - Vertical privilege escalation : 
    un utilisateur exécute des fonctions dont il ne possède pas le rôle qui lui permet
  - Horizontal privilege escalation : 
    un utilisateur peut modifier ou voir des ressources dont il n'a pas le droit
  - Business logic exploitation :
    un utilisateur arrive à exploiter une faille dans l'état actuel de la l'application pour gagner l'accès à des ressources clés

#### Fontionnalité complétement non protégées

  L'utilisation d'une URL d'une page d'administration ne contenant aucune vérification et la seule défense est l'omission du lien sur une page non administrateur ainsi que l'affichage de ce lien géré dans un script javascript coté client.

#### Identifier-Based Functions
#### Multistage Functions
#### Static Files
#### Platform Misconfiguration
#### Insecure Access Control Methods
## Attacking Access Controls
#### Testing with Different User Accounts

La meilleure manière et la plus efficace de  tester les contrôle d'accès d'une application est d'utiliser différents compte. Cela permet de déterminer quelle ressource ou fonctionnalité est accéssible par différent type de compte et s'assurer que leurs accès sont légitimes

Certains outils peuvent aider à cela tel que Burp qui permet de  comparer le contenu d'un site web selon différents contextes, on peu ensuite comparer ses derniers.

#### Testing Multistage Processes

La méthode décrite précédement peut se trouve être inéfficace lors du test de certains processus en plusieurs étapes. Dans le cas de multistage, pour effectuer une action l'utilisateur doit en avoir fait d'autre au préalable et celles-ci dans le bon ordre.

Donc intérroger les formulaire de manière séparée avec différents comptes peut ne pas donner les résultats attentu car ces action pourraient générer des erreurs.

Par exemple si on imagine le cas ou on aurait un formulaire pour ajoute nouvel utilisateur :

1. chargement du formulaire pour ajoute un utilisateur
2. soumettre le formulaire avec les détails de l'utilisateur
3. examiner ces détails
4. confirmer l'action

Dans ce cas, l'application pourrait protéger l'accès au formulaire initiale, mais ne protégerais pas la page qui soumet le formulaire, le processu global peut impliquer de nombreuse demandes, rediréctions et paramètres provenant des étapes précédente. Pour cela, chaque étapes doit être testée individuellement afin de vérifier que les contrôle d'accès sont appliqué correctement.

Pour tester cela, on peut envoyer des requêtes Burp en utilisant la session courante.

#### Testing with Limited Access

Lors que on l'on test une application et que notre accès est limité, par exemple que un compte utilisateur à notre disposition, Il va falloir redoubler d'éffort pour tester l'entièreté de l'application. Un fonctionnalité non protégée peut exister sans pour autant se trouver sur notre interface. Par exemple des fonctionnalité prévue pour d'autre utilisateurs, d'anciennes fonctionnalités ou des nouvelles pas encore intégrée.

Pour tester ces choses là nous pouvons utiliser la technique de "Content discovery" expliqué au Chapitre 4.

#### Testing Direct Access to Methods

Lorsque une application utilise des requêtes donnant accès à une API côté serveur, vous devriez découvrir les faiblesses de contrôle d'accès avec les méthodes précédentes. Cependant vous devriez aussi vérifier l'existence d'API supplémentaires qui pourraient ne pas être correctement protégées.

Pour cela essayer d'identifier les appels d'API existants, get, add, updates etc... Vous pouvez aussi consulter des sources publiques, forum, documentation etc... afin de vous renseigner sur l'API utilisée. Ensuite tester les appels avec différents paramètres, id etc...

#### Testing Controls Over Static Resources

Dans le cas ou l'application peut fournir des ressources statiques, vérifiez que celles-ci sont gérée par contrôle d'accès et qu'elles ne sont pas directement accéssible par l'URL.

Pour voir les fichiers d'une application plusieurs outils existent comme par exemple : Sparta qui permet de faire un brute-force sur tous les fichiers, dossier d'un site web.

#### Testing Restrictions on HTTP Methods

Dernièrement vous pouvez tester les contrôles d'accès en modifiant les requêtes HTTP (POST, Get, HEAD) ainsi que modifier les cookies. Surtout intéréssant lorsque on a des méthodes pour ajouter des utilisateur ou changer le rôle d'un utilisateur.

Pour cela vous pouvez utiliser Burp.

## Securing Access Controls
La sécurité des contrôle d'accès est la partie la plus simple à comprendre lors de la sécurisation d'une application, mais ce n'est pas pour autant qu'elle doit être sous-estimée. Beaucoup de contrôles ont tendances à être oubliée lors du développement.

Faites attention au :

- numéro de compte, ID etc... pouvant se trouver dans l'url.
- Query string de style admin=true.
- ne partez pas du principe que les utilisateurs vont suivre l'ordre des actions que vous avez prévus.
- vérifier les contrôles d'accès pas seulement à l'affichage des informations mais aussi lors de la soumission de formulaire.
- Evaluez de manière précises et documentée qu'elle action est autorisée par quel type d'utilisateur.
- définissez les contrôle d'accès selon la session.
- Utilisez un système de session sécurisé.
- Dans certains language vous pouvez définir des interfaces obligatoires à implémenter sur les différentes pages afin de forcer les développeurs à les programmer.
- Pour les fichiers statiques, on peut y accéder en passant par les nom de fichiers au serveur qui implémente un contrôle d'accès dynamique ou faire un authentification en utilisant HTTP.

#### A Multilayered Privilege Model

Le contrôle d'accès concerne pas seulement l'application web mais aussi les autres niveaux d'infrastructure qui se trouve "en dessous". En particulier les base de données, système d'exploitation etc...

L'intérêt de protéger toutes les couche est qui si un hacker compromet une couche il n'aura pas accès à tout le système, par exemple si votre site web est compromis vous devez être sûr que le hacker ne peux pas s'immiscer dans le système d'exploitation.