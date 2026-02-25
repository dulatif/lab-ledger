# 💡 DK03.3.2: Mematikan Semuanya (down)

**Outline:**

- **The Cleanup (SEEI):** Using `docker compose down` to gracefully stop and remove your application stack.
- **The Granularity (PPP):** Understanding what stays and what goes (Containers vs. Networks vs. Volumes).
- **Your Mission (Production):** Safely decommissioning a development environment.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Cleanup (SEEI)**

**The "Undo" Button**
`docker compose down` is the opposite of `up`. It ensures that your host machine doesn't have "ghost" containers or networks lingering after you are done working.

**Why not just use `stop`?**

- `stop`: Only pauses the containers. They still exist and take up memory pointers.
- `down`: Stops the containers, removes them, and deletes the virtual networks. It's a "Fresh Start" command.

**The Implicit Safety:** By default, `down` **does not** delete volumes. This is a safety feature to prevent you from accidentally wiping your database.

### **Part 2: Practice - The Granularity (PPP)**

**Controlling the Destruction:**

1. **Standard Down:**

   ```bash
   docker compose down
   ```

   _Removes: Containers, Networks._

2. **Down with Volumes (The "Wipe" command):**

   ```bash
   docker compose down -v
   ```

   _Removes: Containers, Networks, AND Volumes. Use this for a total reset._

3. **Down with Images:**
   ```bash
   docker compose down --rmi local
   ```
   _Removes: Containers, Networks, and any images built by this project._

## 🧠 **Real-World Case Study: "The Ghost Network"**

- **Scenario:** A developer starts 10 different projects over a week using `docker run` but never cleans up the networks.
- **The Problem:** Eventually, they try to start a new project and get an "Address space already in use" error.
- **The Fix:** They learn to use `docker compose down` at the end of every work session.
- **The Lesson:** Development hygiene is important. `up` to start, `down` to finish. If you don't use `down`, you are leaking system resources.

## 🤔 **Reflective Questions**

1. If you run `docker compose down`, will your `docker-compose.yml` file be deleted? (Answer: No, only the running resources).
2. Why is it important that `down` doesn't delete volumes by default?
3. What is the difference between `docker compose stop` and `docker compose down`?

## 📚 **Further Reading**

- **Docker CLI:** [docker compose down reference](https://docs.docker.com/compose/reference/down/)
- **Guide:** [Best practices for cleaning up](https://docs.docker.com/config/pruning/)

## 📝 **Mini Task (Production)**

1. Start your test stack: `docker compose up -d`.
2. Verify it's running: `docker compose ps`.
3. Stop it: `docker compose stop`.
4. Run `docker ps -a`. Notice the containers still exist in "Exited" status.
5. Now run `docker compose down`.
6. Run `docker ps -a` and `docker network ls`.
7. Confirm that both the containers and the project-specific networks are gone.
8. If you had a volume attached, is it still there? Check with `docker volume ls`.
