# 4. Three-tier Architecture vs Hexagonal Architecture

## 🎯 Learning Goal

Understand the fundamental inversion of dependencies in **Hexagonal Architecture** (also known as Ports & Adapters).

## 🧠 Concept

In **Layered Architecture**, dependencies point **down** towards the Database.
`Service -> Database`

In **Hexagonal Architecture**, dependencies point **inward** towards the Domain.
`Service <- Database Adapter`

Wait, what? How can the Service _not_ depend on the DB?
**Dependency Inversion Principle (DIP)**!

The Service defines an **Interface** (Port): "I need a way to save a user."
The Infrastructure implements that Interface: "I am a Postgres Adapter and I can save a user."

This puts the **Business Logic** at the center of the universe, protected from outside changes.

## 💻 Implementation

### Visualizing the difference

**Layered:**
`[Controller] -> [Service] -> [TypeORM]`

**Hexagonal:**
`[Controller] -> [Service] <- [TypeORM Adapter]`

The Service has NO external dependencies. It is pure TypeScript code.

## 🧩 Activity / Challenge

1.  Think: If we switch from SQL to MongoDB...
    - In Layered: We have to rewrite the Service because it probably uses SQL-specific logic or ORM entities.
    - In Hexagonal: We leave the Service alone. We just write a new "Mongo Adapter" that implements the "Repository Port".

## 🔑 Key Takeaways

- **Hexagonal**: Protects the Domain Logic.
- **Ports**: Interfaces defined by the Domain.
- **Adapters**: Implementations defined by the Infrastructure.
