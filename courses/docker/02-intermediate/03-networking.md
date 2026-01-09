# 3. Docker Networking Basics

## 🎯 Learning Goal

Understand how containers talk to each other and the world.

## 🧠 Concept

Docker creates a software-defined network.
By default, containers run in the "Bridge" network.

### 1. The Bridge Network

- It's a private internal network (e.g., `172.17.0.0/16`).
- Containers on the same bridge can talk to each other.
- Containers get an internal IP (e.g., `172.17.0.2`). **These IPs change** on restart.

### 2. Service Discovery (DNS)

- **Problem**: Since IPs change, how does my API connect to my DB?
- **Solution**: Docker DNS.
- **How**: If you put two containers on a **User-Defined Bridge Network**, they can resolve each other by **Container Name**.
  - Container A (`api`) wants to call Container B (`db`).
  - A calls `postgres://db:5432`.
  - Docker DNS resolves `db` to `172.17.0.5`.

## 💻 Implementation

```bash
# 1. Create a network
docker network create my-app-net

# 2. Run DB attached to network
docker run -d --name mongo-db --network my-app-net mongo

# 3. Run App attached to network
docker run -d --name my-app --network my-app-net \
  -e DB_URL=mongodb://mongo-db:27017 \
  my-app-image
```

Notice we used `mongo-db` (the container name) as the hostname.

## 💻 Other Network Drivers

- **Host (`--network host`)**: Removes isolation. Container uses Host's IP stack directly. Fast, but dangerous/messy ports.
- **None**: No network access. Sandbox.

## 🧩 Activity / Challenge

1.  Create a network `lab-net`.
2.  Run two `alpine` containers on it: `c1` and `c2`.
3.  Exec into `c1`: `docker exec -it c1 sh`.
4.  Ping `c2`: `ping c2`. It works!
5.  Try to ping `c2` from a container _not_ on the network. It fails.

## 🔑 Key Takeaways

- **Never** rely on IP addresses. Use Container Names + Custom Networks.
- Isolate stacks (Dev, Test) using separate networks.
