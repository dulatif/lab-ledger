# 💡 DK05.2.4: Simulasi Klaster dengan Play with Docker

**Outline:**

- **The Sandbox (SEEI):** Introducing a free, browser-based tool to practice multi-node orchestration.
- **The Lab (PPP):** Setting up a 3-node cluster in under 60 seconds without installing anything.
- **Your Mission (Production):** Successfully connecting a "Virtual Armada" in the cloud.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Sandbox (SEEI)**

**Hardware is expensive**
Not everyone has 3 servers or 32GB of RAM to run virtual machines locally. This is where **Play with Docker (PWD)** comes in.

**What is PWD?**

- It's a free, playground environment built by the Docker community.
- It gives you 4 hours of session time.
- You can create up to 5 separate virtual "Instances" (Docker engines) that are all on the same private network.

**Why use it for Swarm?**
It is the perfect place to practice `swarm init` and `swarm join` because you can see multiple different terminals side-by-side in your browser.

### **Part 2: Practice - The Lab (PPP)**

**The "Speedrun" Setup:**

1. **Visit [labs.play-with-docker.com](https://labs.play-with-docker.com/)** and log in with your Docker Hub account.
2. **Click "ADD NEW INSTANCE"** three times. You now have `node1`, `node2`, and `node3`.
3. **On `node1` (Manager):**
   ```bash
   docker swarm init --advertise-addr <INTERNAL_IP>
   ```
   _(PWD usually provides the internal IP at the top of the terminal)._
4. **On `node2` and `node3` (Workers):**
   Paste the `docker swarm join` command from `node1`.
5. **Verify on `node1`:**
   ```bash
   docker node ls
   ```
   _You should see all 3 nodes listed!_

## 🧠 **Real-World Case Study: "The Practice Deployment"**

- **Scenario:** A dev team wanted to move to Swarm but was afraid of breaking production.
- **The Experiment:** Every Friday for a month, they spent 1 hour on PWD recreating their stack and intentionally "Killing" nodes to see how Swarm reacted.
- **The Result:** When they finally moved to production, they were confident because they had already "Failed" 20 times in a safe environment.
- **The Lesson:** Use playgrounds to build "Muscle Memory." Never run a command for the first time in production.

## 🤔 **Reflective Questions**

1. Why does PWD give you a 4-hour limit?
2. Can you run a real website on PWD for your customers? (Answer: No, it's for training only!).
3. What happens to your data when the PWD timer reaches 00:00?

## 📚 **Further Reading**

- **Play with Docker:** [Official Website](https://labs.play-with-docker.com/)
- **GitHub:** [PWD project source code](https://github.com/play-with-docker/play-with-docker)

## 📝 **Mini Task (Production)**

1. Open Play with Docker.
2. Create 3 instances.
3. Initialize the first as a manager.
4. Join the other two as workers.
5. Create a simple service: `docker service create --replicas 3 --name pinger alpine ping google.com`.
6. Run `docker service ps pinger`.
7. Notice how Swarm distributed the 3 "Pings" across your 3 virtual instances.
8. **Nuclear Option:** Delete `node2` (click the trash icon).
9. Wait 30 seconds. Run `docker service ps pinger` on `node1`.
10. Did Swarm move the missing ping to a healthy node?
