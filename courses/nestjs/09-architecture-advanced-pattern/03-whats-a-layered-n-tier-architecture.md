# 3. What's a Layered (N-tier) Architecture?

## 🎯 Learning Goal

Understand the default architecture used by NestJS (and most web frameworks).

## 🧠 Concept

**N-tier** architecture organizes code into horizontal layers. Requests flow down, Data flows up.

1.  **Presentation Layer** (Controllers): Handles HTTP, validation, parsing.
2.  **Business Logic Layer** (Services): The "Brain". Rules, calculations.
3.  **Data Access Layer** (Repositories/DAOs): Talking to DB, File System, APIs.

### The Dependency Rule ⬇️

Controller depends on Service. Service depends on Repository.
`Controller -> Service -> Repository`

## 💻 Implementation

This is what you've likely been doing already!

```typescript
// cats.controller.ts (Presentation)
@Get()
findAll() { return this.service.findAll(); }

// cats.service.ts (Business)
findAll() { return this.repo.find(); }

// cats.repository.ts (Data)
find() { return db.query('SELECT * ...'); }
```

## 🧩 Activity / Challenge

1.  Look at your `src` folder.
2.  Identify which files belong to which layer.
3.  Draw a diagram (mentally or on paper) of the dependency arrows.

## 🔑 Key Takeaways

- **Pros**: Simple, familiar, standard. Great for CRUD.
- **Cons**:
  - **Transitive Dependencies**: If the DB schema changes, the Repo changes, which might force the Service to change.
  - **Coupling**: The Business logic often becomes tightly coupled to the Database implementation (e.g., using TypeORM entities directly in the Service).
