# 💡 DK02.2.1: Membuat Jaringan Kustom

**Outline:**

- **The Command (SEEI):** Mastering the `docker network create` CLI.
- **The Options (PPP):** Defining subnets, gateways, and internal flags.
- **Your Mission (Production):** Orchestrating a private network for your app stack.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Command (SEEI)**

**Why Create Your Own?**
As we learned in Chapter 1, the default bridge has no DNS support. Creating a custom network solves this and allows you to isolate different parts of your infrastructure.

**The Basic Syntax:**

```bash
docker network create [OPTIONS] NETWORK
```

### **Part 2: Practice - The Options (PPP)**

**Advanced Network configuration:**
Sometimes you need more control over your network's IP range to avoid conflicts with your company's physical network.

1. **Subnetting:**
   ```bash
   docker network create --subnet 10.10.0.0/16 my-secure-net
   ```
2. **Internal Only:**
   _Create a network that has NO access to the internet._
   ```bash
   docker network create --internal isolated-net
   ```

**Managing Networks:**

- **List:** `docker network ls`
- **Inspect:** `docker network inspect <name>`
- **Delete:** `docker network rm <name>`
- **Clean up:** `docker network prune` (Removes all unused networks).

## 🧠 **Real-World Case Study: "The Subnet Conflict"**

- **Scenario:** A dev team starts Docker Desktop, and suddenly they can't access their company's internal Git server.
- **The Diagnosis:** Docker's default bridge picked an IP range (e.g., `172.17.x.x`) that overlapped with the Git server's IP range.
- **The Solution:** They delete the offending networks and create custom networks with a safe, non-conflicting subnet:
  ```bash
  docker network create --subnet 192.168.100.0/24 office-safe-net
  ```
- **The Lesson:** In corporate environments, always check your IP ranges before letting Docker choose them automatically.

## 🤔 **Reflective Questions**

1. Why would you want to create a network with the `--internal` flag?
2. What happens to running containers if you delete the network they are connected to? (Note: You can't delete a network while it has active containers).
3. How do you find the "Gateway" IP of a custom network?

## 📚 **Further Reading**

- **Docker CLI:** [docker network create reference](https://docs.docker.com/engine/reference/commandline/network_create/)
- **Guide:** [Networking with standalone containers](https://docs.docker.com/network/network-tutorial-standalone/)

## 📝 **Mini Task (Production)**

1. Create a network named `secure-backend` with a specific subnet:
   ```bash
   docker network create --subnet 10.5.0.0/24 secure-backend
   ```
2. Inspect the network. Does the "Gateway" match what you expected?
3. Create another network named `untrusted-net` with the `--internal` flag.
4. Try to start an alpine container in `untrusted-net` and ping `google.com`. It should fail!
   ```bash
   docker run --rm --network untrusted-net alpine ping -c 2 google.com
   ```
