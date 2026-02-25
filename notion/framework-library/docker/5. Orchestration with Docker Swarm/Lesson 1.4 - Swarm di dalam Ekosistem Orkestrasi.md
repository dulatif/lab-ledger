# 💡 DK05.1.4: Swarm di dalam Ekosistem Orkestrasi

**Outline:**

- **The Big Picture (SEEI):** Comparing Docker Swarm with other orchestrators like Kubernetes.
- **The Sweet Spot (PPP):** Identifying when to choose Swarm over more complex alternatives.
- **Your Mission (Production):** Deciding if Swarm is the right tool for your next project.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Big Picture (SEEI)**

**The Landscape**
Docker Swarm isn't the only orchestrator. The most famous one is **Kubernetes (K8s)**.

**Key Differences:**

- **Swarm:** "Batteries Included." Built into Docker. Extremely easy to learn and use. Uses the same YAML (mostly) as Docker Compose.
- **Kubernetes:** Extremely powerful and flexible. But it is very complex to install and manage. It requires learning a completely different set of concepts and YAML syntax.

### **Part 2: Practice - The Sweet Spot (PPP)**

**Why choose Swarm?**

1. **Developer Experience:** If you know Docker Compose, you already know 80% of Swarm.
2. **Speed:** You can set up a production-ready Swarm cluster in under 10 minutes.
3. **Single Host Migration:** Swarm is the perfect "Next Step" for moving from `docker-compose` on one server to multiple servers.
4. **Maintenance:** It requires much less "Ops" effort than a Kubernetes cluster.

**When to move to Kubernetes?**

- When you need complex networking policies.
- When you have thousands of nodes.
- When you need a massive ecosystem of plugins (Helm, Istio, etc.).

## 🧠 **Real-World Case Study: "The Lean Startup"**

- **Scenario:** A startup had 3 developers and 10 microservices. They tried to use Kubernetes but spent 50% of their time fixing cluster configuration instead of writing code.
- **The Pivot:** They simplified and moved to Docker Swarm.
- **The Outcome:** The migration took 1 day. Suddenly, they could deploy with a single command. The developers were happy, and the system was reliable enough for their 10,000 users.
- **The Lesson:** Don't use a "Chainsaw" (Kubernetes) to cut a "Piece of Bread" (Small/Medium app). Swarm is the right tool for most non-enterprise stacks.

## 🤔 **Reflective Questions**

1. Why is Docker Swarm called "Batteries Included"?
2. Can you run Kubernetes inside Docker? (Answer: Yes, using `kind` or `minikube` for testing).
3. If you decide to move from Swarm to Kubernetes later, is your Docker image still valid? (Answer: Yes! The image is universal).

## 📚 **Further Reading**

- **Comparison:** [Swarm vs Kubernetes](https://www.docker.com/blog/docker-swarm-vs-kubernetes-which-one-should-you-choose/)
- **Case Study:** [Why we chose Swarm for production](https://blog.octo.com/en/docker-swarm-mode-is-it-still-alive-and-kicking/)

## 📝 **Mini Task (Production)**

1. Look at a simple `docker-compose.yml` file.
2. Research the `deploy:` key inside a service.
3. Does Kubernetes use the `deploy:` key? (Hint: No, it uses `Deployments` and `Services` as separate objects).
4. List 3 things you like about the simplicity of Docker Compose that you might lose in Kubernetes.
5. If you were starting a new project with 2 servers today, which orchestrator would you pick? Why?
