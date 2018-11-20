# Chap. 8, Attacking Access Controls

## Vulnérabilité  commune

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
    un utilisateur arrive à exploiter une faille dans l'état actuel de la l'application pour gagner l'accès à des ressources clés, exemple bypasser l'étape de payement

#### Fontionnalité non protégées

Les pages avec des fonctionnalités particulières comme des pages admin doivent être protégée de l'accès des utilisateurs standard. Une technique pour éviter l'accès des ces pages et de tester le rôle de l'utilisateur est d'ajouter en fonction les boutons d'accès. Il n'y a alors d'autre protection pour l'accès au page protégées que l'affichage visuel d'un bouton permettant d'y accéder.
Ce genre d'affichage peut être géré côté serveur ou coté client depuis JavaScript.
Dans ce dernier cas il est encore plus facile pour un attaquant de trouver les pages en question, car leur URL est disponnible dans le fichier en claire du côté client.

#### Accès direct au méthodes

Dans le cadre d'un API par exemple il est possible de rendre disponible certaines fonction pour tous. Il  faut alors s'assurer que les autres méthodes sensibles ne soient pas également accessible. Si l'accès n'est pas restreint, il sera possible à n'importe qui de deviner les noms de fonction et de les utiliser.

#### Accès par identifiant

Une manière commune d'accès à des données est de passer l'identifiant de la ressource dans l'url. c'est  une manière simple de procéder, mais il faut s'assurer qu'un utilisateur n'ai accès au ressources qui lui sont autorisées. Ce genre de vulnérabilité intervient dans le cadre de la  vulnérabilité horizontale. 

#### Multistage Functions

La gestion d'application avec des étapes multiples nécessitent une attention particulière. En effet, certaines action vont traverser un certains nombre d'étapes successive pour valider une action. Il faut dans ce cas de figure s'assurer que l'utilisateur et les droits d'effectuer chacune des étapes. On ne peut pas partir du principe qu'il est passé par les étapes précédentes. 
Pour la modification d'un utilisateur, si on fait la validation d'authentification sur le requête get qui affiche le formulaire mais pas sur la méthode post qui fait la modification. Il est alors possible de n'envoyer que la requête post et donc appliquer la modification directement.

#### Fichiers statiques

Si des fichiers sont accessible en téléchargement direct depuis le site, pour des site de téléchargement d'Ebooks par exemple. Il existe donc un lien qui pointe directement vers le fichier. Quand on va sur ce lien, il ne fait que pointer vers une ressource du serveur en accès libre et aucune logique de validation ne peut être implémenté entre les deux. Le serveur va simplement retourner la ressource demandée. Après avoir téléchargé un fichier il nous est possible de deviner les noms des autres fichiers et les télécharger et faisant directement appel à la ressource.
Tout type de document pouvant être téléchargé est sujet à ce risque.

#### Mauvaise configuration

Certaine méthodologie d'authentification sont valide mais comporte des vulnérabilités de par leur mise en application. En interdisant par exemple les requêtes POST pour les utilisateur qui ne sont pas admin. On se dit que c'est une bonne idée vu qu'il faut faire en général un appel de POST pour faire une modification. Si l'application est mal configurée il sera dans certains cas possible de faire les actions POST avec une requête GET.
Certains applications pourraient utiliser GET pour faire des modification en utilisant les query strings par exemple. Ce n'est pas une bonne pratique mais c'est possible. Il faudrait alors bloquer les requêtes GET également. Mais il est possible est de faire une requête HEAD qui souvent effectue le code d'une requête GET mais ne retourne que l'entête. Si la requête GET est blockée mais que des données sont traitée à l'intérieure, il est possible d'activer ce process sans recevoir de requête avec HEAD. L'action aura cependant été faite quand même.

#### Méthodologie Vulnérable

Certaine méthodologie d’authentification sont fondamentalement erronées. Elle ont pour but de valider si un utilisateur est admin ou non. Celle-ci sont cependant mal pensée et ne permettent pas de valider sont authenticité de manière suffisamment fiable. 

##### Lié au paramètres

Les paramètres qui sont transmis pas le client peuvent servire d'authentification. Le problème est que cette sécurité est faite côté client. C'est est une mauvaise manière de faire du fait qu'il est possible pour n'importe quel utilisateur de modifier ce qui est fait sur ça machine. Il peut ainsi passer un flag "isAdmin" à true sans problème avant de retourner la requête. Cela peut aller de la query string dans l'url au paramètres dans le cookies.

##### Lié au "Referer"

Le référer indique dans l'entête quelle est la page précédent. Il est alors possible de vérifier que la page précédente est la page du menu "Admin". Si l'authentification est validé pour l'accès à cette page on peut alors supposer que l'utilisateur s'est authentifier et est un administrateur. Cette manière est liée aux paramètres modifiables par l'utilisateur. En effet l'entête d'une requêtes est modifiable à la guise de l'utilisateur.

##### Selon la localisation

Pour restreindre l'accès à une ressource certaines application se base sur la position géographique d'un utilisateur. On va par exemple restreindre la rediffusion des émissions pour les personnes hors du pays. C'est par exemple ce que fait TFI, pour limiter l'accès au live en ligne. Il suffit alors d'utiliser un proxy ou un VPN pour contourner ce contôle.

## Attacking Access Controls
#### Testing with Different User Accounts
#### Testing Multistage Processes
#### Testing with Limited Access
#### Testing Direct Access to Methods
#### Testing Controls Over Static Resources
#### Testing Restrictions on HTTP Methods
## Securing Access Controls
#### A Multilayered Privilege Model