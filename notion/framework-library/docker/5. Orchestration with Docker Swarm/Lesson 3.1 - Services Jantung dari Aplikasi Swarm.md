# 💡 DK05.3.1: Services Jantung dari Aplikasi Swarm

**Outline:**

- **The Concept (SEEI):** Understanding why "Services" replace "Containers" as the fundamental unit of work in Swarm.
- **The Hierarchy (PPP):** Mapping the relationship between Services, Tasks, and Containers.
- **Your Mission (Production):** Visualizing the logical abstraction of your application.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Concept (SEEI)**

**Goodbye `docker run`**
When you switch to Swarm mode, you stop thinking about "Running a container" and start thinking about "Deploying a Service."

**What is a Service?**
A Service is the **Definition** of a desired state. It tells Swarm:

- What image to use.
- What command to run.
- How many replicas (copies) should exist.
- What ports to expose.

**Why the change?**
A single container is fragile. A Service is an abstraction that allows Swarm to manage uptime, load balancing, and scaling without you worrying about individual container IDs.

### **Part 2: Practice - The Hierarchy (PPP)**

**The "Russian Doll" of Swarm:**

1. **Service:** The high-level abstraction (e.g., "Web App").
2. **Task:** The "Slot" where a container runs. If you have 3 replicas, Swarm creates 3 Tasks.
3. **Container:** The actual Linux process running inside the Task.

**The Workflow:**

- You create a **Service**.
- The Manager creates **Tasks**.
- The Manager assigns Tasks to **Nodes**.
- The Nodes start the **Containers**.

If a container dies, the **Task** is marked as failed, and the Manager creates a **New Task** to replace it.

## 🧠 **Real-World Case Study: "The Ghost in the Machine"**

- **Scenario:** A developer tried to "Fix" a bug by running `docker stop [container_id]` on a container that was part of a Swarm service.
- **The Surprise:** Two seconds later, the container was running again with a new ID.
- **The Confusion:** "I stopped it! Why won't it stay stopped?"
- **The Lesson:** In Swarm, the Service is the "Boss." If the Service says "I need 1 replica," and you kill that replica, Swarm sees a discrepancy and fixes it. To stop it, you must remove the **Service**, not the container.

## 🤔 **Reflective Questions**

1. Can a single Service have containers running on 5 different physical servers?
2. What is the difference between a `docker-compose` service and a `docker swarm` service?
3. If you change the image of a Service, does Swarm restart all containers at once? (Answer: No, it does a rolling update!).

## 📚 **Further Reading**

- **Docker Orientation:** [How services work](https://docs.docker.com/engine/swarm/how-swarm-mode-works/services-tasks-containers/)
- **Guide:** [Deploy services to a swarm](https://docs.docker.com/engine/swarm/services/)

## 📝 **Mini Task (Production)**

1. Open your terminal.
2. If you are in Swarm mode, run `docker service ls`. Why is it empty?
3. Research the `docker service create` command.
4. List 3 flags that look similar to the `docker run` command (e.g., `-p`, `-e`, `--name`).
5. Reflect: If you have a Service named `api` with 3 replicas, how many **Tasks** will you see in `docker service ps api`?
