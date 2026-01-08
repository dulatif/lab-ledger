# 7. Onion Architecture?

## 🎯 Learning Goal

Understand **Onion Architecture** and how it relates to Hexagonal.

## 🧠 Concept

Onion Architecture (and "Clean Architecture") is extremely similar to Hexagonal.
They share the same goal: **Dependencies point inwards.**

### The Layers (The Onion Rings 🧅)

1.  **Domain Model** (Center): Entities, Enums. (No dependencies).
2.  **Domain Services**: Logic that doesn't fit in an entity. (Depends on Model).
3.  **Application Services**: Orchestration, Use Cases. (Depends on Domain).
4.  **Outer Layer** (Infrastructure): DB, UI, Tests. (Depends on Application).

## 💻 Implementation

In NestJS, this maps 1:1 with what we just built:

- `domain/` = Center.
- `application/` = Application Services.
- `infrastructure/` = Outer Layer.
- `controllers/` = Outer Layer.

## 🧩 Activity / Challenge

1.  Draw your current folder structure for the `User` module.
2.  Draw circles around them.
3.  Verify: Does `domain` import anything from `infrastructure`? (It shouldn't!).
4.  Verify: Does `infrastructure` import from `domain`? (Yes, it must implement the plain interfaces).

## 🔑 Key Takeaways

- Hexagonal, Onion, and Clean Architecture vary slightly in terminology but are identical in **purpose** and **dependency flow**.
- The "Core" must be framework-agnostic.
- NestJS is just a detail in the Outer Layer (Controllers/Modules).
