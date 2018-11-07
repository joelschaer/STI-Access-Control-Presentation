# Chap. 8, Attacking Access Controls

 71% des applications testées y sont vulnérable.

Décrit les différents manière d'effectuer des actions non authorisées dans une application.



### Commune

catégories :

#### vertical

##### vertical escalation

get higher priviledge,

#### horizontal

##### horizontal escalation

accèder du contenu qui ne vous est pas destiné

#### context-dependent



### Unprotectd Functionality

### Direct Access to Methods

Accès direct aux méthodes depuis l'api

### Identifier-based Functions

Utilisant l'id pour accéder une ressource ( id d'un document ), si on trouve l'id on peut récupérer la ressource.

#### Multistage Functions

Dans une série de formulaires, on sécurise le premier et on considère que si le premier n'est pas accèdé les suivant non plus, mais si on bypasse le premier ...

#### Static Files

Accès direct au ressources par url. ils en donc possible si on connait l'url de dl n'importe quel ressource depuis le server.

#### Platform Misconfiguration

#### Insecure Access Control Methods

L'utilisation d'un model fonctamentallement insécure. 

##### Parameter-Based Access Control

difficile de savoir ce qui est fait si on est pas admin (chap.4 pour la découverte)

##### Referer-base Access Controlcatégories :

vertical
vertical escalation
get higher priviledge,

horizontal
horizontal escalation
accèder du contenu qui ne vous est pas destiné

context-dependent

##### Location-Based Access Control

often based on IP address (bypass):

- proxy
- VPN
- mobile device
- manipulate client side mechanims

## Attacking Access Controls

#### Quesitons to consider

#### Testing with Different User Accounts

#### Testing Multistage Processes

#### Testing Direct Access to Methods

#### Testing Controls Over Static Resources

#### Testing Restrictions on HTTP Methods

## Securing Access Controls

#### A Multilayered Privilege Model

