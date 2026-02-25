# 💡 DK02.2.3: Resolusi DNS Internal

**Outline:**

- **The Automation (SEEI):** Understanding how Docker handles name-to-IP translation automatically.
- **The Process (PPP):** How a container finds another container in the same network.
- **Your Mission (Production):** Verifying DNS resolution and handling "Alias" names.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Automation (SEEI)**

**Goodbye IP Addresses**
In a user-defined network, you should almost never use hardcoded IP addresses (e.g., `172.18.0.2`). Instead, use the **Container Name** or a **Network Alias**.

**What is the Docker DNS?**
Docker runs a lightweight DNS server at `127.0.0.11` inside every container connected to a custom network. When your app tries to connect to `db`, the Docker DNS intercept the request and returns the current IP of the `db` container.

**The Negative Impact of No DNS:**

- Fragile code that breaks when containers are restarted.
- Need for complex service registry tools (Etcd, Consul) for simple local setups.
- Manual configuration of `/etc/hosts` (Don't do this!).

### **Part 2: Practice - The Process (PPP)**

**Name Resolution:**

1. You run `docker run --network my-net --name backend node-app`.
2. Any other container on `my-net` can reach it via `ping backend`.

**Using Network Aliases:**
You can give a container a nickname that is specific to a network. This is useful if you want multiple containers to share a generic name (like a load balancer pool).

```bash
docker run -d --network my-net --network-alias api node-app
```

_Now, other containers can reach this app using either its name OR the name `api`._

## 🧠 **Real-World Case Study: "The Database Switch"**

- **Scenario:** A production database named `db-old` needs to be replaced by `db-v2`.
- **The Old way:** Change the IP address in 10 different application config files and restart them all.
- **The Docker Way:**
  1. Start `db-v2` with `--network-alias db`.
  2. Stop `db-old`.
- **The Result:** The applications keep trying to connect to `db`. The DNS server simply starts pointing the name `db` to the new IP address of `db-v2`.
- **The Lesson:** Always code your apps to use a generic name like `db` or `api`, and use Docker Network Aliases to point that name to the correct container instance.

## 🤔 **Reflective Questions**

1. Why is `127.0.0.11` a special IP address in Docker?
2. What happens if two containers in the same network have the same `--network-alias`? (Answer: Docker DNS will return both IPs in a round-robin fashion—a simple Load Balancer!).
3. Can a container on the default `bridge` resolution names of containers on a custom network?

## 📚 **Further Reading**

- **Docker Orientation:** [Networking overview - DNS services](https://docs.docker.com/network/#dns-services)
- **Deep Dive:** [Internal DNS in Docker](https://docs.docker.com/network/drivers/bridge/#differences-between-user-defined-bridges-and-the-default-bridge)

## 📝 **Mini Task (Production)**

1. Create a network `dns-lab`.
2. Start a container `receiver` with an alias:
   ```bash
   docker run -d --network dns-lab --name receiver-real --network-alias receiver-alias alpine sleep 1000
   ```
3. Start a `tester` container:
   ```bash
   docker run -it --network dns-lab --name tester alpine sh
   ```
4. Inside `tester`, try:
   - `ping receiver-real` (Wait for it to resolve).
   - `ping receiver-alias` (Wait for it to resolve).
5. Run `cat /etc/resolv.conf` inside `tester`. What is the nameserver IP?
