# 💡 DK02.4.1: Volumes vs Bind Mounts

**Outline:**

- **The Comparison (SEEI):** Understanding the two primary ways to persist data in Docker.
- **The Differences (PPP):** Comparing Managed Volumes with Host Bind Mounts.
- **Your Mission (Production):** Choosing the right storage type for your specific scenario.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Comparison (SEEI)**

Docker provide two main "Mount Types" to handle persistent data:

1. **Volumes:** Stored in a part of the host filesystem which is _managed by Docker_ (`/var/lib/docker/volumes/` on Linux). Non-Docker processes should not modify this storage.
2. **Bind Mounts:** Maps a _specific folder on your host_ (e.g., `C:\Projects\my-app`) directly into the container. This folder can be modified by any process on your host (like your code editor).

### **Part 2: Practice - The Differences (PPP)**

**Which one to use?**

| Feature             | Docker Volumes            | Bind Mounts                 |
| :------------------ | :------------------------ | :-------------------------- |
| **Location**        | Managed by Docker         | Anywhere on the host        |
| **I/O Performance** | Higher                    | Depends on host OS          |
| **Ease of Use**     | Command-line managed      | Managed via paths           |
| **Sharing**         | Safe between containers   | Potential permission issues |
| **Best For...**     | Database data, Production | Development (Live reload)   |

**Key Syntax Difference:**

- **Volume:** `docker run -v my-vol:/app ...` (Source is a **name**).
- **Bind Mount:** `docker run -v /home/user/code:/app ...` (Source is an **absolute path**).

## 🧠 **Real-World Case Study: "The Database vs. the Code"**

- **Scenario:** You are building a Node.js app that uses MongoDB.
- **The Strategy:**
  - For **MongoDB data**, use a **Volume** (`-v mongodb-data:/data/db`). This ensures maximum performance and isolation.
  - For **Node.js code**, use a **Bind Mount** (`-v $(pwd):/app`). This allows you to edit code in VS Code and have the container see the changes instantly.
- **The Result:** The best of both worlds. Safe, fast database storage and a highly efficient development workflow.

## 🤔 **Reflective Questions**

1. Why is a "Bind Mount" less safe than a "Volume"? (Hint: it gives the container access to any file on your host).
2. Can a Bind Mount reference a folder that doesn't exist yet on the host? (Answer: Yes, Docker will create an empty folder for you).
3. Which mount type is completely independent of the directory structure of the host machine?

## 📚 **Further Reading**

- **Docker Orientation:** [Manage data - Mount types](https://docs.docker.com/storage/#choose-the-right-mount-type)
- **Deep Dive:** [Bind mounts reference](https://docs.docker.com/storage/bind-mounts/)

## 📝 **Mini Task (Production)**

1. Identify a folder on your computer (e.g., your Desktop).
2. Run a container that mounts that folder as `/host-files`:
   ```bash
   # Use $(pwd) for the current directory
   docker run -it -v $(pwd):/host-files alpine sh
   ```
3. Inside the container, run `touch /host-files/docker-was-here.txt`.
4. Exit the container.
5. Check your Desktop/folder. Did the file appear?
6. Describe why this behavior is perfect for development but potentially dangerous for production servers.
