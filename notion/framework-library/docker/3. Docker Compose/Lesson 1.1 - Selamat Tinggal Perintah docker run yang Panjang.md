# 💡 DK03.1.1: Selamat Tinggal Perintah docker run yang Panjang

**Outline:**

- **The Problem (SEEI):** The complexity and error-proneness of managing multi-container stacks via the CLI.
- **The Solution (PPP):** Introducing Docker Compose as the orchestrator for multi-container applications.
- **Your Mission (Production):** Transforming a messy CLI list into a clean, declarative configuration.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Problem (SEEI)**

**The CLI Nightmare**
As your application grows, running it requires more and more complex commands. Imagine having to type this every time you want to start your environment:

```bash
docker run -d --name db --network my-net -v db-data:/data/db mongo
docker run -d --name api --network my-net -p 3000:3000 -e DB_URL=mongodb://db:27017 my-api:v1
docker run -d --name web --network my-net -p 80:80 my-web:v1
```

**The Negative Impact:**

1. **Forgetfulness:** You forget a flag, and the app breaks.
2. **Onboarding:** A new developer has to copy-paste several long commands.
3. **Reproducibility:** How do you move this exact stack to a staging server?

### **Part 2: Practice - The Solution (PPP)**

**The "Declarative" Approach**
Docker Compose allows you to define your entire multi-container application in a single file: `docker-compose.yml`. Instead of saying "Run this, then run that," you say "Here is what my system looks like."

**Advantages Of Docker Compose:**

- **Single Command:** Run `docker compose up` and everything starts in the right order.
- **Verison Control:** Your infrastructure is now a file that can be checked into Git.
- **Environment Parity:** The same `yml` file works on your laptop, a co-worker's laptop, and a CI server.

## 🧠 **Real-World Case Study: "The Morning Routine"**

- **Scenario:** Every morning, a developer spent 15 minutes manually starting 5 different containers (Redis, Mongo, API, Frontend, Worker).
- **The Friction:** They often made mistakes in port mapping or environment variables.
- **The Fix:** They spent 20 minutes writing a `docker-compose.yml` file.
- **The Result:** Now, they just type `docker compose up -d` and grab a coffee. In 5 seconds, the whole stack is up and healthy.
- **The Lesson:** Automation isn't just for production; it's a vital tool for developer productivity.

## 🤔 **Reflective Questions**

1. Why is "Declarative" configuration better than "Imperative" CLI commands?
2. Can Docker Compose manage containers on multiple different hosts? (Answer: No, it's for single-host orchestration. For multiple hosts, you need Swarm or Kubernetes).
3. If you change a setting in your `docker-compose.yml`, what command do you run to apply it?

## 📚 **Further Reading**

- **Docker Documentation:** [What is Docker Compose?](https://docs.docker.com/compose/)
- **Guide:** [Getting started with Compose](https://docs.docker.com/compose/gettingstarted/)

## 📝 **Mini Task (Production)**

1. Look at the three `docker run` commands in Part 1 above.
2. Identify:
   - How many networks are needed?
   - How many volumes are needed?
   - What are the dependencies between them? (Which one must start first?).
3. Try to imagine how you would represent the `api` container in a YAML format. (Don't worry about syntax yet, just think about the mapping).
