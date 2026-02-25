# 💡 DK02.3.1: Masalah Penyimpanan Kontainer

**Outline:**

- **The Problem (SEEI):** Understanding the ephemeral (temporary) nature of container storage.
- **The Data Loss (PPP):** Witnessing how data vanishes when a container is deleted.
- **Your Mission (Production):** Proving that a container's writable layer is not for permanent data.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Problem (SEEI)**

**Containers are Ephemeral**
One of the most important concepts in Docker is that containers are intended to be disposable. You should be able to stop, delete, and recreate a container without fear.

**The Writable Layer**
When a container starts, Docker adds a thin "Writable Layer" on top of the read-only image layers.

- All files created or modified while the container is running are stored in this layer.
- **The Catch:** When the container is deleted (`docker rm`), this writable layer is **destroyed** permanently.

**The Negative Impact:**

- **Lost Data:** If your database stores entries in the writable layer, they are gone forever if the container crashes or is updated.
- **Performance:** Writing to the writable layer is slower than writing to a proper volume or the host's file system.

### **Part 2: Practice - The Data Loss (PPP)**

**The "Vanishing File" Experiment:**

1. **Create:** Start a container and create a file inside it.
   ```bash
   docker run --name temp-storage alpine sh -c "echo 'secret data' > /myfile.txt"
   ```
2. **Verify:** Check that the file exists.
   ```bash
   docker exec temp-storage cat /myfile.txt
   ```
3. **Delete:** Remove the container.
   ```bash
   docker rm -f temp-storage
   ```
4. **Conclusion:** Start a _new_ container from the same image.
   ```bash
   docker run --name new-temp alpine cat /myfile.txt
   ```
   _Note: You will get an error: "No such file or directory." The data is gone._

## 🧠 **Real-World Case Study: "The Disposable Database"**

- **Scenario:** A developer deploys a MySQL container to production but forgets to set up persistent storage.
- **The Event:** Six months later, they need to update the MySQL version. They delete the old container and start the new version.
- **The Disaster:** 100% of user data is gone. The new container started with an empty image.
- **The Lesson:** Never, ever store stateful data (databases, user uploads, logs) inside the container's writable layer. Use **Volumes** instead.

## 🤔 **Reflective Questions**

1. Why does Docker destroy the writable layer when a container is removed?
2. If I `stop` a container and then `start` it again, is my data still there? (Answer: Yes, but it's still at risk if the container is _removed_).
3. Where is the "Writable Layer" actually stored on the host computer?

## 📚 **Further Reading**

- **Docker Orientation:** [Manage data in Docker](https://docs.docker.com/storage/)
- **Deep Dive:** [The Writable Layer](https://docs.docker.com/storage/storagedriver/#the-writable-container-layer)

## 📝 **Mini Task (Production)**

1. Start an interactive alpine container.
2. Install a package (`apk add curl`).
3. Exit and remove the container.
4. Start a new one. Is `curl` still installed?
5. Explain in one sentence why installing software inside a running container (instead of a Dockerfile) is a bad idea.
