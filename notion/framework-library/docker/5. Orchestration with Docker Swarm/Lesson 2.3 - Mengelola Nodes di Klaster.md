# 💡 DK05.2.3: Mengelola Nodes di Klaster

**Outline:**

- **The Visibility (SEEI):** Using `docker node ls` and `inspect` to monitor the health of your cluster.
- **The Operations (PPP):** Promoting, demoting, and removing nodes safely.
- **Your Mission (Production):** Performing maintenance on a live swarm node.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Visibility (SEEI)**

**"Who is on my team?"**
As a Manager, you need to see the status of all your workers. The command `docker node ls` is your "Dashboard."

**Key Icons and Statuses:**

- **`*`**: Indicates the node you are currently logged into.
- **AVAILABILITY**:
  - `Active`: Can receive new tasks.
  - `Pause`: Existing tasks stay, but no new tasks are assigned.
  - `Drain`: Move all existing tasks off this node (used for maintenance).
- **MANAGER STATUS**:
  - `Leader`: The primary decision maker.
  - `Reachable`: A backup manager ready to take over.

### **Part 2: Practice - The Operations (PPP)**

**Maintaining the Cluster:**

1. **Promoting a Node:** _Upgrade a loyal worker to a manager._
   ```bash
   docker node promote <NODE_ID>
   ```
2. **Draining for Maintenance:** _Crucial before rebooting a server._
   ```bash
   docker node update --availability drain <NODE_ID>
   ```
   _Swarm will automatically move the containers from this node to others._
3. **Removing a Node:**
   - On the node: `docker swarm leave`.
   - On the manager: `docker node rm <NODE_ID>`.

## 🧠 **Real-World Case Study: "The Zero-Downtime Reboot"**

- **Scenario:** A sysadmin needs to update the security patches on `Worker-03`, which requires a reboot.
- **The Wrong Way:** Just typing `sudo reboot`. (This would cause 10% of users to see errors during the 5 minutes the server is down).
- **The Right Way:**
  1. The sysadmin runs `docker node update --availability drain Worker-03`.
  2. Swarm gracefully moves all traffic to other nodes.
  3. The sysadmin reboots, updates, and stays calm.
  4. Once back, they run `docker node update --availability active Worker-03`.
- **The Result:** The app stayed 100% online.
- **The Lesson:** "Drain" is your best friend for non-disruptive maintenance.

## 🤔 **Reflective Questions**

1. If you "Demote" a Leader Manager, who becomes the new leader? (Answer: The Raft algorithm automatically elects one of the "Reachable" managers).
2. Why can't you run `docker node ls` on a Worker node?
3. What is the difference between "Pause" and "Drain"?

## 📚 **Further Reading**

- **Docker Documentation:** [Manage nodes in a swarm](https://docs.docker.com/engine/swarm/manage-nodes/)
- **Guide:** [Draining a node](https://docs.docker.com/engine/swarm/manage-nodes/#drain-a-node-on-the-swarm)

## 📝 **Mini Task (Production)**

1. Run `docker node ls`.
2. Pick a node (even if you only have one) and "Pause" its availability.
3. Run `docker node ls` again. What changed?
4. Now, "Drain" the node and reflect: If this node were running a mission-critical database, what would happen right now?
5. Finally, set it back to "Active".
6. Research: How do you identify a node by its **ID** instead of its **Hostname**?
