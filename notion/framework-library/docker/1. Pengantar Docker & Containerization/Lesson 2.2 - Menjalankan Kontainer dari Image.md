# 💡 DK02.2: Menjalankan Kontainer dari Image

**Outline:**

- **The Command (SEEI):** Mastering `docker run` and its most important flags.
- **The Interaction (PPP):** Modes of running (Foreground, Background, Interactive).
- **Your Mission (Production):** Launching various types of containers.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Command (SEEI)**

**The Swiss Army Knife: `docker run`**
`docker run` is the command most used in Docker. It transitions an Image from a static file to a running process.

**Anatomy of the Command:**
`docker run [OPTIONS] IMAGE [COMMAND] [ARG...]`

**Crucial Flags (Options):**

- **`-d` (Detached):** Runs the container in the background (perfect for servers/databases).
- **`-it` (Interactive + TTY):** Keeps the container open and allows you to type commands inside it (perfect for shells).
- **`--name`:** Gives your container a readable name (otherwise Docker picks a random one like "flamboyant_hopper").
- **`-p` (Publish):** Maps a port from your machine to the container (e.g., `-p 8080:80`).
- **`--rm`:** Automatically deletes the container when it stops (keeps your system clean).

### **Part 2: Practice - The Interaction (PPP)**

**The 3 Main Ways to Run:**

1. **One-Shot (Short-lived):**
   ```bash
   docker run alpine echo "Hello"
   ```
2. **Interactive Shell (Manual work):**
   ```bash
   docker run -it alpine /bin/sh
   ```
3. **Background Service (Long-running):**
   ```bash
   docker run -d --name my-web nginx
   ```

## 🧠 **Real-World Case Study: "Port Mapping"**

- **The Problem:** A web server (Nginx) inside a container is listening on port 80. But your browser can't see "inside" the container.
- **The Solution:** `docker run -p 8080:80 nginx`.
- **The Result:** You go to `localhost:8080` on your machine. Docker "routes" that traffic to port 80 _inside_ the container.
- **The Principle:** Containers are isolated by default; you must explicitly open "ports" to talk to them.

## 🤔 **Reflective Questions**

1. What is the difference between an Image and a Container (revisited)?
2. When should you use the `-d` flag?
3. Why is `--rm` helpful during development?

## 📚 **Further Reading**

- **Docker CLI:** [docker run official documentation](https://docs.docker.com/engine/reference/commandline/run/)
- **Guide:** [Containerize an application](https://docs.docker.com/get-started/02_our_app/)

## 📝 **Mini Task (Production)**

1. Run a container named `my-shell` using the `ubuntu` image in interactive mode.
   ```bash
   docker run -it --name my-shell ubuntu /bin/bash
   ```
2. Inside the container, type `ls` and `whoami`.
3. Type `exit` to leave.
4. Now, run an Nginx web server in the background and map it to port 9000.
   ```bash
   docker run -d -p 9000:80 --name my-web nginx
   ```
5. Open your browser and go to `localhost:9000`. Do you see the Nginx welcome page?
