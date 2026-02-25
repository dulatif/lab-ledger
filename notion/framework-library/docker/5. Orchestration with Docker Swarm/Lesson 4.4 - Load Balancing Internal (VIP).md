# 💡 DK05.4.4: Load Balancing Internal (VIP)

**Outline:**

- **The Invisible Bridge (SEEI):** Understanding how containers find each other in a cluster using Virtual IPs.
- **The Routing Mesh (PPP):** How Docker handles traffic from the outside world and routes it to the right container.
- **Your Mission (Production):** Verifying that your load balancer is actually balancing the requests.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Invisible Bridge (SEEI)**

**The IP Problem**
In a cluster, containers are constantly moving. A database might be at `10.0.0.5` today and `10.0.0.12` tomorrow. You cannot hardcode these IPs.

**The Solution: Virtual IPs (VIP)**
When you create a service, Swarm gives it a **Single Virtual IP**.

- Your App talks to `http://db-service` (the name).
- Swarm resolves this name to the **VIP**.
- The VIP acts as a load balancer, sending the request to one of the healthy containers behind it.

### **Part 2: Practice - The Routing Mesh (PPP)**

**The "Ingress" Magic**
When you expose a port (e.g., `-p 80:80`), Docker enables the **Routing Mesh**.

- A user hits the IP of **ANY** node in the cluster on port 80.
- Even if that node isn't running the container, it "Forwards" the packet over the overlay network to a node that IS running it.
- **Result:** You don't need to know which node is running your app. Just hit any node.

**Internal vs External:**

- **External:** Users hitting a node's IP (Routing Mesh).
- **Internal:** One container talking to another via service name (VIP/Internal Load Balancer).

## 🧠 **Real-World Case Study: "The Round-Robin Success"**

- **Scenario:** A developer wants to prove that their 3 Nginx replicas are actually sharing the work.
- **The Test:** They created a simple page that prints the **Hostname** of the container.
- **The Observation:** They hit the website and refreshed 10 times. They saw 3 different hostnames appearing in a sequence.
- **The Conclusion:** Swarm was using **Round-Robin** load balancing. It was perfectly distributing the traffic without any external configuration.
- **The Lesson:** Swarm doesn't just manage "Life and Death"; it manages "Traffic" too.

## 🤔 **Reflective Questions**

1. What is the difference between a `VIP` and a `DNS RR` (Round-Robin) in Swarm? (Answer: VIP is one IP that balances; DNS RR gives a list of all IPs).
2. Does the Routing Mesh slow down my application? (Answer: Negligibly, for most apps).
3. If I have 10 nodes, do I need to put a Load Balancer (like Cloudflare or AWS ALB) in front of the cluster? (Answer: Yes, to distribute traffic _to_ the nodes initially).

## 📚 **Further Reading**

- **Docker Documentation:** [Use swarm mode routing mesh](https://docs.docker.com/engine/swarm/ingress/)
- **Guide:** [Internal load balancing](https://docs.docker.com/engine/swarm/networking/#internal-load-balancing)

## 📝 **Mini Task (Production)**

1. Initialize a swarm.
2. Create a service: `docker service create --name whoami --replicas 3 -p 8000:8000 jwilder/whoami`.
3. Use `curl localhost:8000` (or visit in browser) several times.
4. Look at the output. Does the "I'm [hostname]" change?
5. Now, go to a different terminal/machine on your network and `curl` the IP of your Docker host on port 8000.
6. Does it still work?
7. Reflect: How does this "Ingress" behavior simplify your DNS setup?
