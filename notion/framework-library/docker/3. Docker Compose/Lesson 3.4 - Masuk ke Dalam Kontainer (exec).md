# 💡 DK03.3.4: Masuk ke Dalam Kontainer (exec)

**Outline:**

- **The Access (SEEI):** Using `docker compose exec` to run commands inside a running service.
- **The Interaction (PPP):** Accessing databases, running migrations, and checking local files.
- **Your Mission (Production):** Executing a manual database query from the CLI.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Access (SEEI)**

**Tunneling in**
Sometimes, logs aren't enough. You might need to check if a file was correctly mounted or run a database CLI tool. `docker compose exec` is your "SSH-like" tunnel into a specific service.

**Why use `compose exec` instead of `docker exec`?**

- You don't need to find the specific random container ID.
- You simply use the service name defined in your YAML:
  ```bash
  docker compose exec [SERVICE_NAME] [COMMAND]
  ```

### **Part 2: Practice - The Interaction (PPP)**

**Common Usage Patterns:**

1. **Open a Shell (Bash/Sh):**
   ```bash
   docker compose exec web sh
   ```
2. **Access a Database CLI:**
   ```bash
   docker compose exec db mysql -u root -p
   ```
3. **Run Package Commands:**
   ```bash
   docker compose exec api npm run migration
   ```
4. **Environment Check:**
   ```bash
   docker compose exec api env
   ```

**The difference between `run` and `exec`:**

- **`exec`**: Runs a command in an **already running** container.
- **`run`**: Creates a **new** container just for that command and then deletes it.
- _Rule of thumb: Use `exec` for everything except one-off setup tasks._

## 🧠 **Real-World Case Study: "The Secret Config"**

- **Scenario:** An application is failing to connect to the DB, even though the environment variables look correct in the YAML.
- **The Debugging:** The developer suspects the `.env` file isn't being read correctly by the container's OS.
- **The Tool:** They run `docker compose exec api env`.
- **The Result:** They see that the variable `DB_PASSWORD` is actually empty inside the container.
- **The Fix:** They realize they had a typo in the `.env` filename host-side.
- **The Lesson:** `exec env` is the ultimate way to verify what your application "sees" at runtime.

## 🤔 **Reflective Questions**

1. What happens if you run `exec` on a service that isn't running?
2. Why is `sh` used more often than `bash` in the Alpine image?
3. How do you exit a shell session inside a container?

## 📚 **Further Reading**

- **Docker CLI:** [docker compose exec reference](https://docs.docker.com/compose/reference/exec/)
- **Guide:** [docker compose run vs exec](https://docs.docker.com/compose/reference/run/)

## 📝 **Mini Task (Production)**

1. Start a stack with a Redis service.
2. Enter the Redis container using exec:
   ```bash
   docker compose exec redis redis-cli
   ```
3. Inside the Redis CLI, run `SET greeting "Hello from Compose"`.
4. Now run `GET greeting`.
5. Exit the CLI: `type exit`.
6. Explain why `exec` is safer than opening ports (like 6379) to your computer just to run a quick test.
