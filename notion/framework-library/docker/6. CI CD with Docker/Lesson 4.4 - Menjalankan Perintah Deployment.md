# 💡 DK06.4.4: Menjalankan Perintah Deployment

**Outline:**

- **The Script (SEEI):** Understanding the specific Docker commands required to update a running service.
- **The Pruning (PPP):** Cleansing your server of old, dangling images and containers.
- **Your Mission (Production):** Writing a robust deployment script that handles "Rolling back" on the server level.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Script (SEEI)**

**"The Core Five"**
Regardless of how you connect to the server (SSH Action or manual terminal), these five steps constitute a "Pro" deployment:

1. **Pull:** Get the new image layers.
2. **Setup:** Ensure environment variables are current.
3. **Update:** Replace the old containers with new ones.
4. **Health Check:** Verify the new containers are actually responding.
5. **Prune:** Clean up the disk space.

**Why is Pull separate from Update?**
If you just run `up`, Docker will try to pull the image _while_ the site is potentially shutting down. By running `docker pull` first, you ensure the image is 100% on the server's disk before you trigger the restart. This minimizes downtime.

### **Part 2: Practice - The Pruning (PPP)**

**The "Prune" Protocol:**
Every time you deploy a new image, the "Old" image stays on the server's disk as an "Untagged" or "Dangling" image. If you deploy 5 times a day, your server will run out of disk space within a month.

**The Cleanup Command:**

```bash
docker image prune -af --filter "until=24h"
```

- `-a`: Remove all unused images, not just dangling ones.
- `-f`: Force (don't ask for permission).
- `--filter "until=24h"`: **Crucial!** Keep images from the last 24 hours so you can roll back quickly if needed.

## 🧠 **Real-World Case Study: "The Disk Space Apocalypse"**

- **Scenario:** A dev team had a CI/CD pipeline that worked perfectly for 6 months. Then, one Tuesday, the deployments started failing with the error `No space left on device`.
- **The Issue:** The production server was 100% full. The database crashed because it couldn't write logs.
- **The Discovery:** There were 500GB of "Dangling" images from 6 months of automated deployments.
- **The Fix:** They added `docker system prune -f` to the end of their deployment script.
- **The Outcome:** The server's disk usage dropped from 100% to 15%. They never had a "No Space" error again.
- **The Lesson:** Automation must include **Cleanup**. A pipeline that doesn't clean is just a slow-motion disaster.

## 🤔 **Reflective Questions**

1. Why shouldn't you run `docker system prune` _before_ the new image is pulled?
2. What is the difference between `docker-compose down` and `docker-compose stop`? (Answer: `down` removes networks and containers; `stop` just pauses them).
3. If you use `--filter "until=24h"`, are you 100% safe to roll back if a bug appears 2 days later?

## 📚 **Further Reading**

- **Docker:** [Prune unused Docker objects](https://docs.docker.com/config/pruning/)
- **Guide:** [Deployment best practices with Compose](https://docs.docker.com/compose/production/)

## 📝 **Mini Task (Production)**

1. Open your terminal.
2. Run `docker image ls`. Count how many images you have.
3. Now, run `docker image prune -f`.
4. Run `docker image ls` again. Did the list get smaller?
5. Look at the output of the prune command: "Total reclaimed space." How many MBs or GBs did you save?
6. Reflect: If you were paying $0.10 per GB of storage in the cloud, how much did that command just save you?
