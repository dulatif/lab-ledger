# 💡 DK02.1.3: Mode Jaringan Lainnya

**Outline:**

- **The Alternatives (SEEI):** Exploring the `host` and `none` network drivers.
- **The Trade-offs (PPP):** Understanding when to prioritize performance (`host`) or security (`none`) over isolation.
- **Your Mission (Production):** Comparing container behavior in different networking modes.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Alternatives (SEEI)**

While `bridge` is the default, Docker provides other drivers for specific use cases:

1. **`host` Mode:** The container shares the host's networking namespace. It doesn't get its own IP; it uses the host's IP directly.
   - _Analogy: Instead of living in a separate house, the container is just another room in your own house._
2. **`none` Mode:** The container has no external networking. It only has a loopback interface (`localhost`).
   - _Analogy: A panic room with no windows, doors, or phone lines._

### **Part 2: Practice - The Trade-offs (PPP)**

**Comparing Drivers:**

| Driver     | Isolation | Performance | Best For...                              |
| :--------- | :-------- | :---------- | :--------------------------------------- |
| **Bridge** | High      | Medium      | Most apps, microservices.                |
| **Host**   | None      | High        | High-performance apps, networking tools. |
| **None**   | Maximum   | N/A         | Batch jobs that only process local data. |

**Commands to Run:**

```bash
# Host mode (No port mapping needed!)
docker run -d --network host nginx

# None mode (Completely isolated)
docker run -d --network none alpine sleep 1000
```

## 🧠 **Real-World Case Study: "The Performance Bottleneck"**

- **Scenario:** A developer is running a high-frequency packet analyzer that needs to process 1,000,000 requests per second.
- **The Problem:** The `bridge` network adds too much overhead because of NAT and the virtual switch logic.
- **The Solution:** Switching to `--network host`.
- **The Result:** The latency drops significantly, as the container reads directly from the host's network card.
- **The Warning:** Since the container shares the host's ports, if the container uses port 80, your host's port 80 is now occupied. You can't run two containers on the same host with `host` mode if they both need the same port.

## 🤔 **Reflective Questions**

1. Why would you ever want a container with `none` networking?
2. In `host` mode, do you still need to use the `-p` flag? (Answer: No, it's ignored).
3. What is the biggest security risk of using `host` networking?

## 📚 **Further Reading**

- **Docker Orientation:** [Network drivers](https://docs.docker.com/network/#network-drivers)
- **Reference:** [Host networking](https://docs.docker.com/network/drivers/host/)

## 📝 **Mini Task (Production)**

1. Run an Nginx container in `host` mode:
   ```bash
   docker run -d --network host --name host-web nginx
   ```
2. Try to visit `localhost` in your browser. It should work immediately without `-p 80:80`.
3. Stop and remove it.
4. Now run an alpine container in `none` mode:
   ```bash
   docker run -d --network none --name isolated alpine sleep 1000
   ```
5. Try to ping anything from inside it:
   ```bash
   docker exec isolated ping google.com
   ```
   Can it reach the outside world?
6. Run `docker exec isolated ip addr` and observe the available interfaces.
