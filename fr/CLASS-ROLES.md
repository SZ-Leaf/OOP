# ARCHITECTURE LOGICIELLE — FICHE DE RÉVISION

---

# DOMAINE CENTRAL

## 1. Entité (Entity / Model)

**Rôle :** représenter un objet du métier avec un état

**À retenir :**
- Possède une **identité**
- Contient des **données + un peu de logique**
- Représente un **objet du monde réel**

**Exemple :**
User, Product, Order

**Question clé :**
> Qu'est-ce que c'est ?

---

## 2. Aggregate Root

**Rôle :** point d’entrée d’un groupe d’objets (agrégat)

**À retenir :**
- C’est une **entité spéciale**
- Contrôle **toutes les modifications**
- Garantit la **cohérence métier**
- Les autres objets ne sont accessibles **qu’à travers elle**

**Exemple :**
Order qui gère ses OrderItems

**Question clé :**
> Par où doivent passer tous les changements ?

---

## 3. Value Object

**Rôle :** représenter une valeur immuable

**À retenir :**
- **Pas d’identité**
- **Immuable**
- Comparé par **valeur**, pas par ID

**Exemple :**
Money, Email, Date

**Question clé :**
> Quelle valeur représente cet objet ?

---

## 4. Domain Service

**Rôle :** logique métier complexe

**À retenir :**
- Logique qui ne tient pas dans une seule entité
- Travaille avec **plusieurs objets**
- Généralement **stateless**

**Exemple :**
Calcul de prix, règles de remise

**Question clé :**
> Où mettre cette règle métier ?

---

# COUCHE APPLICATION

## 5. Application Service

**Rôle :** orchestrer un cas d’usage

**À retenir :**
- Coordonne les objets du domaine
- Ne contient **pas de logique métier complexe**
- Gère le **flux d’un use case**

**Exemple :**
Processus de paiement

**Question clé :**
> Comment s’exécute ce cas d’usage ?

---

## 6. DTO (Data Transfer Object)

**Rôle :** transporter des données

**À retenir :**
- **Pas de logique**
- Juste des **données**
- Sert entre couches (API, service, DB)

**Exemple :**
UserDTO

**Question clé :**
> Comment faire passer ces données ?

---

## 7. Mapper

**Rôle :** transformer les objets

**À retenir :**
- Convertit :
  - Entité → DTO
  - DTO → Entité
- Évite de mélanger les couches

**Exemple :**
User → UserDTO

**Question clé :**
> Comment changer la forme de cet objet ?

---

# COUCHE PRÉSENTATION

## 8. Controller

**Rôle :** point d’entrée (HTTP)

**À retenir :**
- Reçoit les requêtes
- Appelle les services
- Retourne une réponse

**Exemple :**
GET /users

**Question clé :**
> Comment le système est-il appelé ?

---

## 9. Presenter / ViewModel

**Rôle :** préparer les données pour l’extérieur

**À retenir :**
- Formate les données
- Cache le domaine
- Adapté à l’UI / API

**Exemple :**
UserViewModel

**Question clé :**
> À quoi doivent ressembler les données ?

---

# COUCHE INFRASTRUCTURE

## 10. Repository

**Rôle :** accès aux données

**À retenir :**
- Abstraction du stockage
- Retourne des **entités**
- Cache SQL / ORM / API

**Exemple :**
UserRepository

**Question clé :**
> Comment récupérer / sauvegarder ?

---

## 11. Infrastructure Service

**Rôle :** gérer les services externes

**À retenir :**
- Email, SMS, APIs, cloud
- Technique, pas métier
- Caché derrière une interface

**Exemple :**
EmailService

**Question clé :**
> Comment parler aux systèmes externes ?

---

## 12. Adapter / Gateway

**Rôle :** connecter un système externe

**À retenir :**
- Traduit les formats
- Isole les dépendances externes
- Sert d’intermédiaire

**Exemple :**
Paiement via API externe

**Question clé :**
> Comment intégrer sans couplage ?

---

# AVANCÉ (CQRS / EVENT-DRIVEN)

## 13. Command

**Rôle :** représenter une action

**À retenir :**
- Intention
- Données uniquement
- Pas de logique

**Exemple :**
CreateOrderCommand

**Question clé :**
> Quelle action est demandée ?

---

## 14. Command Handler

**Rôle :** exécuter une commande

**À retenir :**
- Contient le flux du use case
- Un handler par commande

**Exemple :**
Créer une commande

**Question clé :**
> Qui exécute l’action ?

---

## 15. Event

**Rôle :** représenter un fait passé

**À retenir :**
- Déjà arrivé
- Immuable
- Peut être interne ou externe

**Exemple :**
OrderPaidEvent

**Question clé :**
> Que s’est-il passé ?

---

## 16. Event Handler

**Rôle :** réagir aux événements

**À retenir :**
- Déclenche des actions
- Découple le système
- Gère les effets de bord

**Exemple :**
Envoyer un email après paiement

**Question clé :**
> Que faire après cet événement ?

---

# RÉSUMÉ RAPIDE

| Type                | Rôle principal                  |
|--------------------|-------------------------------|
| Entité             | Objet métier                  |
| Aggregate Root     | Gardien de cohérence          |
| Value Object       | Valeur immuable               |
| Domain Service     | Logique métier transverse     |
| Application Service| Orchestration                 |
| DTO                | Transport de données          |
| Mapper             | Transformation                |
| Controller         | Entrée du système             |
| Presenter          | Format de sortie              |
| Repository         | Accès aux données             |
| Infra Service      | Services externes             |
| Adapter            | Intégration externe           |
| Command            | Intention                     |
| Handler            | Exécution                     |
| Event              | Fait passé                    |
| Event Handler      | Réaction                      |