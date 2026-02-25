# 💡 DK03.1.2: Konsep Inti Services

**Outline:**

- **The Unit (SEEI):** Understanding the "Service" as the building block of a Compose stack.
- **The Relationship (PPP):** How services interact with Images, Networks, and Volumes.
- **Your Mission (Production):** Defining the roles in your application hierarchy.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Unit (SEEI)**

**In Compose, we talk about "Services"**
A Service is essentially a "template" for a container. It defines which image to use, which ports to open, and how much memory to use.

**Service vs. Container:**

- A **Service** is the definition (the "what").
- A **Container** is the running instance (the "how").
- A single service (like `api`) can be scaled to run multiple container instances if needed.

**Why encapsulate into services?**

- It allows you to group related configurations.
- It simplifies network communication (one service talks to another service name).

### **Part 2: Practice - The Relationship (PPP)**

**The "Stack" Hierarchy:**
A `docker-compose.yml` file has three top-level sections that mirror the Three Pilar of Docker:

1. **`services`**: Where you define your apps (Web, API, DB). This is the only mandatory section.
2. **`networks`**: Where you define the private virtual wires connecting them.
3. **`volumes`**: Where you define the persistent storage.

**The Lifecycle:**
When you run `docker compose up`:

1. Docker creates the **Networks**.
2. Docker creates the **Volumes**.
3. Docker starts the **Services** (Containers) in the order specified.

## 🧠 **Real-World Case Study: "Scaling the Web Service"**

- **Scenario:** Your web application is getting too much traffic for one container to handle.
- **The Docker Compose Power:** You don't need to manually run 5 more containers. You simply run:
  ```bash
  docker compose up --scale web=5 -d
  ```
- **The Result:** Compose launches 5 identical containers under the `web` service.
- **The Magic:** If you have a Load Balancer (like Nginx) in the same network, it can now route traffic to any of those 5 instances.

## 🤔 **Reflective Questions**

1. Can a `docker-compose.yml` file exist without a `services` section?
2. What is the benefit of defining a database as a "Service" instead of just running it separately?
3. How does the concept of a "Service" help when you need to switch from a local image to a production image?

## 📚 **Further Reading**

- **Docker Orientation:** [Compose File Reference - Services](https://docs.docker.com/compose/compose-file/05-services/)
- **Guide:** [Networking in Compose](https://docs.docker.com/compose/networking/)

## 📝 **Mini Task (Production)**

1. Think of a simple social media app.
2. List at least 4 "Services" that this app would likely need (e.g., Frontend, API, Database, Cache).
3. For each service, decide if it needs a **Volume** (is it stateful?) and if it needs to **Expose a Port** to the internet.
   - Frontend: Needs Port? Needs Volume?
   - Database: Needs Port? Needs Volume?
   - etc.
