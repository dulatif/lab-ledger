# 💡 DK05.3.3: Menginspeksi Services dan Tasks

**Outline:**

- **The Deep Dive (SEEI):** Using `inspect` and `ps` to look under the hood of a running service.
- **The Observation (PPP):** Understanding the logs and health status of distributed tasks.
- **Your Mission (Production):** Finding out which node is currently hosting a specific workload.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Deep Dive (SEEI)**

**"Where is my code running?"**
In a cluster of 50 nodes, you can't just run `docker ps` on every machine to find your container. You need a "Global View."

**The Tools:**

1. **`docker service ls`**: The high-level summary.
2. **`docker service ps <name>`**: The list of all "Tasks" (Replicas) and which node they are on.
3. **`docker service inspect <name>`**: The full JSON configuration of the service.

**Why use `ps`?**
It doesn't just show running containers; it shows the **History**. If a container died 10 minutes ago and was replaced, `docker service ps` will show the "Shutdown" task and the new "Running" task.

### **Part 2: Practice - The Observation (PPP)**

**Reading the `ps` Output:**

- **ID**: The unique identifier for the **Task**.
- **NAME**: Usually `service_name.1`, `service_name.2`, etc.
- **NODE**: The physical server name.
- **DESIRED STATE**: What Swarm wants (e.g., `Running`).
- **CURRENT STATE**: What is actually happening (e.g., `Running 5 minutes ago`).
- **ERROR**: If a task fails, the reason will be listed here (e.g., "Non-zero exit code").

**Viewing Logs:**
_To see the logs from all 5 replicas at once:_

```bash
docker service logs -f <service_name>
```

_Swarm will aggregate the logs from different servers into one stream._

## 🧠 **Real-World Case Study: "The Silent Failure"**

- **Scenario:** A dev team deployed a new version of their app. `docker service ls` showed `0/3` replicas running.
- **The Investigation:** They ran `docker service ps [app_name]`.
- **The Finding:** They saw 10 "Failed" tasks in the history. The ERROR column said `task: non-zero exit (1)`.
- **The Root Cause:** They used `docker service inspect` and realized they had a typo in the Database URL environment variable.
- **The Lesson:** When a service "Struggles" to start, `service ps` is the first place you look for clues.

## 🤔 **Reflective Questions**

1. If you have 3 replicas, will they always be on 3 different nodes? (Answer: Swarm tries to distribute them, but if you only have 2 nodes, it will put 2 on one and 1 on the other).
2. What is the difference between `docker ps` and `docker service ps`?
3. Can you inspect a service that has been deleted? (Answer: No).

## 📚 **Further Reading**

- **Docker CLI:** [docker service ps reference](https://docs.docker.com/engine/reference/commandline/service_ps/)
- **Guide:** [Monitoring a Swarm service](https://docs.docker.com/engine/swarm/services/#monitor-service-health)

## 📝 **Mini Task (Production)**

1. Create a service that purposely crashes:
   ```bash
   docker service create --name crasher alpine sh -c "sleep 5 && exit 1"
   ```
2. Wait 20 seconds.
3. Run `docker service ps crasher`.
4. How many tasks do you see in the list?
5. What is the status of the older tasks? (Hint: `Shutdown`).
6. Reflect: Why does Swarm keep trying to start it even though it keeps crashing? (Hint: Desired State).
7. Remove the service to save resources.
