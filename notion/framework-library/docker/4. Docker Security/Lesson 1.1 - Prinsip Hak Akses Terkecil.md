# 💡 DK04.1.1: Prinsip Hak Akses Terkecil

**Outline:**

- **The Risk (SEEI):** Understanding the dangers of running containers as the `root` user.
- **The Best Practice (PPP):** Creating and switching to a non-privileged user in your Dockerfile.
- **Your Mission (Production):** Auditing a container's user ID and implementing a security fix.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Risk (SEEI)**

**Root by Default**
By default, Docker containers run as the `root` user inside the container. This means that if an attacker manages to break out of your application (e.g., through a web exploit), they already have administrative privileges inside the container.

**The "Escalation" Threat**
While a root user in a container is isolated by namespaces, certain vulnerabilities or misconfigurations (like mounting the Docker socket) can allow a container-root to become a host-root. This is the "Holy Grail" for hackers.

**The Negative Impact:**

- **Full Control:** An attacker can read any file, install any malware, and delete any data inside the container.
- **Lateral Movement:** It's much easier to attack other containers or the host if you are starting from a root shell.

### **Part 2: Practice - The Best Practice (PPP)**

**The "USER" Instruction:**
The best way to secure your image is to create a specific user with limited permissions and use the `USER` instruction in your Dockerfile.

**Example Implementation (Node.js):**

```dockerfile
FROM node:18-alpine

# 1. Create a work directory
WORKDIR /app

# 2. Add a non-root user
# Note: Many official images already have a 'node' or 'app' user.
# RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# 3. Copy files and set ownership
COPY --chown=node:node . .

# 4. Switch to the non-root user
USER node

# 5. Start the app
CMD ["node", "index.js"]
```

**Verifying the User:**

```bash
docker exec <container_name> whoami
# Should return 'node' or 'appuser', NOT 'root'.
```

## 🧠 **Real-World Case Study: "The Compromised Admin"**

- **Scenario:** A popular image processing service had a bug that allowed attackers to run shell commands via an uploaded filename.
- **The Incident:** The image was running as `root`. The attacker used the bug to `apt-get install` a cryptominer and scanned the company's internal network.
- **The Remediation:** The developers updated the Dockerfile to use a `USER app` instruction.
- **The Result:** When the same exploit was attempted later, the attacker couldn't install software or access sensitive system files because they lacked permissions.
- **The Lesson:** "Root" is a privilege, not a requirement. Defensive coding starts with the user ID.

## 🤔 **Reflective Questions**

1. Why does Docker even allow containers to run as `root` by default?
2. If I switch to a non-root user mid-Dockerfile, can I switch back to root for a specific command? (Answer: Yes, by using `USER root` again, but you should switch back to the limited user before the end).
3. Does a non-root user need `sudo` inside a container? (Answer: Usually no, and you shouldn't install it!).

## 📚 **Further Reading**

- **Docker Security:** [Rootless mode](https://docs.docker.com/engine/security/rootless/)
- **Best Practices:** [Dockerfile security best practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#user)

## 📝 **Mini Task (Production)**

1. Start a simple container: `docker run -d --name security-audit alpine sleep 1000`.
2. Check the user: `docker exec security-audit whoami`. It will be `root`.
3. Now, try to run a container as a specific numeric ID:
   ```bash
   docker run -d --name low-priv --user 1001 alpine sleep 1000
   ```
4. Check the user again: `docker exec low-priv whoami`.
5. What happened? Why is using a numeric ID like `1001` safer than just name strings sometimes? (Hint: Numeric IDs don't need to exist in `/etc/passwd` to work).
