# 💡 DK06.5.3: Membersihkan Image Lama

**Outline:**

- **The Accumulation (SEEI):** Understanding why "Successful" builds lead to "Full" disks.
- **The Hygiene (PPP):** Implementing `docker image prune` and `docker system prune` as a final pipeline step.
- **Your Mission (Production):** Keeping your production disk usage below 20%.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Accumulation (SEEI)**

**"The Invisible Trash"**
Every time you run `docker-compose up -d --build` or `docker pull`, the old versions of your images don't disappear. They become **Dangling Images**.

- They have no name (usually labeled as `<none>`).
- They still consume GBs of space.
- They complicate auditing.

**The Impact on the Pipeline:**
If your CI/CD pipeline pushes updates to a server multiple times a day, your server will eventually hit "Disk Full." When this happens, your database will crash, and your logs will stop writing.

### **Part 2: Practice - The Hygiene (PPP)**

**The "Post-Deploy" Cleanup Step:**
Add this to your deployment script or your GitHub Action:

```bash
- name: 🧹 Cleanup Old Images
  uses: appleboy/ssh-action@master
  with:
    host: ${{ secrets.HOST }}
    username: ${{ secrets.USER }}
    key: ${{ secrets.KEY }}
    script: |
      docker image prune -af --filter "until=168h"
```

**Understanding the Filter:**
Using `--filter "until=168h"` (7 days) is a professional best practice.

1. It deletes everything older than a week.
2. It keeps the last few versions of your image.
3. If the new version is broken, you can still roll back instantly without waiting for a 1GB download.

## 🧠 **Real-World Case Study: "The Tuesday Morning Crash"**

- **Scenario:** A dev team noticed their server crashed every Tuesday morning at 10 AM.
- **The Investigation:** Monday was their "Batch Update" day. They pushed 50 small updates to their microservices.
- **The Discovery:** Each update left 1GB of dead image layers. By Tuesday morning, the 100GB disk was 100% full.
- **The Fix:** They added a `crontab` job that runs `docker system prune -f` every Sunday, and added a specific `image prune` after every CI/CD deployment.
- **The Result:** The Tuesday crashes stopped immediately. The disk usage stabilized at a constant 40GB.
- **The Lesson:** In the world of Docker, if you aren't cleaning, you're breaking.

## 🤔 **Reflective Questions**

1. What is the difference between `docker system prune` and `docker image prune`? (Answer: `system` deletes containers and networks, too; `image` only affects images).
2. Why is the `-f` (Force) flag necessary in automation?
3. How can you check your current disk usage on a Linux server? (`df -h`).

## 📚 **Further Reading**

- **Docker:** [Prune unused Docker objects](https://docs.docker.com/config/pruning/)
- **Guide:** [Automating Docker cleanup](https://linuxhandbook.com/prune-docker/)

## 📝 **Mini Task (Production)**

1. SSH into your test server (or local machine).
2. Run `docker images -f "dangling=true"`.
3. How many "ghost" images do you see?
4. Run `docker image prune`. Type 'y'.
5. Reflect: How much space did you just "Save" for free?
6. Add a "Cleanup" step to your master deployment GitHub Action YAML draft.
