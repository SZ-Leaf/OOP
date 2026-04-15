# DOMAINE CENTRAL

## 1. Classes Entité (ou Model)

👉 But : représenter les données et l'état du domaine

Exemple :
Une classe User qui contient une propriété privée name et une méthode qui renvoie ce nom.

Ce qu'elles sont :

- Des "objets" du monde réel
- Principalement des données avec une logique simple
- Souvent mappées aux lignes de base de données (dans les couches de persistance)

🧐 Pense :
"Qu'est-ce que c'est ?"

NOTE : Certaines entités sont des Aggregate Roots (frontière publique de cohérence), la plupart ne le sont pas.

---

## 2. Classes Aggregate Root (Entité spécialisée)

👉 But : agir comme point d'entrée principal et frontière de cohérence pour un groupe d'objets de domaine liés (un agrégat)

Exemple :
Une classe Order qui gère OrderItems et OrderAddresses, en garantissant que tous les changements passent par elle (ex. "Une commande ne peut avoir qu'une seule adresse de livraison").

Ce qu'elles sont :

- Un type spécial d'entité
- Le point d'entrée unique vers un agrégat
- Responsables de faire respecter la cohérence et les règles métier
- Peuvent contenir d'autres entités et value objects
- Le code externe ne doit interagir avec l'agrégat qu'à travers la racine

🧐 Pense :
"À travers quel objet tous les changements de ce groupe doivent-ils être contrôlés ?"

---

## 3. Classes Value Object

👉 But : représenter des valeurs immuables

Exemple :
Un objet Money qui contient un montant et une devise et qui ne change pas après création.

Ce qu'elles sont :

- Sans identité
- Représentent des concepts comme l'argent, la date ou l'email
- Toujours immuables

🧐 Pense :
"Quelle valeur est-ce ?"

---

## 4. Classes Domain Service

👉 But : contenir la logique de domaine qui n'appartient pas à une seule entité

Exemple :
Un PricingService qui calcule des remises en se basant sur plusieurs entités comme User, Order et Product.

Ce qu'elles sont :

- Logique métier au niveau domaine
- Utilisées quand la logique ne s'intègre pas naturellement dans une seule Entité ou un seul Value Object
- Opèrent sur plusieurs objets de domaine
- Sans état dans la plupart des cas

🧐 Pense :
"Où placer cette règle métier quand aucun objet unique ne la possède ?"

---

# COUCHE APPLICATION

## 5. Classes Application Service

👉 But : orchestrer les cas d'usage et coordonner la logique de domaine

Exemple :
Un CheckoutService qui gère le processus de paiement en coordonnant plusieurs parties du système.

Ce qu'elles sont :

