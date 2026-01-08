# 9. Command Query Responsibility Segregation (CQRS)

## 🎯 Learning Goal

Understand why separating **Reads** from **Writes** can simplify complex systems.

## 🧠 Concept

In CRUD, we use the same model for `GET` and `POST`.

- `UserEntity` has a password (needed for Write, bad for Read).
- `UserEntity` needs `orders` relation (needed for Read, redundant for Write).

**CQRS** splits the application into two sides:

1.  **Command Side (Write)**: Focus on business logic, validation, and consistency. Optimized for **Writes**.
2.  **Query Side (Read)**: Focus on fetching data fast. DTOs are flat. Optimized for **Reads**.

## 💻 Implementation

We stop calling Services directly from Controllers. We send **Messages**.

- `CommandBus.execute(new CreateUserCommand(...))`
- `QueryBus.execute(new GetUserQuery(...))`

## 🧩 Activity / Challenge

1.  Draw a diagram.
2.  Controller -> CommandBus -> CommandHandler -> Repository -> DB.
3.  Controller -> QueryBus -> QueryHandler -> DB.

## 🔑 Key Takeaways

- **Complexity**: CQRS adds boilerplate (commands, handlers). Don't use it for simple CRUD.
- **Scalability**: You can scale the Read side (replicas) independently of the Write side (master).
