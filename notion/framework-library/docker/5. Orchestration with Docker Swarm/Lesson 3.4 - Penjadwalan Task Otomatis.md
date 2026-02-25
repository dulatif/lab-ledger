# 💡 DK05.3.4: Penjadwalan Task Otomatis

**Outline:**

- **The Strategy (SEEI):** Understanding how Swarm decides which node "Wins" a new task.
- **The Constraints (PPP):** Influencing Swarm's decisions using Node Labels and Resource Limits.
- **Your Mission (Production):** Forcing a database service to run only on your "High Performance" nodes.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Strategy (SEEI)**

**The Lazy Manager**
Docker Swarm uses a "Spread" strategy by default. It looks for the node with the fewest tasks and places the new one there. This is a very simple way to achieve balance.

**The Decision Factors:**

- **Node Health:** Only Active and Reachable nodes get tasks.
- **Resources:** If a node is out of CPU/Memory (and limits are defined), Swarm will skip it.
- **Constraints:** Your specific rules for placement.

### **Part 2: Practice - The Constraints (PPP)**

**Node Labels:**
You can tag your servers with custom metadata. For example:

```bash
docker node update --label-add type=db Worker-01
```

**Using Constraints in a Service:**
_You can then tell a service to "Only run on nodes labeled type=db":_

```bash
docker service create \
  --name my-db \
  --constraint 'node.labels.type == db' \
  postgres
```

**Resource Reservations:**
_Tell Swarm: "Don't put this container on a node unless it has at least 1GB of RAM free."_

```bash
--reserve-memory 1Gb
```

## 🧠 **Real-World Case Study: "The CPU Hog"**

- **Scenario:** A video processing app was periodically consuming 100% of the CPU on whatever node it landed on.
- **The Issue:** It was landing on the Manager node, causing the cluster's "Brain" to freeze.
- **The Fix:** The team added a constraint to the Service: `--constraint 'node.role == worker'`.
- **The Result:** The "CPU Hog" was restricted to worker nodes only. The Manager remained responsive and could still manage the cluster during peak processing.
- **The Lesson:** Use constraints to protect your Managers from unstable or heavy workloads.

## 🤔 **Reflective Questions**

1. What happens if you set a constraint that matches NO nodes in your cluster? (Answer: The service will stay in `Pending` state forever).
2. Can you apply multiple constraints to a single service?
3. Why are resource limits (`--limit-cpu`) important in a multi-tenant cluster?

## 📚 **Further Reading**

- **Docker Documentation:** [Placement constraints](https://docs.docker.com/engine/reference/commandline/service_create/#constraint)
- **Guide:** [Reserve resources for a service](https://docs.docker.com/engine/swarm/services/#reserve-resources-for-a-service)

## 📝 **Mini Task (Production)**

1. Add a label to one of your nodes: `docker node update --label-add environment=prod [node-name]`.
2. Try to create a service with the constraint `node.labels.environment == stage`.
3. Run `docker service ps [service-name]`.
4. What is the status? (Hint: Look for `Pending`).
5. Now, update the service to change the constraint to `prod`.
6. Watch as Swarm immediately schedules the task.
7. Reflect: How does this help you manage "Dev" vs "Prod" workloads on a single cluster?
