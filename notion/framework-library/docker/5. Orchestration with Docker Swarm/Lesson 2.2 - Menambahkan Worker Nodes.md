# 💡 DK05.2.2: Menambahkan Worker Nodes

**Outline:**

- **The Expansion (SEEI):** Using the join token to add "Brawn" to your cluster.
- **The Authentication (PPP):** Understanding how Swarm uses tokens to keep the cluster secure.
- **Your Mission (Production):** Scaling your cluster from 1 node to 3 nodes.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Expansion (SEEI)**

**Joining the Armada**
A cluster of one is just a lonely server. To get the benefits of high availability, you need to add more servers (nodes).

**The Step-by-Step:**

1. Log in to a second server (the potential **Worker**).
2. Ensure Docker is installed.
3. Paste the join command provided by the Manager.

**The command looks like this:**

```bash
docker swarm join --token <TOKEN_HERE> <MANAGER_IP>:2377
```

**The "Success" Message:**
If successful, you will see: `This node joined a swarm as a worker.`

### **Part 2: Practice - The Authentication (PPP)**

**The Security of the Token**
The Join Token isn't just a password; it's an encrypted string that tells the Manager what role (Worker or Manager) the new node should have.

**Managing Tokens:**

- **Rotate them:** If you think a token was leaked, you should rotate it using `docker swarm join-token --rotate worker`.
- **Retrieve them:** If you forgot the command, run `docker swarm join-token worker` on any Manager node.

**Worker vs. Manager Tokens:**

- A **Worker Token** allows a node to run tasks but NOT make decisions.
- A **Manager Token** gives the node full administrative power. **Be very careful with this one!**

## 🧠 **Real-World Case Study: "The Accidental Manager"**

- **Scenario:** A dev accidentally used the Manager Join Token to add 10 worker nodes.
- **The Issue:** Suddenly, the cluster had 11 Managers for its "Brain" (the original 1 + the 10 new ones).
- **The Performance Hit:** Because Raft needs a majority of Managers to agree on everything, the cluster slowed to a crawl trying to get 6 out of 11 nodes to agree on tiny state changes.
- **The Fix:** They simplified. They "Demoted" the 10 workers back to their correct role.
- **The Lesson:** Workers are the "Muscle." Only add "Brains" (Managers) when you specifically need High Availability (usually 3 or 5 total).

## 🤔 **Reflective Questions**

1. Can a Worker node promote itself to a Manager? (Answer: No, it must be promoted by an existing Manager).
2. What happens if a Worker node loses its internet connection to the Manager?
3. How many nodes can you add to a single Swarm cluster? (Answer: Thousands!).

## 📚 **Further Reading**

- **Docker Orientation:** [Join nodes to a swarm](https://docs.docker.com/engine/swarm/swarm-tutorial/join-nodes/)
- **Guide:** [Manage node membership](https://docs.docker.com/engine/swarm/manage-nodes/)

## 📝 **Mini Task (Production)**

1. On your Manager node, run `docker swarm join-token worker`.
2. Compare the output with the command to add a Manager: `docker swarm join-token manager`.
3. Notice how the token strings are different.
4. If you have a second computer or a virtual machine, try to join it to your swarm.
5. If you don't have a second machine, research "Play with Docker" (PWD) which allows you to simulate multiple nodes for free.
