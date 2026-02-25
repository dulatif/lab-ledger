# 💡 DK05.3.2: Membuat Service Pertama Anda

**Outline:**

- **The Deployment (SEEI):** Using `docker service create` to launch your first orchestrated application.
- **The Flags (PPP):** Understanding the essential parameters for replicas, names, and ports.
- **Your Mission (Production):** Deploying a high-availability ping service.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Deployment (SEEI)**

**From CLI to Cluster**
Creating a service is very similar to running a container, but the results are distributed across your entire set of workers.

**The Base Command:**

```bash
docker service create --name my-web -p 8080:80 nginx
```

**Wait, what's different?**
If you run this on a 3-node cluster, Nginx will start on **one** of the nodes. But the magic is in the **Port Routing**. Even if Nginx is running on `Node-1`, you can visit `Node-2:8080` or `Node-3:8080` and you will still see the Nginx welcome page. This is called the **Ingress Routing Mesh**.

### **Part 2: Practice - The Flags (PPP)**

**Building a Robust Service:**

1. **`--replicas`**: The most important flag.
   ```bash
   docker service create --name web --replicas 3 -p 80:80 nginx
   ```
2. **`--update-delay`**: How much time to wait between updating each container.
3. **`--constraint`**: Restrict where a service runs (e.g., "Only on servers with SSDs").
   ```bash
   --constraint 'node.labels.disk == ssd'
   ```

**The "Scale" check:**
After creating the service, run:

```bash
docker service ps my-web
```

_You will see the 3 replicas being "Distributed" across your swarm nodes._

## 🧠 **Real-World Case Study: "The Load Balanced App"**

- **Scenario:** An e-commerce site needs to handle 1,000 requests per second.
- **The Setup:** They deploy an Nginx service with `--replicas 5`.
- **The Magic:** They point their domain name to the IP of ANY node in the cluster.
- **The Result:** The Docker Swarm "Routing Mesh" receives the traffic and automatically distributes it across the 5 running Nginx containers.
- **The Lesson:** Swarm gives you a "Free" load balancer out of the box. You don't need a complex external hardware balancer for simple scaling.

## 🤔 **Reflective Questions**

1. If you create a service with 1 replica, and that node dies, what does Swarm do?
2. Why is it better to use `--name` for a service than letting Docker pick a random one?
3. What happens if you try to bind to a port that is already in use by another service?

## 📚 **Further Reading**

- **Docker CLI:** [docker service create reference](https://docs.docker.com/engine/reference/commandline/service_create/)
- **Guide:** [Service mode: replicated vs global](https://docs.docker.com/engine/swarm/how-swarm-mode-works/services-tasks-containers/#replicated-and-global-services)

## 📝 **Mini Task (Production)**

1. On your Swarm manager, create a service:
   ```bash
   docker service create --name helloworld alpine ping google.com
   ```
2. Verify it is running: `docker service ls`.
3. Check where it is running: `docker service ps helloworld`.
4. Now, look at the "REPLICAS" column in `docker service ls`. It should say `1/1`.
5. What does the "1/1" represent? (Hint: [Actual]/[Desired]).
6. Remove the service: `docker service rm helloworld`.
