# 9. Docker Compose (Defining Services)

## 🎯 Learning Goal

Stop running 5 separate `docker run` commands. Orchestrate multi-container apps.

## 🧠 Concept

**Infinite Wisdom**: "Infrastructure as Code".
`docker-compose.yaml` describes your entire stack configuration.

- Services (App, DB, Cache).
- Networks.
- Volumes.

## 💻 Implementation

`docker-compose.yaml`:

```yaml
version: "3.8"

services:
  web:
    build: . # Build image from current dir
    ports:
      - "3000:3000"
    environment:
      - DB_HOST=db
    depends_on:
      - db

  db:
    image: postgres:15
    environment:
      POSTGRES_PASSWORD: secret
    volumes:
      - db-data:/var/lib/postgresql/data

volumes:
  db-data: # Define managed volume
```

### Commands

- `docker compose up -d`: Build images, create network, volume, and start all containers in background.
- `docker compose down`: Stop and remove containers and network. (Volume persists).
- `docker compose down -v`: Destroys volumes too (Reset data).

## 🧩 Activity / Challenge

1.  Convert the manual networking command from Lesson 3 into a Compose file.
2.  Notice you don't need to manually create the network. Compose creates a `default` network for the stack.

## 🔑 Key Takeaways

- Use Compose for Local Development. (Almost everyone does).
- It serves as documentation for "How to run this project".
