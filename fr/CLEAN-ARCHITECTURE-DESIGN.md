# CLEAN ARCHITECTURE

**Fondation structurelle d'un système backend**
Clean Architecture définit la manière dont une application unique est structurée en interne. Elle peut être utilisée avec des approches d'architecture et de conception comme DDD, CQRS, Repository Pattern et Event-Driven Architecture. Elle est souvent appliquée à l'intérieur de chaque service dans un système microservices.

👉 Elle organise un système backend en couches pour que la logique métier reste indépendante des détails techniques.

🏗️ Clean Architecture protège la logique métier centrale de l'**EXTÉRIEUR**. **pas de DB dans le domaine, pas de HTTP dans le domaine, pas de framework dans le domaine**

---

## 🧠 Quel problème cela résout ?

Sans structure claire, les systèmes finissent comme ceci :
- logique métier mélangée aux requêtes base de données
- contrôleurs qui contiennent des règles de domaine
- changement de bibliothèque qui casse des parties non liées du système

Clean Architecture résout cela en **séparant les responsabilités en couches** et en imposant des règles strictes sur qui peut parler à qui.

---

## 🧠 Idée centrale

👉 Les dépendances doivent toujours pointer **vers l'intérieur** en direction du Core Domain.

Les couches externes connaissent les couches internes.
Les couches internes ne connaissent rien des couches externes.

Cela signifie :
- la logique métier centrale reste indépendante
- les systèmes externes (base de données, email, HTTP) peuvent être remplacés sans toucher à la logique métier centrale
- le système reste testable et flexible

---

## 🔑 Principe clé derrière cette architecture

👉 **Dependency Inversion Principle** : les modules de haut niveau (logique métier) ne doivent pas dépendre des modules de bas niveau (détails techniques). Les deux doivent dépendre d'abstractions (interfaces).

En pratique : la couche Application définit une interface (par ex. `UserRepositoryInterface`) et la couche Infrastructure l'implémente. Le domaine ne sait jamais quelle base de données est utilisée.

NOTE : Les interfaces comme pont entre couches sont expliquées en détail dans [ABSTRACTIONS.md](ABSTRACTIONS.md).

---

## 🗂️ Les 4 couches

NOTE : Les rôles de classes mentionnés dans chaque couche sont détaillés dans [CLASS-ROLES.md](CLASS-ROLES.md).

---

## 🧱 1. Core Domain (couche la plus interne)

Ce qu'elle contient :
- Entités
- Aggregate Roots
- Value Objects
- Domain Services

Règles :
- ❌ ne dépend de rien d'externe
- ✔ contient uniquement de la logique métier pure
- ✔ ne doit RIEN savoir des frameworks, bases de données ou HTTP

🧐 Pense :
"Les règles métier ne doivent pas dépendre de détails techniques"

---

## 🟨 2. Couche Application

Ce qu'elle contient :
- Application Services
- DTOs
- Mappers
- (Interfaces/contrats pour l'Infrastructure — implémentés ailleurs)

Règles :
- ✔ dépend du Core Domain
- ❌ ne dépend PAS directement de l'Infrastructure ni des Frameworks
- ✔ coordonne les objets de domaine et les cas d'usage
- ✔ définit les interfaces que l'Infrastructure doit implémenter
- ❌ ne doit PAS contenir de règles métier centrales (elles appartiennent au Domain)

🧐 Pense :
"Les cas d'usage dépendent des règles métier, pas des détails techniques"

---

## 🟩 3. Couche Interface (API / point d'entrée)

Ce qu'elle contient :
- Controllers (requêtes API / requêtes CLI)
- Output (réponses API / réponses CLI)

Règles :
- ✔ dépend de la couche Application
- ✔ reçoit les entrées externes (HTTP, CLI, etc.)
- ❌ ne contient PAS de logique métier

🧐 Pense :
"Les entrées/sorties ne doivent pas contenir de règles de domaine"

---

## 🟪 4. Couche Infrastructure

Ce qu'elle contient :
- Repositories (implémentations)
- Infrastructure Services
- Adapters / Gateways

Règles :
- ✔ dépend du Core Domain (implémente ses interfaces)
- ✔ dépend des abstractions Application (implémente ses interfaces)
- ✔ implémente tous les détails techniques
- ❌ ne doit PAS contenir de règles métier

🧐 Pense :
"Les détails techniques servent le domaine, ils ne le définissent pas"

---

## 🔁 5. Flux de dépendances (règle d'or)

```
Couche Interface  →  Couche Application  →  Core Domain
Infrastructure    →  Core Domain (implémente ses abstractions)
```

👉 Remarque : l'Infrastructure ne se place PAS "au-dessus" de quoi que ce soit. Elle se branche au système depuis l'extérieur en implémentant les interfaces définies dans les couches internes.

---

## 🧪 Pourquoi cela facilite les tests

Parce que le Core Domain ne dépend de rien d'externe, tu peux tester la logique métier en isolation complète — sans base de données, sans HTTP, sans framework.

Tu remplaces les implémentations d'infrastructure réelles par des versions factices (mocks/stubs) dans les tests.

---

## ⚠️ Erreurs courantes à éviter

| Erreur | Pourquoi cela casse Clean Architecture |
|---|---|
| Le Controller contient de la logique métier | Mélange présentation et règles de domaine |
| Un Domain Service importe une bibliothèque base de données | Le Core Domain dépend de l'infrastructure |
| Un Application Service appelle SQL directement | Contourne l'abstraction Repository |
| L'Infrastructure connaît les Controllers | Dépendance orientée dans la mauvaise direction |
