# Chap. 8, Attacking Access Controls

## Types de Vulnérabilités 

Il y a trois grandes catégorie de contrôle d'accès :
  - **verticale** :
    division des fonctionnalités d'une application suivant différents rôles.
  - **horizontale** :
    accès à des sous-ensembles plus large de ressources du même type.
  - **dépendant du contexte** :
    accès limité à ce qui est autorisé dans l'état actuel de l'application.
    intervient dans le cas d'un processus à étapes multiples.

Les trois types d'attaques reliées à ces contrôles d'accès sont :
  - **Vertical privilege escalation** : 
    un utilisateur exécute des fonctions dont il ne possède pas le rôle adéquat
  - **Horizontal privilege escalation** : 
    un utilisateur peut modifier ou voir des ressources dont il n'a pas le droit
  - **Business logic exploitation** :
    un utilisateur arrive à exploiter une faille dans l'état actuel de l'application pour gagner l'accès à des ressources clés, exemple bypasser l'étape de payement

#### Fonctionnalités non protégées

Les pages avec des fonctionnalités particulières comme des pages admin doivent être protégées d'accès pour les utilisateurs standards. Une technique pour éviter l'accès à ces pages est de tester le rôle de l'utilisateur et d'ajouter en fonction les boutons d'accès. Il n'y a alors pas d'autre protection pour l'accès aux pages protégées que l'affichage visuel d'un bouton permettant d'y accéder.
Ce genre d'affichage peut être géré côté serveur ou coté client depuis JavaScript.
Dans ce dernier cas, il est encore plus facile pour un attaquant de trouver les pages en question, car leur URL est disponible en claire dans le fichier côté client.

#### Accès direct aux méthodes

Dans le cadre d'une API, par exemple, il est possible de rendre disponible certaines fonctions pour tous. Il  faut alors s'assurer que les autres méthodes sensibles ne soient pas également accessible. Si l'accès n'est pas restreint, il sera possible à n'importe qui de deviner les noms de fonction et de les utiliser.

#### Accès par identifiant

Une manière commune d'accès à des données est de passer l'identifiant de la ressource dans l'url. C'est  une manière simple de procéder, mais il faut s'assurer qu'un utilisateur n'ait pas accès aux ressources qui lui sont autorisées. Ce genre de vulnérabilité intervient dans le cadre de la vulnérabilité horizontale. 

#### Fonctions à étapes multiples

La gestion d'application avec des étapes multiples nécessitent une attention particulière. En effet, certaines actions vont traverser un certains nombres d'étapes successives pour valider une action. Il faut, dans ce cas de figure, s'assurer que l'utilisateur ait les droits d'effectuer chacune des étapes. On ne peut pas partir du principe qu'il est passé par les étapes précédentes. 
Pour la modification d'un utilisateur, si on fait la validation d'authentification sur la requête get qui affiche le formulaire mais pas sur la méthode post qui fait la modification. Il est alors possible de n'envoyer que la requête post et donc d'appliquer directement la modification.

#### Fichiers statiques

Si des fichiers sont accessible en téléchargement direct depuis le site, pour des site de téléchargement d'Ebooks par exemple. Il existe donc un lien qui pointe directement vers le fichier. Quand on va sur ce lien, il ne fait que pointer vers une ressource du serveur en accès libre et aucune logique de validation ne peut être implémentée entre les deux. Le serveur va simplement retourner la ressource demandée. Après avoir téléchargé un fichier, il nous est possible de deviner les noms des autres fichiers et les télécharger et faisant directement appel à la ressource.
Tout type de document pouvant être téléchargé est sujet à ce risque.

#### Mauvaise configuration

Certaine méthodologies d'authentification sont valides mais comportent des vulnérabilités de par leur mise en application. En interdisant, par exemple, les requêtes POST pour les utilisateurs qui ne sont pas admin. On se dit que c'est une bonne idée vu qu'il faut faire, en général, un appel de POST pour faire une modification. Si l'application est mal configurée, il sera dans certains cas possible de faire les actions POST avec une requête GET.
Certaines applications pourraient utiliser GET pour faire des modifications en utilisant les query strings par exemple. Ce n'est pas une bonne pratique mais c'est possible. Il faudrait alors bloquer les requêtes GET également. Mais, il est possible de faire une requête HEAD qui souvent effectue le code d'une requête GET mais ne retourne que l'entête. Si la requête GET est bloquée mais que des données sont traitées à l'intérieure, il est possible d'activer ce processus sans recevoir de requête avec HEAD. L'action aura cependant été quand même effectuée.

#### Méthodologie Vulnérable

Certaines méthodologies d’authentification sont fondamentalement erronées. Elle ont pour but de valider si un utilisateur est admin ou non. Cependant, celles-ci sont mal pensées et ne permettent pas de valider leurs authenticités de manière suffisamment fiable. 

##### Lié aux paramètres

Les paramètres, qui sont transmis par le client, peuvent servir d'authentification. Le problème est que cette sécurité est réalisée côté client. C'est une mauvaise manière de procéder du fait qu'il est possible pour n'importe quel utilisateur de modifier ce qui se déroule sur ça machine. Il peut ainsi passer un flag "isAdmin" à true sans problème avant de retourner la requête. Cela peut aller de la query string dans l'url au paramètres dans le cookie.

##### Lié au "Referer"

Le référer indique dans l'entête quelle est la page précédent. Il est alors possible de vérifier que la page précédente est la page du menu "Admin". Si l'authentification est validée pour l'accès à cette page, on peut alors supposer que l'utilisateur s'est authentifié et est un administrateur. Cette manière est liée aux paramètres modifiables par l'utilisateur. En effet, l'entête d'une requêtes est modifiable à la guise de l'utilisateur.

