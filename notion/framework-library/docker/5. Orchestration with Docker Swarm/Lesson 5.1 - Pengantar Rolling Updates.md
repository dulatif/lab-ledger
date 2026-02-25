# 💡 DK05.5.1: Pengantar Rolling Updates

**Outline:**

- **The Big Bang Risk (SEEI):** Understanding the danger of "Stop-Update-Start" deployment strategies.
- **The Incremental Shift (PPP):** Defining how Swarm updates containers one by one to maintain uptime.
- **Your Mission (Production):** Visualizing a smooth transition between app versions.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Big Bang Risk (SEEI)**

**The "Old School" Deployment**
Before orchestrators, updating an app was painful:

1. Stop the current container (App goes offline).
2. Pull the new image.
3. Start the new container.
4. Hope it works.

**The Negative Impact:**

- **Downtime:** Users get "404" or "Connection Refused" during the switch.
- **High Risk:** If the new version is broken, the app stays down while you scramble to revert.

### **Part 2: Practice - The Incremental Shift (PPP)**

**Rolling Updates**
Docker Swarm solves this by updating containers in **Batches**.

**The Workflow:**

1. Swarm stops **one** container of the old version.
2. It starts a new container with the **new** version.
3. It waits for the new one to be "Healthy."
4. It moves on to the next one.

**Result:** At any given moment, most of your replicas are still online and serving traffic. This is the foundation of **Continuous Deployment**.

## 🧠 **Real-World Case Study: "The High-Stakes Update"**

- **Scenario:** A banking app needs to update its "Transaction History" service. They have 100,000 active users at any moment.
- **The Deployment:** They use a Rolling Update with a `parallelism` of 2.
- **The Process:** Swarm updates 2 containers, waits 30 seconds to check for errors, then updates the next 2.
- **The Outcome:** The update took 15 minutes to finish, but the users never experienced a single second of downtime. Some users were seeing the "Old" UI and some the "New" UI for a few minutes, but the bank stayed open.
- **The Lesson:** Trust the process. Slow and steady updates win the race.

## 🤔 **Reflective Questions**

1. Why does Swarm wait for a container to be "Healthy" before moving to the next one?
2. What happens if you have 10 replicas and set `parallelism` to 10? (Answer: It becomes a "Big Bang" update and causes downtime!).
3. Can you update a service's environment variables without changing the image? (Answer: Yes, it still triggers a rolling update).

## 📚 **Further Reading**

- **Docker Orientation:** [Apply rolling updates](https://docs.docker.com/engine/swarm/swarm-tutorial/rolling-update/)
- **Guide:** [Rolling update configuration](https://docs.docker.com/engine/reference/commandline/service_update/#update-parallelism)

## 📝 **Mini Task (Production)**

1. Imagine you have a service with 3 replicas.
2. You want to update them 1 by 1.
3. What flag would you use? (Hint: `--update-parallelism 1`).
4. If you want Swarm to wait 1 minute between each update, what flag would you add? (Hint: `--update-delay 1m`).
5. Reflect: How does this "Delay" help you notice bugs before they affect the entire cluster?
