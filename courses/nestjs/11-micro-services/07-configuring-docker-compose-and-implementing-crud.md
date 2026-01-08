# 7. Configuring Docker Compose & Implementing CRUD

## 🎯 Learning Goal

Orchestrate the entire infrastructure locally.

## 🧠 Concept

We need:

1.  Gateway (Port 3000).
2.  Orders Service (Port 3001).
3.  Orders DB (Postgres).
4.  Notifications Service (Port 3002).
5.  Notifications DB (Mongo).

## 💻 Implementation

```yaml
version: "3"
services:
  orders-db:
    image: postgres
    environment:
      POSTGRES_DB: orders

  notifications-db:
    image: mongo

  # We usually run apps via specific Dockerfiles or just `npm run start:dev` locally
  # hooking into these DB ports.
```

## 🧩 Activity / Challenge

1.  Create `docker-compose.yml`.
2.  Define the DBs.
3.  Configure `TypeOrmModule` in `OrdersModule` to connect to `orders-db`.
4.  Implement `create(dto)` saving to DB.

## 🔑 Key Takeaways

- **Infrastructure as Code**: Keep your environment definition with your code.
