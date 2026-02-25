# 💡 DK03.4.2: Mengontrol Urutan dengan depends_on

**Outline:**

- **The Order (SEEI):** Understanding why some containers must wait for others.
- **The Health (PPP):** Using `depends_on` and `healthcheck` to ensure the stack is truly "ready."
- **Your Mission (Production):** Fixing the "Race Condition" in a multi-service stack.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Order (SEEI)**

**The Race Condition**
When you run `up`, Docker Compose starts all containers at the same time. However, a Web App often crashes if it tries to connect to a Database that is still booting up.

**The Solution:**
`depends_on` tells Compose: "This service is a pre-requisite for that one."

### **Part 2: Practice - The Health (PPP)**

**Basic vs. Advanced Usage:**

1. **Simple Order (V1):**

   ```yaml
   services:
     web:
       image: node-app
       depends_on:
         - db
     db:
       image: postgres
   ```

   _The web container starts AFTER the db container starts. But wait... "Started" doesn't mean "Ready to accept queries"!_

2. **Wait for Health (Recommended):**
   _A better way is to wait until the database reports it is healthy._
   ```yaml
   services:
     web:
       image: node-app
       depends_on:
         db:
           condition: service_healthy
     db:
       image: postgres
       healthcheck:
         test: ["CMD-SHELL", "pg_isready -U postgres"]
         interval: 10s
         timeout: 5s
         retries: 5
   ```

## 🧠 **Real-World Case Study: "The Database Warm-up"**

- **Scenario:** A developer has an app that crashes on start because the database takes 30 seconds to initialize its schema.
- **The First Try:** Using `sleep 30` in the app's startup script. (This is fragile and wastes time if the DB is fast).
- **The Elegant Fix:** Implementing a proper `healthcheck` in the database service and using `condition: service_healthy` in the app's `depends_on`.
- **The Result:** The app starts exactly at the moment the database is ready. No wasted time, no crashes.
- **The Lesson:** Relying on Docker's health engine is always better than hardcoded sleep timers.

## 🤔 **Reflective Questions**

1. If service A `depends_on` service B, and I run `docker compose up A`, will service B also start? (Answer: Yes!).
2. Does `depends_on` work across different `docker-compose.yml` files?
3. What happens if the `healthcheck` fails after the maximum number of retries?

## 📚 **Further Reading**

- **Docker Orientation:** [Control startup and shutdown order](https://docs.docker.com/compose/startup-order/)
- **Guide:** [Healthcheck in Compose](https://docs.docker.com/compose/compose-file/05-services/#healthcheck)

## 📝 **Mini Task (Production)**

1. Write a YAML snippet where an `api` service depends on a `redis` service.
2. Add a simple `healthcheck` for redis using the command `redis-cli ping`.
3. Set the `interval` to 5 seconds and `retries` to 3.
4. Explain why the `api` service should use `condition: service_healthy` instead of just a simple list.
5. Predict what happens if you manually stop the `redis` container while the `api` is still starting.
