# 💡 DK02.3.2: Pengantar Docker Volumes

**Outline:**

- **The Solution (SEEI):** Introducing Docker Volumes as the standard for persistent data.
- **The Advantage (PPP):** Why Volumes are superior to storing data inside the container.
- **Your Mission (Production):** Understanding where Docker keeps your "Gold" (data).

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Solution (SEEI)**

**What is a Docker Volume?**
A Volume is a dedicated directory on the host machine's file system that is managed by Docker (outside of the container's lifecycle).

**The Storage Analogy**

- **Writable Layer:** Like writing notes on a whiteboard. When you clean the board (remove the container), the notes are gone.
- **Docker Volume:** Like writing on a piece of paper and putting it in a filing cabinet. Even if you erase the whiteboard, the paper in the cabinet remains safe.

**Key Characteristics:**

- **Persistent:** Volumes survive even when containers are deleted.
- **Managed:** Docker handles the location, permissions, and drivers.
- **Shared:** Multiple containers can access the same volume simultaneously.

### **Part 2: Practice - The Advantage (PPP)**

**Why use Volumes?**

1. **Decoupling:** Data is stored separately from the container's logic.
2. **Performance:** Volumes have better I/O performance than the writable layer.
3. **Backup/Migration:** It's much easier to back up a volume directory than a container.
4. **Native Management:** You use Docker CLI commands to create, list, and delete them.

**The Volume Location (Linux):**
By default, Docker stores volumes in `/var/lib/docker/volumes/` on the host machine. On Windows/macOS, they are stored inside the Docker Desktop VM.

## 🧠 **Real-World Case Study: "The Shared Config"**

- **Scenario:** You have 3 separate web server containers that all need to use the exact same SSL certificate and configuration file.
- **The Old Way:** Copying the files into each image (duplicates data and makes updates a nightmare).
- **The Solution:** Create a single Volume named `web-config`, put the files there, and mount it to all 3 containers.
- **The Result:** When you update the certificate in the volume, all 3 containers see the change immediately.
- **The Lesson:** Volumes are the best way to share data between containers.

## 🤔 **Reflective Questions**

1. If you delete all containers, do your volumes disappear? (Answer: No, they are safe!).
2. Who is responsible for managing the files inside a Docker Volume?
3. What is the main difference between a Volume and the container's Writable Layer?

## 📚 **Further Reading**

- **Docker Guide:** [Volumes](https://docs.docker.com/storage/volumes/)
- **Reference:** [Use volumes](https://docs.docker.com/storage/volumes/#use-a-volume-with-docker-compose)

## 📝 **Mini Task (Production)**

1. List the volumes on your machine:
   ```bash
   docker volume ls
   ```
2. Create your first named volume:
   ```bash
   docker volume create my-first-vol
   ```
3. Inspect it:
   ```bash
   docker volume inspect my-first-vol
   ```
4. Find the "Mountpoint" in the JSON output. This is the physical location on the host machine where the data is kept.
5. Why is it better to give a volume a name (like `db-data`) instead of letting Docker create a randomID?
