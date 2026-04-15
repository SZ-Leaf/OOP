# CORE DOMAIN

## 1. Entity (or Model) Classes

👉 Purpose: represent data and domain state

Example:
A User class that contains a private name property and a method that returns the name.

What they are:

- Real-world “things”
- Mostly data with simple logic
- Often mapped to database rows (in persistence layers)

🧐 Think:
“What is it?”

NOTE: Some entities are Aggregate Roots (public consistency boundary), most are not.

---

## 2. Aggregate Root Classes (Specialized Entity)

👉 Purpose: act as the main entry point and consistency boundary for a group of related domain objects (an aggregate)

Example:
An Order class that manages OrderItems and OrderAddresses, ensuring all changes go through it (e.g. "An order can only have one shipping address").

What they are:

- A special type of Entity
- The single entry point to an aggregate
- Responsible for enforcing consistency and business rules
- May contain other entities and value objects
- External code should only interact with the aggregate through the root

🧐 Think:
“Through which object should all changes to this group be controlled?”

---

## 3. Value Object Classes

👉 Purpose: represent immutable values

Example:
A Money object that contains an amount and a currency and does not change after creation.

What they are:

- No identity
- Represent concepts like money, date, or email
- Always immutable

🧐 Think:
“What value is this?”

---

## 4. Domain Service Classes

👉 Purpose: contain domain logic that does not belong to a single entity

Example:
A PricingService that calculates discounts based on multiple entities like User, Order, and Product.

What they are:

- Domain-level business logic
- Used when logic doesn’t naturally fit inside one Entity or Value Object
- Operate on multiple domain objects
- Stateless in most cases

🧐 Think:
“Where does this business rule belong when no single object owns it?”

---

# APPLICATION LAYER

## 5. Application Service Classes

👉 Purpose: Orchestrate use cases and coordinate domain logic

Example:
A CheckoutService that handles the checkout process by coordinating multiple parts of the system.

What they are:

- Orchestration/use-case coordination
- Coordinate multiple domain objects
- Stateless in most cases (per-request scope, not because they're pure domain operations)

🧐 Think:
“How is this use case executed?”

---

## 6. DTO (Data Transfer Object) Classes

👉 Purpose: transfer data between layers

Example:
A UserDTO that carries a name and email between the API, service layer, and database.

What they are:

- Simple data containers
- No business logic
- Used to move data across system boundaries

🧐 Think:
“How do I carry data?”

---

## 7. Mapper Classes

👉 Purpose: convert between different object representations

Example:
A UserMapper that converts a User entity into a UserDTO and vice versa.

What they are:

- Translate between layers (Entity ↔ DTO ↔ API models)
- Keep transformation logic centralized
- Prevent leaking domain models into external layers
- Pure transformation logic

🧐 Think:
“How do I convert this object into another form?”

NOTE: Mapping always exists. Frameworks may automate it.

---

# INTERFACE / PRESENTATION LAYER

## 8. Controller Classes

👉 Purpose: Entry point for external interactions (commonly HTTP)

Example:
A UserController that receives a request to show a user and delegates to a service to get the data.

What they are:

- Receive external requests
- Call services
- Return responses

🧐 Think:
“How does the outside world interact with the system?”

---

## 9. Presenter (MVP) / ViewModel Classes (MVVM)

👉 Purpose: shape data for presentation layer

Example:
A UserViewModel that formats user data specifically for API responses.

What they are:

- Format data for UI or API responses
- Hide internal domain structure
- Tailored for consumption (not storage or domain logic)
- Often used in MVP, MVVM, or Clean Architecture.

🧐 Think:
“How should this data look to the outside world?”

---

# INFRATSRUCTURE LAYER (External Systems & Persistence)

👉 Handles communication with external systems (APIs, services) and persistence layers (database, cache)

## 10. Repository Classes

👉 Purpose: provide an abstraction to retrieve and persist domain objects

Example:
An abstraction over data storage (could be API, cache, SQL, etc.)

What they are:

- Storage-agnostic access layer
- Hide SQL or ORM complexity
- Return domain entities

🧐 Think:
“How do I get or save data?”

---

## 11. Infrastructure Service Classes

👉 Purpose: handle external systems and technical concerns

Example:
An EmailService that sends emails using an external SMTP provider.

What they are:

- Integrations with external systems (email, SMS, APIs, cloud services)
- Technical implementation details
- Not part of core business logic
- Often hidden behind interfaces

🧐 Think:
“How do I talk to external systems?”

---

## 12. Adapter / Gateway Classes

👉 Purpose: act as a bridge between the application and external systems or incompatible interfaces

Example:
A PaymentGatewayAdapter that converts the application’s internal payment request into the format required by an external payment provider API.

What they are:

- Bridge between your system and external services (APIs, third-party libraries, legacy systems)
- Translate or adapt external contracts into internal ones (and sometimes vice versa)
- Hide external implementation details behind a stable internal interface
- Often used in infrastructure layer or integration layer

🧐 Think:
“How do I make an external system work with my application without coupling to it?”

---

# ADVANCED/OPTIONAL CLASS ROLES

👉 These roles are most common in CQRS, event-driven architecture, modular monoliths, and microservices; they are optional in simple CRUD/layered applications.

## 13. Command Classes

👉 Purpose: represent an intent to perform an action in the system

Example:
A CreateOrderCommand that carries the customerId, items, and shippingAddress needed to create an order.

What they are:

- Intent objects that describe a requested action
- Usually contain only input data (no business logic)
- Often used in CQRS or use-case-driven architectures

🧐 Think:
“What action is being requested?”

---

## 14. Command Handler Classes

👉 Purpose: execute a command by coordinating the required use-case flow

Example:
A CreateOrderCommandHandler that validates input, loads domain objects, calls domain behavior, and persists the result.

What they are:

- Use-case executors for specific commands
- Coordinate application flow and dependencies
- Usually one handler per command

🧐 Think:
“Who executes this requested action?”

---

## 15. Event Classes

👉 Purpose: represent something that already happened in the domain or system

Example:
An OrderPaidEvent emitted after an order payment is successfully completed.

What they are:

- Past-tense facts (something already occurred)
- Immutable data describing that occurrence
- Can be domain events (internal) or integration events (external communication)

🧐 Think:
“What happened?”

---

## 16. Event Handler Classes

👉 Purpose: react to events and trigger follow-up actions

Example:
An OrderPaidEventHandler that sends a confirmation email and updates a reporting read model.

What they are:

- Subscribers/listeners that respond to events
- Encapsulate side effects and follow-up workflows
- Help decouple the producer of an event from its consumers

🧐 Think:
“What should happen because this event occurred?”

---

