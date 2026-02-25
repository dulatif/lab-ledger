# 💡 DK05.4.2: Konsep Self-Healing dalam Swarm

**Outline:**

- **The Resilience (SEEI):** Understanding why "Crashing is okay" in a Swarm environment.
- **The Loop (PPP):** How Swarm continuously monitors and repairs your application.
- **Your Mission (Production):** Simulating an application crash and watching Swarm fix it.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Resilience (SEEI)**

**Modern Software is Messy**
Apps run out of memory (OOM), they have segmentation faults, or they freeze. In a manual setup, when an app crashes, you get paged at 3 AM to restart it.

**The "Self-Healing" Philosophy:**
Swarm assumes your application _will_ fail eventually. Instead of preventing every failure, it focuses on **Fast Recovery**.

**Key Features:**

1. **Health Monitoring:** Swarm checks if the container is still "Running."
2. **Auto-Replacement:** If a container stops unexpectedly, Swarm starts a new one immediately.
3. **Node Re-balancing:** If a whole server dies, Swarm moves the workload to a survivor.

### **Part 2: Practice - The Loop (PPP)**

**How to verify Self-Healing:**

1. **Start a service:** `docker service create --name healer --replicas 3 nginx`
2. **Find a container ID:** `docker ps` on one of the nodes.
3. **Kill the container manually:** `docker rm -f [container_id]`
4. **Watch the magic:** Run `docker service ps healer` immediately.

**What you will see:**

- One task will be marked as `Failed` or `Complete`.
- A **New Task** will be in the `Ready` or `Starting` state.
- Within seconds, you are back to 3/3 replicas.

**The "Infinite Loop" Warning:**
If your app crashes _every time_ it starts (e.g., a bad config), Swarm will keep trying to restart it. This is why resource limits and health checks (which we'll learn later) are important.

## 🧠 **Real-World Case Study: "The Silent Memory Leak"**

- **Scenario:** A PDF processing service had a tiny memory leak. Every 12 hours, it would consume all available RAM and crash.
- **The "Band-Aid":** Because the team used Docker Swarm, Swarm would simply notice the crash and start a fresh container.
- **The Result:** The app had 99.9% uptime for months before the developers even bothered to fix the leak. Users never noticed a thing.
- **The Lesson:** Self-healing buys you time to fix bugs without public downtime.

## 🤔 **Reflective Questions**

1. Does Swarm "Heal" a container if it is still running but responding with "500 Internal Server Error"? (Answer: Not by default. You need a **Healthcheck** for that!).
2. Where does Swarm put the "New" container if the original one crashed?
3. How many times will Swarm try to restart a failing service before giving up?

## 📚 **Further Reading**

- **Docker Orientation:** [Swarm mode self-healing](https://docs.docker.com/engine/swarm/how-swarm-mode-works/state-reconciliation/)
- **Guide:** [Control service restart behavior](https://docs.docker.com/engine/reference/commandline/service_create/#restart-policy)

## 📝 **Mini Task (Production)**

1. Create a "Suicidal" service:
   ```bash
   docker service create --name suicide alpine sh -c "sleep 10 && exit 1"
   ```
2. Run `docker service ps suicide` every 15 seconds.
3. Watch as the list grows with many "Failed" tasks.
4. Notice how Swarm manages the history of these failures.
5. Reflect: How do you stop this loop? (Answer: `docker service rm suicide`).
6. Predict: If you had 100 replicas instead of 1, would the cluster get overwhelmed by these restarts?
