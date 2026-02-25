# 💡 DK05.1.3: Konsep Desired State

**Outline:**

- **The Declaration (SEEI):** Understanding that we tell Swarm _what_ we want, not _how_ to do it.
- **The Reconciliation (PPP):** How the "Control Loop" works to fix discrepancies between Actual and Desired states.
- **Your Mission (Production):** Defining a target state and watching Swarm enforce it.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Declaration (SEEI)**

**"I want 5 copies of Nginx"**
In standard Docker, you say: "Run this container." If that container crashes, Docker doesn't automatically restart it unless you have a specific restart policy.

In Swarm, you define a **Desired State**. You say: "I want 5 replicas of the 'Web' service to be running in this cluster at all times."

**The Shift from Imperative to Declarative:**

- **Imperative (Manual):** "Please start a container now."
- **Declarative (Swarm):** "This is what my system should look like. Make it so."

### **Part 2: Practice - The Reconciliation (PPP)**

**The Control Loop**
Docker Swarm constantly monitors the cluster. It compares the **Actual State** (what is currently running) with your **Desired State**.

1. **Monitor:** Swarm checks all nodes to see how many replicas of 'Web' are alive.
2. **Compare:** It finds only 4 replicas running (Actual) but you asked for 5 (Desired).
3. **Act:** Swarm immediately schedules a new replica on a healthy node to reach 5.

**Self-Healing:** This process happens automatically without any human intervention. This is why Swarm is considered "Self-Healing."

## 🧠 **Real-World Case Study: "The Midnight Chaos"**

- **Scenario:** A hardware failure caused an entire server cabinet to lose power at 3 AM.
- **The Automatic Response:** Ten worker nodes went offline instantly.
- **The Swarm Action:** The Manager nodes detected that 50 application replicas were "Missing" from the cluster. They immediately started re-deploying those 50 replicas onto the remaining healthy nodes.
- **The Result:** When the sysadmin arrived at work at 9 AM, they saw the hardware failure, but the application was running perfectly with the full number of replicas.
- **The Lesson:** Desired State is your "Digital Guardian." It never sleeps.

## 🤔 **Reflective Questions**

1. What happens if you manually delete a container that belongs to a Swarm service? (Answer: Swarm will restart it immediately).
2. Is "Desired State" something you set once, or can you update it anytime?
3. How does Swarm know if a container has "Failed" vs. just taking a long time to respond?

## 📚 **Further Reading**

- **Docker Orientation:** [How Swarm mode works](https://docs.docker.com/engine/swarm/how-swarm-mode-works/)
- **Guide:** [Services, tasks, and containers](https://docs.docker.com/engine/swarm/how-swarm-mode-works/services-tasks-containers/)

## 📝 **Mini Task (Production)**

1. Imagine you have a Desired State of `replicas: 10`.
2. Suddenly, you have a massive marketing campaign and want to double your capacity.
3. How would you "Change" your Desired State? (Hint: `docker service scale...`).
4. Reflect: Why is this better than manually running 10 more `docker run` commands?
5. Predict: What happens to the "Extra" 10 containers if you scale back down to 5 later?
