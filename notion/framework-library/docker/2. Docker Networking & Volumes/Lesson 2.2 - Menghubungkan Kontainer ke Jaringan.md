# 💡 DK02.2.2: Menghubungkan Kontainer ke Jaringan

**Outline:**

- **The Connection (SEEI):** Attaching containers to networks at startup and during runtime.
- **The Multiple-Network Pattern (PPP):** Allowing a container to bridge two different networks.
- **Your Mission (Production):** Dynamically wiring containers together.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Connection (SEEI)**

**When to Connect?**
There are two ways to get a container into a network:

1. **At Startup (Most Common):**
   ```bash
   docker run --network <name> ...
   ```
2. **During Runtime (The "Plugging In" method):**
   ```bash
   docker network connect <network> <container>
   ```

**The Power of Connection**
Unlike physical servers where you have to plug in a cable, Docker allows you to add a "Virtual NIC" (Network Interface Card) to a running container instantly.

### **Part 2: Practice - The Multiple-Network Pattern (PPP)**

**The "Proxy" or "Gateway" Pattern**
A container can belong to multiple networks. This is essential for security.

**The Workflow:**

1. Create `frontend` and `backend` networks.
2. Start the `database` on `backend`.
3. Start the `api` on `backend`.
4. **CONNECT** the `api` to `frontend` as well.
5. Start the `web-ui` on `frontend`.

**The Result:** The `web-ui` can talk to the `api`. The `api` can talk to the `database`. But the `web-ui` has **no physical way** to talk to the `database`.

**Commands:**

```bash
docker network connect frontend api
docker network disconnect frontend api
```

## 🧠 **Real-World Case Study: "The Hot-Fix Connection"**

- **Scenario:** A production database is having performance issues, but it's isolated on a `management-only` network. The developer's debug tool is on a different network.
- **The Solution:** Instead of restarting the database (and causing downtime), the admin runs:
  ```bash
  docker network connect debugging-net prod-db
  ```
- **The Result:** The debug tool can now "see" the database, perform the fix, and then the admin **disconnects** it.
- **The Lesson:** Docker networking is dynamic. Use `connect` and `disconnect` for maintenance without rebooting services.

## 🤔 **Reflective Questions**

1. If a container is in two networks, how many IP addresses does it have? (Answer: Two, one for each network).
2. What is the benefit of connecting a container to a network _after_ it has already started?
3. Can a container communicate with itself via its name in a custom network?

## 📚 **Further Reading**

- **Docker CLI:** [docker network connect docs](https://docs.docker.com/engine/reference/commandline/network_connect/)
- **Tutorial:** [Manage container networking](https://docs.docker.com/config/containers/container-networking/)

## 📝 **Mini Task (Production)**

1. Create two networks: `net1` and `net2`.
2. Start a container `lone-wolf` in `net1`:
   ```bash
   docker run -d --name lone-wolf --network net1 alpine sleep 1000
   ```
3. Start another container `bridge-builder` in `net2`:
   ```bash
   docker run -d --name bridge-builder --network net2 alpine sleep 1000
   ```
4. Try to ping `lone-wolf` from `bridge-builder` by name. It fails!
5. Now, connect `lone-wolf` to `net2`:
   ```bash
   docker network connect net2 lone-wolf
   ```
6. Try the ping again. It works!
7. Use `docker inspect lone-wolf` to see both network configurations.
