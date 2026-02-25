# 💡 DK05.5.2: Strategi Deployment Zero-Downtime

**Outline:**

- **The Configuration (SEEI):** Fine-tuning the update parameters to ensure users never see an error.
- **The Coordination (PPP):** Using `--update-parallelism` and `--update-delay` effectively.
- **Your Mission (Production):** Running a live update on a web service without losing connection.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Configuration (SEEI)**

**"Zero Downtime" is the target**
For your update to be truly silent, the number of "Healthy" containers must never drop below a level that can handle the traffic.

**The Variables of an Update:**

1. **Parallelism:** How many containers to update at once.
2. **Delay:** How long to wait between batches.
3. **Failure Action:** What to do if an update fails (e.g., `pause` or `rollback`).
4. **Order:** Whether to stop the old one first, or start the new one first (`stop-first` vs `start-first`).

### **Part 2: Practice - The Coordination (PPP)**

**The Zero-Downtime Command:**

```bash
docker service update \
  --image nginx:alpine \
  --update-parallelism 1 \
  --update-delay 10s \
  --update-order start-first \
  my-web-service
```

**Why `start-first`?**
Using `start-first` ensures that the new container is running **before** the old one is removed. For a split second, you have "N+1" containers. This is the safest way to prevent even a millisecond of "Connection Refused."

_(Note: `start-first` requires enough spare RAM on your nodes to handle the extra container temporarily)._

## 🧠 **Real-World Case Study: "The Heavy Traffic Update"**

- **Scenario:** A news site is experiencing a breaking story (high traffic). They need to push a fix for the "Breaking News" ticker.
- **The Risk:** If they use the default `stop-first` order, they might lose capacity during the switch.
- **The Solution:** They use `--update-order start-first`.
- **The Result:** On each node, a new ticker container started, and ONLY once it was ready did the old one go away. The CPU usage spiked slightly, but the site stayed blazing fast for the millions of viewers.
- **The Lesson:** If you have the RAM, `start-first` is the gold standard for zero-downtime.

## 🤔 **Reflective Questions**

1. When would you use `stop-first` instead of `start-first`? (Answer: When you have a Database or a Singleton that cannot have two copies alive at the same time).
2. What happens if you don't set an `--update-delay`? (Answer: Swarm moves to the next batch immediately).
3. How do you check the progress of an active rolling update? (`docker service ps [name]`).

## 📚 **Further Reading**

- **Docker Blog:** [Deploying with zero downtime](https://www.docker.com/blog/how-to-deploy-on-docker-swarm-with-zero-downtime/)
- **Guide:** [Update configuration reference](https://docs.docker.com/engine/reference/commandline/service_update/#update-config)

## 📝 **Mini Task (Production)**

1. On PWD or your local Swarm, create a service with 5 replicas.
2. Run a continuous ping or a tool like `watch curl localhost` to keep hitting the service.
3. Update the service image: `docker service update --image nginx:latest --update-parallelism 1 my-service`.
4. Watch the `curl` output. Did any requests fail?
5. Now, update again but use `--update-parallelism 5` (Big Bang).
6. Compare the results.
7. Reflect: Which strategy would you use for your company's checkout page?
