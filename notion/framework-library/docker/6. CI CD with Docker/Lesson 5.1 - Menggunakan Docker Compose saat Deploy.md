# 💡 DK06.5.1: Menggunakan Docker Compose saat Deploy

**Outline:**

- **The Orchestrator (SEEI):** Understanding why Compose is the easiest way to manage multi-container deployments in CI/CD.
- **The Synchronization (PPP):** Using `docker-compose-prod.yml` to keep development and production configurations separate.
- **Your Mission (Production):** Updating a full stack (Database + API + Frontend) in one command.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Orchestrator (SEEI)**

**"One script to rule them all"**
In Chapter 4, we talked about simple `docker run` deployments. But what if your app needs a Redis cache and a Postgres database?

**The Compose Advantage in CI/CD:**
Instead of sending 5 complex `docker run` commands over SSH, you just send one: `docker-compose up -d`.

- **Atomic:** All services start together.
- **Declarative:** The state of your production is defined in the YAML, not in your memory.
- **Volume Awareness:** Compose knows how to preserve your data even if the container is replaced.

### **Part 2: Practice - The Synchronization (PPP)**

**Production vs Development:**
Your production environment often needs different settings (e.g., higher memory limits, fixed port 80, secure database passwords).

**The "Overlay" Pattern:**
You can keep a base `docker-compose.yml` and a specific `docker-compose.prod.yml`.

```bash
docker-compose -f docker-compose.yml -f docker-compose.prod.yml up -d
```

_Compose will "Merge" the files, giving priority to the production settings._

**The Deployment Script:**

```bash
cd /app
docker-compose pull
docker-compose up -d --remove-orphans
```

`--remove-orphans` is important because it cleans up containers that were once part of your app but are no longer in the YAML file (e.g., if you deleted a service).

## 🧠 **Real-World Case Study: "The Forgotten Cache"**

- **Scenario:** A team updated their API from v1 to v2.
- **The Issue:** They manually ran `docker run` for the new API.
- **The Failure:** They forgot to update the Redis cache container that the new API required. The API kept crashing because it couldn't find the cache.
- **The Fix:** They moved to Docker Compose.
- **The Outcome:** The next time they added a new service (like a Search Engine), they just added it to the `docker-compose.yml`. When the CI/CD ran `up -d`, it automatically pulled and started the Search Engine alongside the API. No services were ever "Forgotten" again.
- **The Lesson:** Group your dependencies. If they act as one app, manage them as one Compose file.

## 🤔 **Reflective Questions**

1. Why shouldn't you keep production passwords inside the `docker-compose.prod.yml` file? (Answer: Because that file is usually committed to Git! Use environment variables instead).
2. What happens if one service fails to start but the others succeed during an `up -d`?
3. How do you handle "Database Migrations" when using Compose in production?

## 📚 **Further Reading**

- **Docker:** [Compose in Production](https://docs.docker.com/compose/production/)
- **Guide:** [Using Multiple Compose Files](https://docs.docker.com/compose/multiple-compose-files/)

## 📝 **Mini Task (Production)**

1. Create a `docker-compose.dev.yml` and a `docker-compose.prod.yml`.
2. In the `dev` version, expose port `8080:80`.
3. In the `prod` version, expose port `80:80`.
4. Research how to run Compose with two `-f` flags.
5. Reflect: How does this prevent you from accidentally exposing your development ports to the public internet?
