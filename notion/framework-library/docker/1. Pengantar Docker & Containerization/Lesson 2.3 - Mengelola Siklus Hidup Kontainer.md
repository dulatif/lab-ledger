# 💡 DK02.3: Mengelola Siklus Hidup Kontainer

**Outline:**

- **The Lifecycle (SEEI):** Understanding the different states of a container: Created, Running, Stopped, and Paused.
- **The Management Commands (PPP):** Mastering `stop`, `start`, `restart`, `kill`, and `rm`.
- **Your Mission (Production):** Effectively managing a container fleet.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Lifecycle (SEEI)**

**The Lifetime of a Container**
A container is not just "there" or "gone." It moves through several states:

1. **Created:** The container exists but isn't running yet.
2. **Running:** The process inside the container is active.
3. **Paused:** The process is suspended (freezes the container's state).
4. **Exited (Stopped):** The process has finished or was manually stopped. The container still exists on disk.
5. **Removed:** The container and its file system are deleted completely.

**Visibility:**

- `docker ps`: Shows only **Running** containers.
- `docker ps -a`: Shows **All** containers (including stopped ones).

### **Part 2: Practice - The Management Commands (PPP)**

**Controlling the Beast:**

1. **Stop vs. Kill:**
   - `docker stop <name>`: Sends a signal to the app to "shut down gracefully" (saves data, closes connections).
   - `docker kill <name>`: Pulls the plug immediately.
2. **Restart:**
   - `docker restart <name>`: Equivalent to stop then start.
3. **Removal:**
   - `docker rm <name>`: Deletes a **stopped** container.
   - `docker rm -f <name>`: Forces deletion of a running container.

**Useful Tip: The "Prune" command**

```bash
docker container prune
```

_This command deletes all stopped containers at once. Great for housekeeping!_

## 🧠 **Real-World Case Study: "The Ghost Containers"**

- **The Problem:** A beginner runs `docker run hello-world` 20 times. They run `docker ps` and see nothing. They think their disk is empty.
- **The Discovery:** They run `docker ps -a` and find 20 "Exited" containers taking up space and using names.
- **The Lesson:** Always remember to clean up with `--rm` flag or by using `docker rm` periodically. Stopped containers don't use CPU, but they do use disk space.

## 🤔 **Reflective Questions**

1. Why shouldn't you use `docker kill` as your primary way to stop a container?
2. What is the difference between `docker ps` and `docker ps -a`?
3. Can you restart a container that has been `rm`-ed?

## 📚 **Further Reading**

- **Docker Orientation:** [Container Lifecycle](https://docs.docker.com/engine/reference/commandline/container/)
- **Guide:** [Manage Containers](https://docs.docker.com/get-started/02_our_app/#stopping-the-container)

## 📝 **Mini Task (Production)**

1. Start an Nginx container in the background:
   ```bash
   docker run -d --name lifecycle-test nginx
   ```
2. Check its status using `docker ps`.
3. Stop it: `docker stop lifecycle-test`.
4. Run `docker ps`. Is it there?
5. Run `docker ps -a`. Is it there?
6. Restart it: `docker start lifecycle-test`.
7. Finally, stop and remove it:
   ```bash
   docker stop lifecycle-test
   docker rm lifecycle-test
   ```
8. Verify it's completely gone from `docker ps -a`.
