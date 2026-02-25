# 💡 DK03.3.1: Menghidupkan Semuanya (up)

**Outline:**

- **The Command (SEEI):** Using `docker compose up` to orchestrate your application stack.
- **The Foreground vs. Background (PPP):** Understanding the `-d` flag and log streaming.
- **Your Mission (Production):** Launching and observing a multi-container environment.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Command (SEEI)**

**The "Go" Button**
`docker compose up` is the most frequently used command in the Compose CLI. It is the command that reads your `yml` file and makes it a reality.

**What it actually does:**

1. **Aggregates Logs:** If run in the foreground, you see colorful logs from every service.
2. **Maintains State:** It checks if containers are already running and only recreates them if the configuration has changed.
3. **Implicit Build:** If you are using the `build` key, `up` will automatically trigger a build if the image doesn't exist.

### **Part 2: Practice - The Foreground vs. Background (PPP)**

**Two Ways to Run:**

1. **Foreground (Interactive):**

   ```bash
   docker compose up
   ```

   _Pros:_ You see errors immediately. Great for debugging.
   _Cons:_ Your terminal is locked. If you press `Ctrl+C`, the stack stops.

2. **Detached Mode (Background):**
   ```bash
   docker compose up -d
   ```
   _Pros:_ Your terminal is freed. Containers stay running even if you close the terminal.
   _Cons:_ You have to run a separate command to see what's happening (e.g., `docker compose logs`).

**The "Force Recreation":**
If you want to force Compose to restart everything regardless of status:

```bash
docker compose up -d --force-recreate
```

## 🧠 **Real-World Case Study: "The Silent Crash"**

- **Scenario:** A developer runs `docker compose up -d` and assumes the app is fine because no errors appeared in the terminal.
- **The Issue:** The database container crashed 2 seconds later because of a missing environment variable.
- **The Fix:** The developer runs `docker compose logs -f` and sees the error message.
- **The Lesson:** Detached mode (`-d`) is for stability. During the first launch of a new stack, always run in the foreground or check the logs immediately.

## 🤔 **Reflective Questions**

1. What happens if you run `docker compose up` when some containers are already running?
2. How do you stop a stack that was started in the foreground?
3. What is the benefit of the colorful prefixes in the Docker Compose logs?

## 📚 **Further Reading**

- **Docker CLI:** [docker compose up reference](https://docs.docker.com/compose/reference/up/)
- **Guide:** [Overview of docker compose up](https://docs.docker.com/compose/cli-command/#docker-compose-up)

## 📝 **Mini Task (Production)**

1. Create a `docker-compose.yml` with two `alpine` services running `sleep 1000`.
2. Run `docker compose up` (without `-d`).
3. Notice how you can't type any more commands.
4. Press `Ctrl+C`. Did the containers stop? (Check with `docker ps`).
5. Now run `docker compose up -d`.
6. Run `docker ps`. Your stack is now running in the background.
7. Why is `-d` the preferred way to run services on a production server?
