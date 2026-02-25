# 💡 DK05.4.3: Menangani Kegagalan Node

**Outline:**

- **The Catastrophe (SEEI):** Understanding what happens when a physical server in your cluster disappears.
- **The Evacuation (PPP):** How Swarm identifies a "Down" node and evacuates its tasks.
- **Your Mission (Production):** Simulating a total server outage and verifying service continuity.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Catastrophe (SEEI)**

**When Servers Die**
A container crashing is a common event. A whole server motherboard failing is a **Catastrophe**. Without orchestration, all apps on that server are dead until someone manually replaces the hardware.

**The Swarm Detection System:**
Managers and Workers send constant "Heartbeats" to each other.

1. If a Worker stops sending heartbeats for a few minutes, the Manager marks it as `Down`.
2. The Manager then checks: "Which services were running on that node?"
3. It declares those tasks as `Orphaned`.

### **Part 2: Practice - The Evacuation (PPP)**

**The Rescheduling Process:**
Once a node is `Down`, Swarm doesn't just wait. It starts the **Reconciliation**.

1. **Scan Survivors:** Swarm finds healthy nodes with spare CPU/RAM.
2. **Re-deploy:** It starts new tasks on those healthy nodes to reach the Desired State.
3. **Status Update:** The `docker service ps` history will show that the old tasks were on a dead node and new ones are on a live node.

**The "Quorum" Reminder:**
If you lose a **Manager** node, the cluster is fine as long as you still have a majority of managers left. If you lose too many managers, the cluster becomes "Frozen" (Read-only).

## 🧠 **Real-World Case Study: "The Data Center Flood"**

- **Scenario:** A burst pipe in a data center caused 3 out of 10 servers to short-circuit.
- **The Chaos:** Thirty percent of the company's infrastructure was gone in an instant.
- **The Swarm Response:** Because they used Docker Swarm, the remaining 7 servers automatically detected the loss. Within 2 minutes, every single web and API container that was on the flooded servers had been re-created on the 7 surviving dry servers.
- **The Outcome:** The website slowed down due to less total RAM, but it **never went offline**.
- **The Lesson:** Build your cluster so it can handle the loss of at least 1/3 of its capacity without total failure.

## 🤔 **Reflective Questions**

1. What is the default heartbeat interval in Swarm? (Answer: Usually around 5 seconds).
2. If the "Down" node comes back online later, does Swarm automatically move the tasks back to it? (Answer: No, Swarm doesn't like moving running containers unless it has to).
3. How do you manually force Swarm to re-balance the cluster after a node returns? (Answer: `docker service update --force`).

## 📚 **Further Reading**

- **Docker Documentation:** [How Swarm mode works - Raft](https://docs.docker.com/engine/swarm/raft/)
- **Guide:** [Monitoring node health](https://docs.docker.com/engine/swarm/manage-nodes/#monitor-node-health)

## 📝 **Mini Task (Production)**

1. Open Play with Docker (PWD).
2. Create a 3-node cluster.
3. Run a service with 6 replicas: `docker service create --name web --replicas 6 nginx`.
4. Run `docker service ps web` to see where they land (should be ~2 per node).
5. **Simulate a crash:** Delete `node2` using the PWD interface.
6. Run `docker service ps web` on `node1` every 10 seconds.
7. Watch as the 2 tasks that were on `node2` are marked as `Shutdown` and reappear on `node1` or `node3`.
8. Reflect: How much "Work" did you have to do to fix this 33% infrastructure failure?
