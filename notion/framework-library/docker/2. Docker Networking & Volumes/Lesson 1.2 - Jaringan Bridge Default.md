# 💡 DK02.1.2: Jaringan Bridge Default

**Outline:**

- **The Standard (SEEI):** Understanding the `bridge` network that Docker creates by default.
- **The Limitations (PPP):** Why the default bridge is unsuitable for production multi-container apps.
- **Your Mission (Production):** Analyzing the properties of the default bridge.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Standard (SEEI)**

**The "Standard Out-of-the-Box" Network**
When you install Docker, it automatically creates a network named `bridge`. If you run a container without the `--network` flag, it is attached to this network.

**Key Features:**

- **Software Bridge:** It is a software-defined link that allows containers connected to it to communicate.
- **NAT (Network Address Translation):** It allows containers to reach the internet using the host machine's IP.
- **Isolation:** It provides a basic level of isolation from the host and other networks.

### **Part 2: Practice - The Limitations (PPP)**

**Why you should usually AVOID the default bridge:**

1. **No Automatic Service Discovery:** In the default bridge, containers cannot find each other by name (e.g., `ping database` won't work). You must use IP addresses.
2. **Unwanted Proximity:** All containers on the host end up in the same network by default, which can lead to security risks if one container is compromised.
3. **Manual Port Mapping Required:** Communicating with the host always requires explicit `-p` publishing.

**Command to Analyze:**

```bash
docker network inspect bridge
```

_Look at the "Containers" section to see everything sharing this network._

## 🧠 **Real-World Case Study: "The DNS Dilemma"**

- **Scenario:** A team deploys an app with `docker run app` and `docker run db`.
- **The Issue:** The developer has to run a script to find the DB's IP and inject it into the app's environment variables every time.
- **The Friction:** This makes scaling and automated recovery almost impossible.
- **The Solution:** Creating a **User-Defined Network** (the topic of our next chapter) which enables built-in DNS.

## 🤔 **Reflective Questions**

1. Why does Docker provide a default bridge if it has so many limitations?
2. How do containers on the default bridge reach external websites?
3. Is it possible to rename the default `bridge` network?

## 📚 **Further Reading**

- **Docker Reference:** [Default bridge network](https://docs.docker.com/network/drivers/bridge/#use-the-default-bridge-network)
- **Comparison:** [User-defined vs Default bridge](https://docs.docker.com/network/drivers/bridge/#differences-between-user-defined-bridges-and-the-default-bridge)

## 📝 **Mini Task (Production)**

1. Run `docker network inspect bridge`.
2. Find the "Subnet" and the "Gateway" IP addresses.
3. Start a container: `docker run -d --name test-bridge alpine sleep 1000`.
4. Inspect the bridge again. Can you see your container's MAC address and IP address listed in the JSON output?
5. Try to access the internet from that container:
   ```bash
   docker exec test-bridge ping -c 4 google.com
   ```
   If it works, your default bridge is correctly performing NAT.
