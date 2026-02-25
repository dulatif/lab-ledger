# 💡 DK05.5.3: Rollback Otomatis Saat Gagal

**Outline:**

- **The Safety Brake (SEEI):** Understanding the "Self-Correction" mechanism when a new version fails.
- **The Threshold (PPP):** Configuring Swarm to automatically revert to the previous working version.
- **Your Mission (Production):** Purposely deploying a broken image and watching Swarm "Undo" your mistake.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Safety Brake (SEEI)**

**The Developer's Worst Nightmare**
You push an update at 5 PM on a Friday. The deployment starts, but after 5 minutes, you realize the new containers are all crashing.

**The "Manual" Recovery:**
You have to find the previous image tag, run the update command again, and hope you didn't break things further.

**The "Swarm" Recovery (Automatic Rollback):**
Docker Swarm can monitor the health of the new version _during_ the update. If too many of the new containers fail, Swarm will:

1. Stop the update immediately.
2. Revert all containers back to the **Previous Version** that was known to be working.

### **Part 2: Practice - The Threshold (PPP)**

**The "Self-Healing" Update Configuration:**

```bash
docker service update \
  --image my-app:broken \
  --update-failure-action rollback \
  --update-max-failure-ratio 0.2 \
  my-app
```

**Key Parameters:**

- **`--update-failure-action rollback`**: Tell Swarm to "Undo" instead of just "Pause."
- **`--update-max-failure-ratio 0.2`**: If more than 20% of the new containers fail, trigger the rollback.
- **`--rollback-parallelism`**: How many to revert at once (usually keep the same as update parallelism).

**Manual Rollback:**
Sometimes you realize the app is broken _after_ the update finishes. You can trigger the undo manually:

```bash
docker service update --rollback my-app
```

## 🧠 **Real-World Case Study: "The Friday Saver"**

- **Scenario:** A dev pushed a new image that had a database connection bug.
- **The Deployment:** The rolling update started. The first new container reached the cluster.
- **The Detection:** Because the container couldn't connect to the DB, its health check failed. Swarm waited for the `update-monitor` period (e.g., 20 seconds).
- **The Rollback:** Swarm detected that 100% of the _new_ containers were failing. It stopped the update and reverted that one broken container back to the old version.
- **The Result:** Only 1% of users experienced an error for 20 seconds. The other 99% stayed on the old version and never knew there was a problem.
- **The Lesson:** Always configure `rollback`. It's your permanent "Undo" button.

## 🤔 **Reflective Questions**

1. Can Swarm rollback if you don't use labels/tags properly? (Answer: No, it needs the record of what the previous image was).
2. What is the difference between "Pause" and "Rollback" as a failure action?
3. How long does Swarm wait before deciding a container is "Successful"? (Answer: Controlled by `--update-monitor`).

## 📚 **Further Reading**

- **Docker Documentation:** [Roll back to a previous version](https://docs.docker.com/engine/swarm/swarm-tutorial/rolling-update/#roll-back-to-a-previous-version)
- **Guide:** [Update & Rollback configuration](https://docs.docker.com/engine/reference/commandline/service_update/#rollback-config)

## 📝 **Mini Task (Production)**

1. Create a service with a working image (e.g., `redis:6`).
2. Update the service to an image that doesn't exist: `docker service update --image redis:nonexistent --update-failure-action rollback [service-name]`.
3. Watch the logs: `docker service ps [service-name]`.
4. You will see the `nonexistent` task marked as `Rejected` or `Failed`.
5. Watch as Swarm creates a new task with `redis:6` again.
6. Reflect: How much stress does this feature save a DevOps engineer?
