# 💡 DK02.4.4: Pertimbangan Keamanan

**Outline:**

- **The Risk (SEEI):** Understanding the security implications of granting containers access to your host's filesystem.
- **The Best Practices (PPP):** Minimizing the attack surface using Read-Only mounts and unprivileged users.
- **Your Mission (Production):** Auditing a Docker command for security vulnerabilities.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Risk (SEEI)**

**A Hole in the Wall**
A Bind Mount is a direct path through the container's isolation. If a container is compromised (e.g., via a remote code execution vulnerability in your app), the attacker can potentially read and write any file on your host that is included in the mount.

**The Worst-Case Scenario:**
Mounting the host's root directory: `docker run -v /:/host_root ...`. This effectively gives the container (and any attacker inside it) total control over your server.

### **Part 2: Practice - The Best Practices (PPP)**

**The "Principle of Least Privilege":**

1. **Use Read-Only Mounts:** If the container only needs to read data (like config files), add `:ro` to the mount.
   - `docker run -v $(pwd)/config.json:/app/config.json:ro ...`
2. **Limit the Scope:** Never mount more than exactly what the container needs. Don't mount your entire `Desktop` if the app only needs a `data` folder.
3. **Avoid Mounting the Docker Socket:** Mounting `/var/run/docker.sock` allows a container to control the Docker daemon itself—**extremely dangerous!**

**Command to Verify Permissions:**

```bash
docker exec <container> ls -la /mounted-folder
```

_Always check who owns the files and what permissions (`rwx`) are set._

## 🧠 **Real-World Case Study: "The Configuration Leak"**

- **Scenario:** A dev mounts their entire project folder including a `.ssh` folder.
- **The Exploit:** A hacker finds a bug in the web app and uses it to read the `.ssh/id_rsa` private key through the bind mount.
- **The Consequence:** The hacker now has SSH access to the developer's GitHub and other servers.
- **The Fix:** Move sensitive folders out of the project directory or use a strict `.dockerignore` / specific `COPY` in the Dockerfile instead of a blind Bind Mount.

## 🤔 **Reflective Questions**

1. What does the suffix `:ro` do to a mount?
2. Why is mounting `/var/run/docker.sock` considered a "Root-level" security risk?
3. How can using a non-root user inside a container (`USER node`) help mitigate Bind Mount risks?

## 📚 **Further Reading**

- **Docker Security:** [General security patterns](https://docs.docker.com/engine/security/)
- **Guide:** [Docker Volume Security](https://docs.docker.com/storage/volumes/#use-a-volume-driver)

## 📝 **Mini Task (Production)**

1. Start Nginx with a read-only bind mount for a specific file:
   ```bash
   docker run -d --name security-test -v $(pwd)/index.html:/usr/share/nginx/html/index.html:ro nginx
   ```
2. Try to "hack" the container and change the file from inside:
   ```bash
   docker exec security-test sh -c "echo 'hacked' > /usr/share/nginx/html/index.html"
   ```
3. Observe the error: `Read-only file system`.
4. Explain why this error is actually a "success" from a security perspective.
