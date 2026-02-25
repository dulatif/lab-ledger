# 💡 DK03.3.3: Memeriksa Status dan Log

**Outline:**

- **The Monitoring (SEEI):** Using `ps` and `logs` to keep an eye on your running services.
- **The Observation (PPP):** Filtering and following logs for specific services.
- **Your Mission (Production):** Diagnosing a bug in a multi-container stack.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Monitoring (SEEI)**

**"Is everything running?"**
Once your stack is up (`-d`), you need a way to check its health. Docker Compose provides specialized commands that are "Project Aware."

**Why not just use `docker ps`?**

- `docker ps`: Shows every container on the system, which can be noisy.
- `docker compose ps`: Shows ONLY the containers belonging to the current project. It's much cleaner!

### **Part 2: Practice - The Observation (PPP)**

**Essential Monitoring Commands:**

1. **List Project Containers:**
   ```bash
   docker compose ps
   ```
2. **View Combined Logs:**
   ```bash
   docker compose logs
   ```
3. **Follow Logs (Live):**
   ```bash
   docker compose logs -f
   ```
4. **Log for a specific service:**
   _If your database is crashing, you don't want to see web logs._
   ```bash
   docker compose logs -f db
   ```

**Identifying Problems:**
Inside `docker compose ps`, look for the "STATUS" column. If it says `Exit 0`, the process finished successfully. If it says `Exit 1` (or any non-zero number), something went wrong.

## 🧠 **Real-World Case Study: "The Intermittent Bug"**

- **Scenario:** A web app works 90% of the time, but occasionally fails to load data. The developer suspects the Database is restarting.
- **The Investigation:** The developer runs `docker compose ps` and sees that the `db` service has been running for only 10 minutes, even though the stack was started 2 hours ago.
- **The Deep Dive:** They run `docker compose logs --tail 50 -f db`.
- **The Discovery:** They see an "Out of Memory" error from the Postgres engine.
- **The Fix:** They increase the memory limit in the `docker-compose.yml` file.
- **The Lesson:** Always check the uptime in `ps` and the tail of the logs in `logs`.

## 🤔 **Reflective Questions**

1. What is the difference between `docker compose ps` and `docker compose top`?
2. Why is the `-f` flag useful when trying to reproduce a bug?
3. Can you view logs from a container that has already stopped?

## 📚 **Further Reading**

- **Docker CLI:** [docker compose ps reference](https://docs.docker.com/compose/reference/ps/)
- **Guide:** [docker compose logs reference](https://docs.docker.com/compose/reference/logs/)

## 📝 **Mini Task (Production)**

1. Start a simple Compose stack.
2. Run `docker compose ps` and find the "Name" and "Status" of your services.
3. Open a second terminal window and run `docker compose logs -f`.
4. In the first terminal, run a command that triggers a log (e.g., visit the web app or run a query).
5. Watch the logs update in real-time in the second window.
6. How do the log prefixes help you distinguish between a Web log and a Database log?
