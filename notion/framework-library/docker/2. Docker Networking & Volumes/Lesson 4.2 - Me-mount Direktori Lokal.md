# 💡 DK02.4.2: Me-mount Direktori Lokal

**Outline:**

- **The Mapping (SEEI):** Using Bind Mounts to connect your local development environment to a container.
- **The Path rules (PPP):** Understanding absolute paths and working with different Operating Systems.
- **Your Mission (Production):** Creating a "Shared Window" between your host and a container.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Mapping (SEEI)**

**Direct Host Access**
A Bind Mount creates a bi-directional link. Changes made in the container are seen on the host, and changes made on the host are seen in the container.

**The Main Limitation:**
Unlike Volumes, which are managed by Docker, Bind Mounts depend on the host machine having a specific directory structure. This makes images less "portable" if the paths are hardcoded.

### **Part 2: Practice - The Path rules (PPP)**

**Working with Paths:**

1. **Absolute Paths are Mandatory:** You cannot use relative paths like `../my-code`. You must use the full path.
2. **Shortcuts:**
   - **Linux/Mac/PowerShell:** `$(pwd)`
   - **Command Prompt:** `%cd%`

**The Command:**

```bash
docker run -d -v /absolute/path/to/my/project:/var/www/html nginx
```

**Common Pitfall: Empty Folders**
If you bind mount an empty directory on your host over a non-empty directory in the container (e.g., `/etc`), the files in the container will be "hidden" by the empty host folder. Use with caution!

## 🧠 **Real-World Case Study: "The Permission Headache"**

- **Scenario:** A Linux developer mounts a project folder: `docker run -v $(pwd):/app ...`.
- **The Issue:** The container (running as `root`) creates a file in `/app`.
- **The Problem:** Back on the host, the developer cannot delete that file because it is owned by the `root` user, not the developer's user.
- **The Solution:** They must explicitly set user IDs (using `--user`) or fix permissions manually (`chown`).
- **The Lesson:** Bind mounts use the host's permission model. Be aware of who "owns" the files being created.

## 🤔 **Reflective Questions**

1. What is the main difference in syntax between a "Named Volume" and a "Bind Mount" in the `-v` flag?
2. What happens if you delete a folder on the host that is being used by a running container's bind mount?
3. Why do we say Bind Mounts are "High Performance"?

## 📚 **Further Reading**

- **Docker Reference:** [Use bind mounts](https://docs.docker.com/storage/bind-mounts/)
- **Guide:** [Best practices for bind mounts](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#use-bind-mounts)

## 📝 **Mini Task (Production)**

1. Create a file named `hello.html` on your desktop with some text.
2. Run Nginx with a bind mount to that specific file:
   ```bash
   # Replace /path/to/desktop with your actual path
   docker run -d -p 8081:80 -v /path/to/desktop/hello.html:/usr/share/nginx/html/index.html nginx
   ```
3. Visit `localhost:8081`. You should see your custom HTML.
4. Edit the file on your desktop and save it.
5. Refresh the browser. Does it update instantly?
6. This feature is the foundation of "Hot Reloading" in development.
