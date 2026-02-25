# 💡 DK05.1.1: Dari Kontainer Tunggal ke Armada

**Outline:**

- **The Limit (SEEI):** Understanding why a single Docker host is a "Single Point of Failure."
- **The Orchestrator (PPP):** Introducing the concept of a cluster and the need for coordination.
- **Your Mission (Production):** Visualizing your application scaling beyond one machine.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Limit (SEEI)**

**The "Lap" is not enough**
Up until now, we have been running containers on one machine (your laptop or a single server). This is great for development, but in production, it's dangerous.

**The Problems with Single-Host Docker:**

1. **Downtime:** If the server hardware fails, your entire application goes down.
2. **Scaling:** One server has a finite amount of CPU and RAM. What if you need more?
3. **Updating:** How do you update the host OS without stopping your app?

### **Part 2: Practice - The Orchestrator (PPP)**

**Enter the "Armada"**
To solve these problems, we group multiple servers together into a **Cluster**. But managing 10 servers manually is impossible. We need an **Orchestrator**.

**What an Orchestrator (like Swarm) does:**

- **Placement:** It decides which server has room to run your container.
- **Health:** If a server dies, it automatically restarts the containers on a healthy server.
- **Networking:** It creates a "Virtual Bridge" across all servers so containers can talk to each other regardless of where they are physically.

## 🧠 **Real-World Case Study: "The Power Outage"**

- **Scenario:** A small company ran their web shop on one powerful server in a local data center.
- **The Disaster:** A power surge fried the server's motherboard. The shop was offline for 2 days while they ordered a replacement.
- **The Solution:** They migrated to Docker Swarm with 3 cheaper servers.
- **The Result:** Six months later, one server lost power again. Docker Swarm detected the failure and moved the shop's containers to the other 2 servers within seconds.
- **The Victory:** The customers didn't even notice. The shop stayed open.
- **The Lesson:** Reliability comes from numbers, not from "Better Hardware."

## 🤔 **Reflective Questions**

1. If you have 5 servers in a cluster, how many can fail before your app goes down (assuming you have enough spare capacity)?
2. What is the difference between "Docker" and "Docker Swarm" in one sentence?
3. Why is orchestration considered a "Requirement" for modern production apps?

## 📚 **Further Reading**

- **Docker Documentation:** [Swarm mode overview](https://docs.docker.com/engine/swarm/)
- **Guide:** [Key concepts of Docker Swarm](https://docs.docker.com/engine/swarm/key-concepts/)

## 📝 **Mini Task (Production)**

1. Imagine you have 3 servers: Server A (4GB RAM), Server B (8GB RAM), Server C (4GB RAM).
2. You want to run a Database that requires 6GB of RAM.
3. Which server will an orchestrator likely choose?
4. If Server B fails, can the orchestrator move the Database to Server A or C? Why?
5. Reflect: How does this make infrastructure management "Invisible" to the developer?
