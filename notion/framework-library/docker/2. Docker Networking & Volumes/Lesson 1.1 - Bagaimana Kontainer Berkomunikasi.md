# 💡 DK02.1.1: Bagaimana Kontainer Berkomunikasi?

**Outline:**

- **The Concept (SEEI):** Understanding the isolated nature of container networking and the need for a bridge.
- **The Mechanism (PPP):** How Docker handles IP address assignment and internal routing.
- **Your Mission (Production):** Visualizing the path a packet takes between two containers.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Concept (SEEI)**

**Network isolation**
By default, every Docker container is an island. It has its own network stack, loopback interface, and IP address. This isolation is a core security feature of containerization.

**The Bridge Analogy**
Imagine each container is a house on an island. To let them talk to each other (or the outside world), Docker builds a **Bridge**. This virtual bridge (usually named `docker0`) acts like a network switch, connecting all containers on the same host.

**The Negative Impact of No Networking**
Without a network strategy, you couldn't:

- Connect a Web App to a Database.
- Scale services across different nodes.
- Access your application from a browser.

### **Part 2: Practice - The Mechanism (PPP)**

**Docker's Internal DNS and IP Management:**

1. **Automatic IP:** Every time you start a container, it gets a private IP address within the bridge's subnet (e.g., `172.17.0.x`).
2. **Standard Bridge:** If you don't specify a network, Docker puts the container in the `bridge` network.
3. **Communication:** Containers can talk to each other via IP address, but this is fragile because IPs change when containers restart.

**Command to Explore:**

```bash
docker network ls
docker network inspect bridge
```

## 🧠 **Real-World Case Study: "The Micro-Service Handshake"**

- **Scenario:** A developer wants their "Order Service" to send data to the "Inventory Service."
- **The Old Way:** Hard-coding a specific IP address `172.17.0.5` in the code.
- **The Problem:** The Inventory Service restarts and gets IP `172.17.0.6`. The Order Service crashes.
- **The Solution:** Using **User-Defined Networks** (which we'll cover in Chapter 2) to enable **Service Discovery** by container name.
- **The Result:** Order Service just looks for `http://inventory` and Docker handles the translation automatically.

## 🤔 **Reflective Questions**

1. Why is relying on a container's IP address a "bad practice"?
2. What is the role of the `docker0` bridge on a Linux host?
3. If two containers are on different hosts, can they talk to each other via the default `bridge` network?

## 📚 **Further Reading**

- **Docker Docs:** [Networking overview](https://docs.docker.com/network/)
- **Deep Dive:** [Docker container networking](https://docs.docker.com/network/drivers/bridge/)

## 📝 **Mini Task (Production)**

1. List the networks on your machine: `docker network ls`.
2. Start two containers using the `alpine` image:
   ```bash
   docker run -d --name container1 alpine sleep 1000
   docker run -d --name container2 alpine sleep 1000
   ```
3. Find the IP of `container1`:
   ```bash
   docker inspect -f '{{ .NetworkSettings.IPAddress }}' container1
   ```
4. Try to "ping" `container1` from inside `container2`:
   ```bash
   docker exec container2 ping <IP_OF_CONTAINER1>
   ```
5. What happens if you try to `ping container1` by its name instead of its IP? (Hint: It will fail on the default bridge).
