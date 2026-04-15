# CLEAN ARCHITECTURE

**Structural Foundation of a Backend System**
Clean Architecture defines how a single application is structured internally. It can be used together with architectural and design approaches such as DDD, CQRS, Repository Pattern, and Event-Driven Architecture. It is commonly applied inside each service in a microservices system.

👉 Organizes a backend system into layers so business logic stays independent from technical details.

🏗️ Clean Architecture protects core business logic from the **OUTSIDE**. **no DB in domain, no HTTP in domain, no framework in domain**

---

## 🧠 What problem does it solve?

Without a clear structure, systems end up like this:
- business logic mixed with database queries
- controllers that contain domain rules
- changing a library breaks unrelated parts of the system

Clean Architecture solves this by **separating concerns into layers** and enforcing strict rules about who can talk to whom.

---

## 🧠 Core idea

👉 Dependencies should always point **inward** toward the Core Domain.

The outer layers know about the inner layers.
The inner layers know nothing about the outer layers.

This means:
- core business logic stays independent
- external systems (database, email, HTTP) can be replaced without touching core business logic
- the system remains testable and flexible

---

## 🔑 Key principle behind it

👉 **Dependency Inversion Principle**: high-level modules (business logic) should not depend on low-level modules (technical details). Both should depend on abstractions (interfaces).

In practice: the Application layer defines an interface (e.g. `UserRepositoryInterface`), and the Infrastructure layer implements it. The domain never knows which database is used.

NOTE: Interfaces as the bridge between layers are covered in depth in [ABSTRACTIONS.md](ABSTRACTIONS.md).

---

## 🗂️ The 4 Layers

NOTE: The class roles mentioned in each layer are covered in detail in [CLASS-ROLES.md](CLASS-ROLES.md).

---

## 🧱 1. Core Domain (Innermost Layer)

What it contains:
- Entities
- Aggregate Roots
- Value Objects
- Domain Services

Rules:
- ❌ depends on nothing external
- ✔ contains pure business logic only
- ✔ must NOT know anything about frameworks, databases, or HTTP

🧐 Think:
"Business rules must not depend on technical details"

---

## 🟨 2. Application Layer

What it contains:
- Application Services
- DTOs
- Mappers
- (Interfaces/contracts for Infrastructure — implemented elsewhere)

Rules:
- ✔ depends on Core Domain
- ❌ does NOT depend on Infrastructure or Frameworks directly
- ✔ coordinates domain objects and use cases
- ✔ defines interfaces that Infrastructure must implement
- ❌ must NOT contain core business rules (those belong to the Domain)

🧐 Think:
"Use cases depend on business rules, not on technical details"

---

## 🟩 3. Interface Layer (API / Entry Point)

What it contains:
- Controllers (API Requests / CLI Requests)
- Output (API Responses / CLI Responses)

Rules:
- ✔ depends on Application Layer
- ✔ receives external input (HTTP, CLI, etc.)
- ❌ does NOT contain business logic

🧐 Think:
"Input/output should not contain domain rules"

---

## 🟪 4. Infrastructure Layer

What it contains:
- Repositories (implementations)
- Infrastructure Services
- Adapters / Gateways

Rules:
- ✔ depends on Core Domain (implements its interfaces)
- ✔ depends on Application abstractions (implements its interfaces)
- ✔ implements all technical details
- ❌ must NOT contain business rules

🧐 Think:
"Technical details support the domain, not define it"

---

## 🔁 5. Dependency Flow (Golden Rule)

```
Interface Layer  →  Application Layer  →  Core Domain
Infrastructure   →  Core Domain (implements its abstractions)
```

👉 Notice: Infrastructure does NOT sit "above" anything. It plugs into the system from the outside by implementing interfaces defined in the inner layers.

---

## 🧪 Why this makes testing easier

Because the Core Domain depends on nothing external, you can test business logic in complete isolation — no database, no HTTP, no framework needed.

You replace real infrastructure implementations with fake ones (mocks/stubs) in tests.

---

## ⚠️ Common mistakes to avoid

| Mistake | Why it breaks Clean Architecture |
|---|---|
| Controller contains business logic | Mixes presentation with domain rules |
| Domain Service imports a database library | Core Domain depends on infrastructure |
| Application Service calls SQL directly | Bypasses the Repository abstraction |
| Infrastructure knows about Controllers | Dependency pointing the wrong direction |
