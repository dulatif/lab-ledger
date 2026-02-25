# 💡 DK05.1.2: Arsitektur Docker Swarm

**Outline:**

- **The Brain vs. The Brawn (SEEI):** Understanding the distinction between Manager and Worker nodes.
- **The Consensus (PPP):** How Managers make decisions using the Raft algorithm.
- **Your Mission (Production):** Designing a resilient management layer for your cluster.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Brain vs. The Brawn (SEEI)**

**The Two Roles in a Swarm:**

1. **Manager Nodes:** These are the "Decision Makers." They handle cluster state, scheduling, and API requests.
2. **Worker Nodes:** These are the "Executioners." They simply run the containers as instructed by the Managers.

**Can a Manager also be a Worker?**
Yes, by default, Manager nodes are also Workers. In small clusters, this is common. In large production clusters, we often "Drain" the managers so they only focus on orchestration.

### **Part 2: Practice - The Consensus (PPP)**

**The "High Availability" Rule:**
You should always have an **Odd Number** of Managers (usually 3 or 5). This is because Swarm uses the **Raft Consensus Algorithm** to ensure all managers agree on the cluster state.

**Why Odd?**

- If you have 3 managers, you can lose 1 and still have a "Quorum" (2/3) to make decisions.
- If you have 2 managers and lose 1, you only have 1/2 (50%). This is not a majority, so the cluster stops accepting changes to prevent "Split-Brain" errors.

## 🧠 **Real-World Case Study: "The Split-Brain Nightmare"**

- **Scenario:** A dev team set up a cluster with 2 Manager nodes.
- **The Issue:** The network cable between the two managers was severed.
- **The Confusion:** Manager 1 thought Manager 2 was dead, and Manager 2 thought Manager 1 was dead. Both tried to take full control and start conflicting versions of the database.
- **The Result:** Data corruption occurred because two "Bosses" were giving different orders to the workers.
- **The Fix:** They added a 3rd Manager node. Now, even if one is isolated, the other two can agree on who is the leader.
- **The Lesson:** Always follow the "Odd Number" rule for your orchestrator's brain.

## 🤔 **Reflective Questions**

1. If you have a cluster of 10 nodes, should all of them be Managers? (Answer: No, it's inefficient. Usually, 3 managers and 7 workers is plenty).
2. What happens to the "Workers" if all "Managers" go offline? (Answer: The containers keep running, but you can't change anything or recover from failures).
3. How do Managers communicate with Workers?

## 📚 **Further Reading**

- **Raft.github.io:** [How Raft works (Visual Guide)](https://raft.github.io/)
- **Docker Documentation:** [Administrator guide for Swarm](https://docs.docker.com/engine/swarm/admin_guide/)

## 📝 **Mini Task (Production)**

1. Draw a simple diagram of a 5-node cluster.
2. Label them as Manager (M) or Worker (W) following best practices (3 Managers, 2 Workers).
3. If one Manager node goes down, how many are left? Is it still a majority of the original 3?
4. If two Manager nodes go down, what happens to the cluster's ability to "Schedule new containers"?
5. Reflect: Why do we care more about Manager health than Worker health?