- Orchestration/coordination des cas d'usage
- Coordonnent plusieurs objets de domaine
- Sans état dans la plupart des cas (portée par requête, pas parce qu'elles sont des opérations de domaine pures)

🧐 Pense :
"Comment ce cas d'usage est-il exécuté ?"

---

## 6. Classes DTO (Data Transfer Object)

👉 But : transférer des données entre les couches

Exemple :
Un UserDTO qui transporte un nom et un email entre l'API, la couche service et la base de données.

Ce qu'elles sont :

- Conteneurs de données simples
- Sans logique métier
- Utilisées pour déplacer des données à travers les frontières du système

🧐 Pense :
"Comment transporter ces données ?"

---

## 7. Classes Mapper

👉 But : convertir entre différentes représentations d'objets

Exemple :
Un UserMapper qui convertit une entité User en UserDTO et inversement.

Ce qu'elles sont :

- Traduction entre les couches (Entité ↔ DTO ↔ modèles API)
- Centralisent la logique de transformation
- Évitent les fuites de modèles de domaine vers les couches externes
- Logique de transformation pure

🧐 Pense :
"Comment convertir cet objet vers une autre forme ?"

NOTE : Le mapping existe toujours. Les frameworks peuvent l'automatiser.

---

# COUCHE INTERFACE / PRÉSENTATION

## 8. Classes Controller

👉 But : point d'entrée des interactions externes (souvent HTTP)

Exemple :
Un UserController qui reçoit une requête pour afficher un utilisateur et délègue à un service la récupération des données.

Ce qu'elles sont :

- Reçoivent les requêtes externes
- Appellent les services
- Renvoient des réponses

🧐 Pense :
"Comment le monde extérieur interagit-il avec le système ?"

---

## 9. Classes Presenter (MVP) / ViewModel (MVVM)

👉 But : façonner les données pour la couche de présentation

Exemple :
Un UserViewModel qui formate les données utilisateur spécifiquement pour les réponses API.

Ce qu'elles sont :

- Formatent les données pour l'UI ou les réponses API
- Cachent la structure interne du domaine
- Adaptées à la consommation (pas au stockage ni à la logique de domaine)
- Souvent utilisées en MVP, MVVM, ou Clean Architecture.

🧐 Pense :
"À quoi ces données doivent-elles ressembler pour le monde extérieur ?"

---

# COUCHE INFRASTRUCTURE (Systèmes externes et persistance)

👉 Gère la communication avec les systèmes externes (APIs, services) et les couches de persistance (base de données, cache)

## 10. Classes Repository

👉 But : fournir une abstraction pour récupérer et persister des objets de domaine

Exemple :
Une abstraction au-dessus du stockage de données (peut être API, cache, SQL, etc.)

Ce qu'elles sont :

- Couche d'accès indépendante du type de stockage
- Cachent la complexité SQL ou ORM
- Renvoient des entités de domaine

🧐 Pense :
"Comment récupérer ou sauvegarder des données ?"

---

## 11. Classes Infrastructure Service

👉 But : gérer les systèmes externes et les préoccupations techniques

Exemple :
Un EmailService qui envoie des emails via un fournisseur SMTP externe.

Ce qu'elles sont :

- Intégrations avec les systèmes externes (email, SMS, APIs, services cloud)
- Détails d'implémentation techniques
- Ne font pas partie de la logique métier centrale
- Souvent cachées derrière des interfaces

🧐 Pense :
"Comment communiquer avec les systèmes externes ?"

---

## 12. Classes Adapter / Gateway

👉 But : servir de pont entre l'application et des systèmes externes ou des interfaces incompatibles

Exemple :
Un PaymentGatewayAdapter qui convertit la requête de paiement interne de l'application vers le format attendu par l'API d'un fournisseur de paiement externe.

Ce qu'elles sont :

- Pont entre ton système et des services externes (APIs, bibliothèques tierces, systèmes legacy)
- Traduisent ou adaptent des contrats externes en contrats internes (et parfois l'inverse)
- Cachent les détails d'implémentation externes derrière une interface interne stable
- Souvent utilisées dans la couche infrastructure ou d'intégration

🧐 Pense :
"Comment faire fonctionner un système externe avec mon application sans couplage ?"

---

# RÔLES DE CLASSES AVANCÉS/OPTIONNELS

👉 Ces rôles sont surtout courants en CQRS, architecture orientée événements, monolithes modulaires et microservices ; ils sont optionnels dans les applications CRUD/en couches simples.

## 13. Classes Command

👉 But : représenter une intention d'exécuter une action dans le système

Exemple :
Un CreateOrderCommand qui transporte customerId, items, et shippingAddress nécessaires pour créer une commande.

Ce qu'elles sont :

- Objets d'intention qui décrivent une action demandée
- Contiennent généralement seulement des données d'entrée (pas de logique métier)
- Souvent utilisées en CQRS ou en architecture orientée cas d'usage

🧐 Pense :
"Quelle action est demandée ?"

---

## 14. Classes Command Handler

👉 But : exécuter une commande en coordonnant le flux du cas d'usage requis

Exemple :
Un CreateOrderCommandHandler qui valide les entrées, charge les objets de domaine, appelle le comportement de domaine, et persiste le résultat.

Ce qu'elles sont :

- Exécuteurs de cas d'usage pour des commandes spécifiques
- Coordonnent le flux applicatif et les dépendances
- Généralement un handler par commande

🧐 Pense :
"Qui exécute cette action demandée ?"

---

## 15. Classes Event

👉 But : représenter quelque chose qui s'est déjà produit dans le domaine ou le système

Exemple :
Un OrderPaidEvent émis après qu'un paiement de commande est terminé avec succès.

Ce qu'elles sont :

- Des faits au passé (quelque chose s'est déjà produit)
- Données immuables décrivant cette occurrence
- Peuvent être des événements de domaine (internes) ou d'intégration (communication externe)

🧐 Pense :
"Qu'est-ce qui s'est passé ?"

---

## 16. Classes Event Handler

👉 But : réagir aux événements et déclencher des actions de suivi

Exemple :
Un OrderPaidEventHandler qui envoie un email de confirmation et met à jour un modèle de lecture de reporting.

Ce qu'elles sont :

- Abonnés/listeners qui réagissent aux événements
- Encapsulent les effets de bord et les workflows de suivi
- Aident à découpler le producteur d'un événement de ses consommateurs

🧐 Pense :
"Que doit-il se passer parce que cet événement a eu lieu ?"

---
