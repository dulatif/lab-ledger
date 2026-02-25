# 💡 DK03.2.2: Mendefinisikan Services Anda

**Outline:**

- **The Configuration (SEEI):** Deep-diving into the options available for each service.
- **The Lifecycle (PPP):** Defining how a service starts, stops, and restarts.
- **Your Mission (Production):** Building a robust service definition for a Web API.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Configuration (SEEI)**

Think of a Service definition as a "Recipe" for a container. You are telling Docker: "Use this image, set these environment variables, and map these ports."

**Essential Keys:**

- **`image`**: Which pre-built image to use.
- **`build`**: Instead of an image, point to a folder with a `Dockerfile`. Compose will build it for you.
- **`container_name`**: Overriding the default name (useful for scripts, but avoid for scaling).
- **`restart`**: What to do if the container crashes (`always`, `on-failure`, `unless-stopped`).

### **Part 2: Practice - The Lifecycle (PPP)**

**Controlling Container Behavior:**

1. **`command` / `entrypoint`**: Override the default action of the image.
   ```yaml
   command: ["npm", "start"]
   ```
2. **`user`**: Specify which user should run the process (for security).
   ```yaml
   user: "1000:1000"
   ```
3. **`stop_grace_period`**: How long to wait for the app to shut down cleanly before killing it.
   ```yaml
   stop_grace_period: 1m
   ```

**The "Always Restart" Strategy:**
In production, you want your services to stay up.

```yaml
restart: always
```

_Note: If the host reboots, Docker will automatically start this container again._

## 🧠 **Real-World Case Study: "The Infinite Crash Loop"**

- **Scenario:** A developer sets `restart: always` on an app that has a spelling error in its startup command.
- **The Consequence:** The container starts, crashes instantly, and Docker restarts it. This happens 1,000 times per minute, pegging the CPU and filling the logs.
- **The Fix:** Switch to `restart: on-failure` and use `docker compose logs -f` to find the bug before turning on "Always" mode.
- **The Lesson:** `restart: always` is for stable applications. During development, it can hide bugs and waste resources.

## 🤔 **Reflective Questions**

1. What is the difference between `restart: always` and `restart: unless-stopped`?
2. Why should you avoid `container_name` if you plan to use `docker compose up --scale`? (Answer: Because container names must be unique).
3. If both `image` and `build` are provided, what does Compose do?

## 📚 **Further Reading**

- **Docker Reference:** [Service configuration reference](https://docs.docker.com/compose/compose-file/05-services/)
- **Guide:** [Build configuration in Compose](https://docs.docker.com/compose/compose-file/build/)

## 📝 **Mini Task (Production)**

1. Write a service definition for a service named `background-worker`.
2. Requirement:
   - Use the `python:3.9-slim` image.
   - Run the command `python worker.py`.
   - Set an environment variable `QUEUE_NAME` to `orders`.
   - Ensure the container restarts if it fails with an error code.
3. Compare your result with a standard Nginx service definition. What are the key differences?
