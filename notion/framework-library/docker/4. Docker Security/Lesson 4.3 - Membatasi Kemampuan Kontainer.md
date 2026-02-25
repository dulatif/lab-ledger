# 💡 DK04.4.3: Membatasi Kemampuan Kontainer

**Outline:**

- **The Capabilities (SEEI):** Understanding that "Root" in a container is broken down into specific OS privileges.
- **The Removal (PPP):** Using `--cap-drop` and `--cap-add` to follow the Principle of Least Privilege.
- **Your Mission (Production):** Stripping a container of its ability to perform network attacks or modify files.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Capabilities (SEEI)**

**Not all Root is created equal**
Linux "Capabilities" allow you to break down the traditionally all-powerful `root` user into smaller, specific permissions. By default, Docker grants a subset of these capabilities to every container.

**The Risk:**
A standard container has the permission to:

- Bind to low ports (<1024).
- Send "Kill" signals to other processes.
- ...and many others that a simple Web App doesn't actually need.

**The "Defense":**
If an attacker takes over your container, the first thing they will try to do is use these default capabilities to sniff the network or change internal system clocks.

### **Part 2: Practice - The Removal (PPP)**

**The "Drop All" Strategy:**
The most secure way to run a container is to drop EVERY capability and then add back only what you strictly need.

**Example Command:**

```bash
docker run -d \
  --cap-drop ALL \
  --cap-add NET_BIND_SERVICE \
  nginx
```

_In this example, Nginx can still listen on port 80 (NET_BIND_SERVICE), but it can't do anything else (like change the system time or reboot the container)._

**Common Capabilities:**

- `CHOWN`: Change file ownership.
- `SETUID`: Change user ID of processes.
- `NET_RAW`: Use raw sockets (useful for pinging, but also for spoofing).

## 🧠 **Real-World Case Study: "The Network Sniffer"**

- **Scenario:** An attacker compromised a NodeJS container on a company's bridge network.
- **The Attack:** They used the default `NET_RAW` capability to start "ARP Poisoning" and sniff traffic from other containers on the same host.
- **The Preventative:** Had the container been started with `--cap-drop NET_RAW`, the attack would have failed instantly.
- **The Lesson:** "Drop All" shouldn't be an option; it should be your default policy for production containers.

## 🤔 **Reflective Questions**

1. If you run a container with the `--privileged` flag, what happens to the capabilities? (Answer: It gets ALL of them, which is extremely dangerous).
2. Can you drop capabilities in a `docker-compose.yml` file? (Answer: Yes, using the `cap_drop` and `cap_add` keys).
3. Why does a container need `NET_BIND_SERVICE` if it wants to listen on port 443?

## 📚 **Further Reading**

- **Docker Documentation:** [Runtime privilege and Linux capabilities](https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities)
- **Linux Manual:** [Capabilities(7) man page](https://man7.org/linux/man-pages/man7/capabilities.7.html)

## 📝 **Mini Task (Production)**

1. Start an alpine container with all capabilities dropped:
   ```bash
   docker run --rm -it --cap-drop ALL alpine sh
   ```
2. Inside the container, try to change the owner of a file: `chown 1000 /etc/passwd`. Did it work?
3. Try to use `ping`. Did it work?
4. Now, exit and start a new one with `--cap-add CHOWN`.
5. Try the `chown` command again.
6. Reflect: Why is this one of the most powerful security features of Docker?
