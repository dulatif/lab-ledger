# 💡 DK05.5.4: Praktik Terbaik Pembaruan Aplikasi

**Outline:**

- **The Discipline (SEEI):** Understanding the cultural and technical habits that lead to successful Swarm deployments.
- **The Checklist (PPP):** Implementing Healthchecks, Resource Limits, and Immutable Tags.
- **Your Mission (Production):** Reviewing a deployment plan for flaws.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Discipline (SEEI)**

**"It works on my machine!"**
Moving from a single host to a cluster requires more discipline. Your application must be "Orchestration Aware."

**The Golden Rules of Updates:**

1. **Never use `:latest`:** Always use specific version tags (e.g., `:v1.2.3`). If you use `:latest`, Swarm won't know the image has changed.
2. **Implement Healthchecks:** Swarm needs to know if your app is _actually_ working, not just "Running."
3. **Set Resource Limits:** Prevent a single container from "Starving" the rest of the cluster during an update.

### **Part 2: Practice - The Checklist (PPP)**

**The "Pro" Update Command Template:**

```bash
docker service update \
  --image my-registry.com/app:v2.1.0 \
  --update-parallelism 1 \
  --update-delay 10s \
  --update-monitor 30s \
  --update-failure-action rollback \
  --limit-cpu 0.5 \
  --limit-memory 512Mb \
  my-app
```

**Why these matter:**

- **Update Monitor:** Give your app 30 seconds to "Warm up" before Swarm declares it a success.
- **CPU/Memory Limits:** Ensure the host node stays stable while the new containers are starting up.
- **Failure Action:** Always default to `rollback` for production.

## 🧠 **Real-World Case Study: "The Image Override Disaster"**

- **Scenario:** A dev team pushed a fix to their registry using the same tag (`:prod`) every time.
- **The Problem:** They ran `docker service update --image app:prod my-service`.
- **The Failure:** Swarm saw that the service was _already_ using `app:prod`, so it did **Nothing**. The fix was never deployed.
- **The Result:** The bug stayed in production for 2 days until they realized their mistake.
- **The Fix:** They switched to **Semantic Versioning** (`:v1.0`, `:v1.1`).
- **The Lesson:** Tags should be **Immutable**. Once a tag is pushed, it should never change.

## 🤔 **Reflective Questions**

1. Why is a Docker `HEALTHCHECK` better than just checking if a process is running?
2. If your app is Stateless (doesn't save data locally), does that make it easier to update?
3. How do you handle database schema migrations during a rolling update? (Answer: This is a complex topic usually handled _before_ the app update).

## 📚 **Further Reading**

- **Docker Orientation:** [Best practices for writing Dockerfiles](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
- **Guide:** [Healthcheck reference](https://docs.docker.com/engine/reference/builder/#healthcheck)

## 📝 **Mini Task (Production)**

1. Research the `HEALTHCHECK` instruction in a Dockerfile.
2. Write a simple `HEALTHCHECK` for a web app that checks if `http://localhost:80/` returns a 200 status.
3. Why should this check be inside the Dockerfile rather than just a Swarm command?
4. Look at your most recent project. Are you using `:latest`? If yes, list 3 reasons why you should change that today.
