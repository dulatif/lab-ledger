# 💡 DK02.1.4: Keuntungan Jaringan Kustom

**Outline:**

- **The Upgrade (SEEI):** Introducing User-Defined Bridge networks.
- **The Power (PPP):** Why names are better than IP addresses for service discovery.
- **Your Mission (Production):** Witnessing the "magic" of automatic DNS.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Upgrade (SEEI)**

**User-Defined Bridges**
Instead of using the default `bridge`, you should create your own. This is the "Industry Standard" way to handle container communication on a single host.

**Why is it better?**

1. **Automatic Service Discovery:** Containers can find each other by name.
2. **Better Isolation:** You can group related containers (e.g., `frontend-net` and `backend-net`).
3. **On-the-fly Connection:** You can connect/disconnect running containers without restarting them.

### **Part 2: Practice - The Power (PPP)**

**Built-in DNS (The Killer Feature)**
In a user-defined network, Docker runs a small DNS server. It maps every container's name to its current IP.

**The Workflow:**

1. **Create:** `docker network create my-net`
2. **Join:** `docker run -d --network my-net --name web nginx`
3. **Connect:** `docker run --network my-net alpine ping web`

**Observe:** The `ping web` command works instantly because Docker resolves `web` to its IP address!

## 🧠 **Real-World Case Study: "Grouping for Security"**

- **Scenario:** You have a Web App, a Public API, and a Database.
- **The Setup:**
  - `frontend-net`: Contains Web App and Public API.
  - `backend-net`: Contains Public API and Database.
- **The Result:** The Web App can talk to the API, but it **cannot** talk to the Database directly. The API acts as a gateway.
- **The Benefit:** If the Web App is hacked, the Database is still logically isolated on a different network.

## 🤔 **Reflective Questions**

1. Why doesn't the default bridge support DNS by container name? (Historical design choice).
2. Can a container belong to two networks at the same time? (Answer: Yes!).
3. What happens if you try to `ping` a container that is NOT in your network?

## 📚 **Further Reading**

- **Docker Guide:** [Use user-defined bridge networks](https://docs.docker.com/network/drivers/bridge/#use-user-defined-bridge-networks)
- **Deep Dive:** [Embedded DNS in Docker](https://docs.docker.com/network/#dns-services)

## 📝 **Mini Task (Production)**

1. Create a network: `docker network create lab-net`.
2. Start a container named `db` in that network:
   ```bash
   docker run -d --network lab-net --name db redis
   ```
3. Start another container named `app` in the SAME network:
   ```bash
   docker run -it --network lab-net --name app alpine sh
   ```
4. From inside `app`, try `ping db`. It works!
5. Now, start another container in the **default** bridge:
   ```bash
   docker run -it --name stranger alpine sh
   ```
6. Try `ping db` from `stranger`. Why does it fail? (1. Different network, 2. No DNS on default bridge).