##### Selon la localisation

Pour restreindre l'accès à une ressource, certaines applications se basent sur la position géographique d'un utilisateur. On va, par exemple, restreindre la rediffusion des émissions pour les personnes hors du pays. C'est par exemple ce que fait TFI, pour limiter l'accès au live en ligne. Il suffit alors d'utiliser un proxy ou un VPN pour contourner ce contôle.

## Attacking Access Controls
#### Testing avec différents User Accounts

La meilleure manière et la plus efficace de  tester les contrôles d'accès d'une application est d'utiliser différents comptes. Cela permet de déterminer quelle ressource ou fonctionnalité est accessible par différent type de compte et s'assurer que leur accès soit légitime.

Certains outils peuvent aider à cela tel que Burp (spidering) qui permet de  comparer le contenu d'un site web selon différents contextes, on peut ensuite comparer ses derniers.

#### Testing Multistage Processes

La méthode décrite précédement peut se voir être inefficace lors du test de certains processus en plusieurs étapes. Dans le cas de multistage pour effectuer une action, l'utilisateur doit en avoir fait d'autre au préalable et celles-ci dans le bon ordre.

Donc interroger les formulaires de manière séparée avec différents comptes peut ne pas donner les résultats attendus car ces action pourraient générer des erreurs.

Par exemple, si on imagine le cas ou on aurait un formulaire pour ajouter un nouvel utilisateur :

1. chargement du formulaire pour ajouter un utilisateur
2. soumettre le formulaire avec les détails de l'utilisateur
3. examiner ces détails
4. confirmer l'action

Dans ce cas, l'application pourrait protéger l'accès au formulaire initiale, mais ne protégerais pas la page qui soumet le formulaire, le processus global peut impliquer de nombreuse demandes, redirections et paramètres provenant des étapes précédentes. Pour cela, chaque étape doit être testée individuellement afin de vérifier que les contrôles d'accès sont appliqués correctement.

Pour tester cela, on peut envoyer des requêtes Burp en utilisant la session courante.

#### Testing with Limited Access

Lorsque l'on test une application et que notre accès est limité, par exemple, un seul compte utilisateur à notre disposition. Il va falloir redoubler d'effort pour tester l'entièreté de l'application. Une fonctionnalité non protégée peut exister sans pour autant se trouver sur notre interface. Par exemple, des fonctionnalités prévues pour d'autres utilisateurs, d'anciennes fonctionnalités ou des nouvelles pas encore intégrées.

Pour tester ces choses-là, nous pouvons utiliser la technique de "Content discovery" expliquée au Chapitre 4. 

#### Testing Direct Access to Methods

Lorsqu'une application utilise des requêtes donnant accès à une API côté serveur, vous devriez découvrir les faiblesses de contrôle d'accès avec les méthodes précédentes. Cependant, vous devriez aussi vérifier l'existence d'API supplémentaires qui pourraient ne pas être correctement protégées.

Pour cela essayer d'identifier les appels d'API existants, get, add, updates etc... Vous pouvez aussi consulter des sources publiques, forum, documentation etc... afin de vous renseigner sur l'API utilisée. Ensuite, tester les appels avec différents paramètres, id etc...

#### Testing Controls Over Static Resources

Dans le cas où l'application peut fournir des ressources statiques, vérifiez que celles-ci sont gérées par contrôle d'accès et qu'elles ne sont pas directement accessible via l'URL.

Pour voir les fichiers d'une application plusieurs outils existent comme par exemple : Sparta qui permet de faire un brute-force sur tous les fichiers et dossiers d'un site web.

#### Testing Restrictions on HTTP Methods

Dernièrement, vous pouvez tester les contrôles d'accès en modifiant les requêtes HTTP (POST, Get, HEAD) ainsi que modifier les cookies. Surtout intéréssant lorsque l'on a des méthodes pour ajouter des utilisateur ou changer le rôle d'un utilisateur.

Pour cela, vous pouvez utiliser Burp.

## Securing Access Controls
La sécurité des contrôle d'accès est la partie la plus simple à comprendre lors de la sécurisation d'une application, mais ce n'est pas pour autant qu'elle doit être sous-estimée. Beaucoup de contrôles ont tendances à être oubliée lors du développement.

Faites attention au :

- Numéro de compte, ID etc... pouvant se trouver dans l'url.
- Query string de style admin=true.
- Ne partez pas du principe que les utilisateurs vont suivre l'ordre des actions que vous avez prévus.
- Vérifiez les contrôles d'accès pas seulement à l'affichage des informations mais aussi lors de la soumission de formulaire.
- Evaluez de manière précise et documentée quelle action est autorisée par quel type d'utilisateur.
- Définissez les contrôles d'accès selon la session.
- Utilisez un système de session sécurisé.
- Dans certains languages, vous pouvez définir des interfaces obligatoires à implémenter sur les différentes pages afin de forcer les développeurs à les programmer.
- Pour les fichiers statiques, on peut y accéder en passant par les noms des fichiers du serveur qui implémentent un contrôle d'accès dynamique ou faire une authentification en utilisant HTTP.

#### A Multilayered Privilege Model

Le contrôle d'accès ne concerne pas seulement l'application web mais aussi les autres niveaux d'infrastructure qui se trouve "en dessous". En particulier, les base de données, système d'exploitation etc...

L'intérêt de protéger toutes les couches est que, si un hacker compromet une couche, il n'aura pas accès à l'intégralité du système, par exemple, si votre site web est compromis, vous devez être sûr que le hacker ne peux pas s'immiscer dans le système d'exploitation.