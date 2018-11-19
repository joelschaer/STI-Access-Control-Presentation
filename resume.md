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
#### Testing Multistage Processes
#### Testing with Limited Access
#### Testing Direct Access to Methods
#### Testing Controls Over Static Resources
#### Testing Restrictions on HTTP Methods
## Securing Access Controls
#### A Multilayered Privilege Model