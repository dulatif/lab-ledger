# 💡 DK04.4.1: Mengamankan Docker Daemon

**Outline:**

- **The Brain (SEEI):** Understanding the Docker Daemon's role and why its API is a high-value target.
- **The Shield (PPP):** Implementing TLS, limiting permissions, and avoiding the "Docker Socket" trap.
- **Your Mission (Production):** Hardening the communication between the Docker CLI and the Engine.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Brain (SEEI)**

**The Privileged Engine**
The Docker Daemon (`dockerd`) runs with root privileges on your host. If an attacker gains control over the Daemon, they effectively have control over your entire server.

**The Attack Vectors:**

- **The Docker Socket (`/var/run/docker.sock`):** If you mount this into a container, that container can control the host.
- **Remote API:** If you expose the Docker API over a network without encryption or authentication, anyone on the internet can run containers on your machine.

**The Negative Impact:**
An attacker can:

1. Start a privileged container that mounts the host's root filesystem.
2. Steal all data on the host.
3. Use your server as a botnet or crypto miner.

### **Part 2: Practice - The Shield (PPP)**

**Securing the Communication:**

1. **Use TLS (Remote Access):**
   _Never expose the Docker API on port 2375 (unencrypted)._ Always use port 2376 with TLS certificates.
2. **Beware the Socket Mount:**
   _Avoid running `docker run -v /var/run/docker.sock:/var/run/docker.sock ...`._ If you must do it (e.g., for monitoring tools), ensure the image is highly trusted and runs as a non-root user.
3. **Rootless Mode:**
   _The ultimate security move._ Running the Docker Daemon as a normal user instead of `root`. This ensures that even a Daemon compromise doesn't lead to a Host compromise.

## 🧠 **Real-World Case Study: "The Jenkins Breach"**

- **Scenario:** A company ran a Jenkins CI server inside a Docker container.
- **The Configuration:** To allow Jenkins to build images, they mounted the Docker socket into the Jenkins container.
- **The Incident:** An attacker gained access to a Jenkins job through a weak plugin password.
- **The Escalation:** Using the mounted socket, the attacker created a new container with the `--privileged` flag and gained root access to the entire AWS EC2 instance.
- **The Lesson:** The Docker socket is a "Skeleton Key" to your host. Mounting it is like leaving your physical front door key under a "Welcome" mat that anyone can find.

## 🤔 **Reflective Questions**

1. Why does the Docker Daemon need root privileges by default? (Answer: To manage namespaces, cgroups, and network bridges).
2. What is the difference between "Rootless Docker" and "Running a container as non-root"?
3. How do you check if your Docker socket is correctly protected?

## 📚 **Further Reading**

- **Docker Documentation:** [Protect the Docker daemon socket](https://docs.docker.com/engine/security/https/)
- **Guide:** [Run the Docker daemon as a non-root user (Rootless mode)](https://docs.docker.com/engine/security/rootless/)

## 📝 **Mini Task (Production)**

1. Run `ls -l /var/run/docker.sock` on your Linux machine (or inside a terminal).
2. Notice who owns the file and which group has access.
3. If you are on Windows/Mac (Docker Desktop), research how the "Socket" is abstracted and why it is slightly safer but still risky.
4. Try to find one tool (like `Portainer` or `Traefik`) that requests access to the Docker socket.
5. Search for a "Security Best Practice" guide for that specific tool regarding the socket mount.
