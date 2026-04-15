# ARCHITECTURE LOGICIELLE — FICHE DE RÉVISION

---

# CLEAN ARCHITECTURE

## 1. Définition

**Rôle :** structurer une application en couches avec des responsabilités claires

**À retenir :**
- Protège le domaine métier des détails techniques
- Réduit le couplage entre logique métier et infrastructure
- Facilite les tests et l'évolution du système

**Exemple :**
Le domaine ne dépend ni de SQL, ni de HTTP, ni d'un framework

**Question clé :**
> Comment garder la logique métier indépendante de la technique ?

---

## 2. Problème résolu

**Rôle :** éviter les systèmes où tout est mélangé

**À retenir :**
- Évite les contrôleurs avec logique métier
- Évite les services métier couplés à la base de données
- Empêche qu'un changement technique casse des règles métier

**Exemple :**
Remplacer PostgreSQL par MongoDB sans réécrire le domaine

**Question clé :**
> Qu'est-ce qui casse quand je change un détail technique ?

---

## 3. Règle des dépendances

**Rôle :** imposer le sens des dépendances

**À retenir :**
- Les dépendances pointent toujours vers l'intérieur
- Les couches externes connaissent les internes
- Les couches internes ignorent les externes

**Exemple :**
Infrastructure -> Application -> Core Domain

**Question clé :**
> Cette couche dépend-elle d'une couche plus interne ?

---

## 4. Core Domain

**Rôle :** contenir les règles métier centrales

**À retenir :**
- Contient entités, value objects, agrégats, domain services
- Aucun accès direct aux bases de données ou APIs externes
- Doit rester pur et testable en isolation

**Exemple :**
Règle de facturation sans import de librairie SQL

**Question clé :**
> Cette logique exprime-t-elle une règle métier pure ?

---

## 5. Couche Application

**Rôle :** orchestrer les cas d'usage

**À retenir :**
- Coordonne le flux d'un use case
- Utilise le domaine sans contenir le coeur des règles métier
- Définit les interfaces à implémenter côté infrastructure

**Exemple :**
`CreateOrderService` orchestre validation, persistance, publication d'événement

**Question clé :**
> Ce code orchestre-t-il un cas d'usage sans devenir technique ?

---

## 6. Couche Interface

**Rôle :** gérer les entrées/sorties du système

**À retenir :**
- Reçoit requêtes HTTP/CLI/messages
- Appelle la couche application
- Ne décide pas des règles métier

**Exemple :**
Un controller transforme la requête puis délègue à un application service

**Question clé :**
> Ce composant sert-il juste d'adaptateur d'entrée/sortie ?

---

## 7. Couche Infrastructure

**Rôle :** implémenter les détails techniques

**À retenir :**
- Implémente repositories, gateways, services externes
- Dépend des interfaces internes
- Peut être remplacée sans toucher le domaine

**Exemple :**
`SqlUserRepository` implémente `UserRepository`

**Question clé :**
> Ce code est-il un détail technique remplaçable ?

---

## 8. Dependency Inversion Principle

**Rôle :** découpler modules métier et modules techniques

**À retenir :**
- Le haut niveau dépend d'abstractions, pas d'implémentations
- Le bas niveau implémente ces abstractions
- Les contrats appartiennent aux couches internes

**Exemple :**
`UserRepository` défini en application, implémenté en infrastructure

**Question clé :**
> L'interface est-elle au bon endroit (interne) ?

---

## 9. Testabilité

**Rôle :** rendre la logique métier facile à tester

**À retenir :**
- Le domaine se teste sans DB, réseau, framework
- Les dépendances techniques se remplacent par des mocks/stubs
- Les tests métier deviennent rapides et stables

**Exemple :**
Tester la création de commande avec un repository factice en mémoire

**Question clé :**
> Puis-je tester ce comportement sans infrastructure réelle ?

---

## 10. Erreurs courantes

**Rôle :** identifier les violations de l'architecture

**À retenir :**
- Controller avec logique métier
- Application service qui exécute du SQL direct
- Domaine qui importe des librairies techniques

**Exemple :**
Un domain service qui envoie directement un email

**Question clé :**
> Cette responsabilité est-elle dans la bonne couche ?

---

# RÉSUMÉ RAPIDE

| Élément | Rôle principal |
|---|---|
| Core Domain | Règles métier pures |
| Application | Orchestration des cas d'usage |
| Interface | Entrées et sorties |
| Infrastructure | Détails techniques |
| Dépendances | Toujours vers l'intérieur |
| DIP | Contrats internes, implémentations externes |
| Testabilité | Domaine isolable |
| Anti-patterns | Mélange des couches |
