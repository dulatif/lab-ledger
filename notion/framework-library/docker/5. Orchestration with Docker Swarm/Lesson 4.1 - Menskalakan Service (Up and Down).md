# 💡 DK05.4.1: Menskalakan Service (Up and Down)

**Outline:**

- **The Elasticity (SEEI):** Understanding how to adjust the number of replicas to match traffic demands.
- **The Swiftness (PPP):** Using the `scale` command to instantly increase or decrease capacity.
- **Your Mission (Production):** Handling a viral traffic spike without dropping a single user.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Elasticity (SEEI)**

**Why Scale?**
In a traditional setup, if your website gets too much traffic, you have to buy a new server, install everything, and configure a load balancer. This takes hours or days.

**The Orchestrated Advantage:**
With Swarm, scaling is a single command that takes seconds.

- **Scale Up:** Increase replicas when your marketing campaign starts.
- **Scale Down:** Decrease replicas at 2 AM when everyone is sleeping to save electricity/cloud costs.

**Where does the traffic go?**
As soon as you scale up, Swarm's internal load balancer automatically starts sending traffic to the new containers. You don't have to change any configuration.

### **Part 2: Practice - The Swiftness (PPP)**

**The Two Ways to Scale:**

1. **The Short Way:**
   ```bash
   docker service scale my-app=10
   ```
2. **The Explicit Way:**
   ```bash
   docker service update --replicas 10 my-app
   ```

**What Swarm does:**
If you scale from 2 to 10, Swarm will:

- Check which nodes have the most free RAM.
- Start 8 new containers across those nodes.
- Health-check the new containers.
- Start routing traffic to all 10.

**Scaling to Zero:**
Yes, you can run `docker service scale my-app=0`. The service definition stays in the cluster, but no containers are running. This is great for "hibernating" apps you only use occasionally.

## 🧠 **Real-World Case Study: "The Flash Sale"**

- **Scenario:** A clothing brand announced a "90% off for 1 hour" sale.
- **The Chaos:** Usually, their site gets 100 visitors/minute. During the sale, it jumped to 5,000 visitors/minute.
- **The Recovery:** The lead developer watched the CPU logs. They ran `docker service scale web=50` and `docker service scale worker=20`.
- **The Result:** Within 45 seconds, the new replicas were online. The site slowed down for a moment but never crashed. They sold $100k of clothes in that hour.
- **The Lesson:** Scalability is the difference between a "Big Win" and a "Public Relations Nightmare."

## 🤔 **Reflective Questions**

1. If you scale to 100 replicas but only have 1 server, what happens? (Answer: The server will likely run out of RAM and crash).
2. Is there a limit to how many replicas a service can have?
3. What is the difference between "Vertical Scaling" (bigger server) and "Horizontal Scaling" (more containers)?

## 📚 **Further Reading**

- **Docker CLI:** [docker service scale reference](https://docs.docker.com/engine/reference/commandline/service_scale/)
- **Guide:** [Horizontal vs Vertical scaling](https://www.docker.com/blog/scaling-docker-containers/)

## 📝 **Mini Task (Production)**

1. Create a simple service with 1 replica:
   ```bash
   docker service create --name scaler nginx
   ```
2. Scale it to 5: `docker service scale scaler=5`.
3. Watch the progress: `docker service ls`. You will see the counter move from `1/5` to `5/5`.
4. Now, scale it back down to 2.
5. Run `docker service ps scaler`.
6. Notice that the 3 "Extra" containers are now marked as `Shutdown`.
7. Reflect: How does this "History" help you debug if a scaling event caused problems?
