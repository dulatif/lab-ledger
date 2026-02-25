# 💡 DK02.3.4: Me-mount Volume ke Kontainer

**Outline:**

- **The Connection (SEEI):** Using `-v` and `--mount` to bridge the gap between container and volume.
- **The Syntax (PPP):** Mastering the mapping: `[Volume Name]:[Path inside Container]`.
- **Your Mission (Production):** Persisting a database's data across restarts.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Connection (SEEI)**

**Plugging in the Storage**
Creating a volume is only half the job. You must "mount" it into a container at a specific path. From the application's perspective inside the container, it's just a regular folder. Behind the scenes, Docker is redirecting all writes to that host-managed volume.

**Two Ways to Mount:**

1. **The `-v` flag (Traditional):** Short and concise.
   - `docker run -v my-data:/app/data ...`
2. **The `--mount` flag (Modern/Recommended):** Verbose but safer and clearer.
   - `docker run --mount source=my-data,target=/app/data ...`

### **Part 2: Practice - The Syntax (PPP)**

**The Key Rule: `Source:Target`**

- **Source:** The name of the volume on your host.
- **Target:** The folder _inside_ the container where you want the data to appear.

**Example: Persisting MongoDB**
MongoDB stores its data in `/data/db`. To make it persistent:

```bash
docker run -d \
  --name my-mongo \
  -v mongo-data:/data/db \
  mongo
```

**Key Benefit:** You can stop `my-mongo`, delete it, and then start a new container with the same `-v mongo-data:/data/db`. **The database will still have all its data!**

## 🧠 **Real-World Case Study: "The Database Upgrade"**

- **Scenario:** You are running Postgres v14 and want to upgrade to v15.
- **The Workflow:**
  1. Stop and remove the Postgres v14 container. (Data is safe in the volume `pg-data`).
  2. Pull the Postgres v15 image.
  3. Start a new container: `docker run ... -v pg-data:/var/lib/postgresql/data postgres:15`.
- **The Result:** The new v15 database immediately "sees" the data from the v14 volume and performs the upgrade.
- **The Lesson:** Volumes enable seamless hardware and software upgrades without data migration.

## 🤔 **Reflective Questions**

1. What happens if you mount a volume to a folder that already contains files in the image? (Answer: The files in the image are copied _to the volume_ if the volume is empty).
2. What is the difference between `-v` and `--mount`?
3. Can one volume be mounted as "Read-Only" to a container? (Answer: Yes, adding `:ro` to the `-v` flag).

## 📚 **Further Reading**

- **Docker Orientation:** [Use volumes (-v vs --mount)](https://docs.docker.com/storage/volumes/#choose-the--v-or---mount-flag)
- **Reference:** [Volumes in Compose](https://docs.docker.com/compose/compose-file/compose-file-v3/#volumes)

## 📝 **Mini Task (Production)**

1. Create a volume: `docker volume create persist-lab`.
2. Start a container and write a file into that volume:
   ```bash
   docker run --rm -v persist-lab:/data alpine sh -c "echo 'preserved' > /data/test.txt"
   ```
3. Notice that the `--rm` flag deleted the container.
4. Start a SECOND container using the same volume:
   ```bash
   docker run --rm -v persist-lab:/backup alpine cat /backup/test.txt
   ```
5. Did the data survive? Why? (Explain the path mapping from `/data` in the first run to `/backup` in the second).
