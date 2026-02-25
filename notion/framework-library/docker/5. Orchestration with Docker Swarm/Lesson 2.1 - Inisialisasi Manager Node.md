# 💡 DK05.2.1: Inisialisasi Manager Node

**Outline:**

- **The Command (SEEI):** Using `swarm init` to transform a standalone Docker engine into a cluster manager.
- **The Advertise Address (PPP):** Understanding how to specify the network interface for cluster communication.
- **Your Mission (Production):** Launching your first Swarm cluster hub.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Command (SEEI)**

**"Engage Swarm Mode"**
By default, Docker runs in "Standalone" mode. To start a cluster, you must choose one server to be the first **Manager**.

**The Command:**

```bash
docker swarm init
```

**What happens behind the scenes?**

1. Docker creates the **Raft consensus database** (the "Brain" of the cluster).
2. It generates **Join Tokens** (passwords) for adding more nodes later.
3. It creates a special **Overlay Network** called `ingress` for routing traffic.
4. It sets up internal PKI (Public Key Infrastructure) to encrypt all cluster communication.

### **Part 2: Practice - The Advertise Address (PPP)**

**Which IP should I use?**
If your server has multiple IP addresses (e.g., a public IP and a private internal IP), you must tell Swarm which one to use for "Talking" to other nodes. This is the **Advertise Address**.

**Example:**

```bash
docker swarm init --advertise-addr 192.168.1.10
```

**The Output:**
When you run the command, Docker will print a long string like this:
`docker swarm join --token SWMTKN-1-XXXX... 192.168.1.10:2377`
**Save this!** This is the "Key" that other servers will use to join your cluster.

## 🧠 **Real-World Case Study: "The Secret IP"**

- **Scenario:** A developer ran `docker swarm init` on an AWS server with both a Public IP and a Private IP.
- **The Issue:** They didn't use `--advertise-addr`. Swarm picked the Public IP by default.
- **The Consequence:** When they tried to join worker nodes over the private network, the connection failed because the Manager was trying to "Listen" on the public internet.
- **The Fix:** They ran `docker swarm leave --force` and re-initialized with the **Private IP**.
- **The Lesson:** In multi-interface environments (like Cloud), always be explicit with your `--advertise-addr`.

## 🤔 **Reflective Questions**

1. Can you run `docker swarm init` more than once on the same server? (Answer: No, you must leave the swarm first).
2. What is the port used for Swarm management traffic? (Answer: Port 2377).
3. If you lose the "Join Token," is your cluster locked forever? (Answer: No, you can retrieve it with `docker swarm join-token worker`).

## 📚 **Further Reading**

- **Docker Orientation:** [Create a swarm](https://docs.docker.com/engine/swarm/swarm-tutorial/create-swarm/)
- **Guide:** [Swarm mode ports](https://docs.docker.com/engine/swarm/swarm-tutorial/#open-protocols-and-ports-between-the-hosts)

## 📝 **Mini Task (Production)**

1. On your Docker machine, run `docker swarm init`.
2. Look at the output. Can you identify the **Token** and the **IP:Port**?
3. Run `docker node ls`.
4. How many nodes are currently in the cluster? What is their status?
5. What is the "Leader" status? Why is it there?
