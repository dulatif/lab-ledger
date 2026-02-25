# 💡 DK02.3.3: Mengelola Volumes

**Outline:**

- **The Management (SEEI):** Mastering the `docker volume` CLI for administrative tasks.
- **The Operations (PPP):** Listing, inspecting, and safely deleting persistent data.
- **Your Mission (Production):** Performing "Housekeeping" on your persistent storage.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Management (SEEI)**

**Why Manage Volumes?**
Volumes represent the actual data of your application. If not managed correctly, they can lead to data leaks (unused volumes taking up space) or accidental data loss.

**The Source of Truth:**
Docker provides a unified interface to manage these volumes regardless of whether they are stored on a local SSD, a network drive (NFS), or cloud storage (AWS EBS).

### **Part 2: Practice - The Operations (PPP)**

**The Essential Toolbox:**

1. **List all Volumes:**
   ```bash
   docker volume ls
   ```
2. **Detailed Inspection:**
   _Find out when it was created and where it is physically located._
   ```bash
   docker volume inspect <volume-name>
   ```
3. **Remove a Volume:**
   ```bash
   docker volume rm <volume-name>
   ```
   _Note: You cannot remove a volume that is currently in use by a container._
4. **Remove Unused Volumes (The "Housekeeper"):**
   ```bash
   docker volume prune
   ```
   _This is a dangerous but useful command. It deletes every volume not associated with at least one container._

## 🧠 **Real-World Case Study: "The Orphaned Data"**

- **Scenario:** Every time a developer starts a database container for a quick test, Docker creates an "Anonymous Volume" (a volume with a random hexadecimal name).
- **The Problem:** The dev deletes the container, but forgetting that volumes are persistent, they don't delete the volume.
- **The Result:** Three months later, the host machine has 200 nameless volumes taking up 50GB of "Ghost Data."
- **The Solution:** They run `docker volume prune` once a week.
- **The Lesson:** Named volumes are for production; anonymous volumes are dangerous "orphans" if not pruned regularly.

## 🤔 **Reflective Questions**

1. What is the difference between an "Anonymous Volume" and a "Named Volume"?
2. Can you rename a Docker Volume? (Answer: No, you would have to create a new one and migrate the data).
3. If I run `docker rm -v <container>`, what happens to the attached volumes? (Answer: It removes the container AND any anonymous volumes attached to it).

## 📚 **Further Reading**

- **Docker CLI:** [docker volume reference](https://docs.docker.com/engine/reference/commandline/volume/)
- **Guide:** [Back up, restore, or migrate volumes](https://docs.docker.com/storage/volumes/#backup-restore-or-migrate-data-volumes)

## 📝 **Mini Task (Production)**

1. List your current volumes.
2. Create three new volumes: `test1`, `test2`, `test3`.
3. Run `docker volume ls` to verify.
4. Remove `test1` specifically: `docker volume rm test1`.
5. Run `docker volume prune` to remove the others (since they aren't used by any containers).
6. Confirm your volume list is clean again.
