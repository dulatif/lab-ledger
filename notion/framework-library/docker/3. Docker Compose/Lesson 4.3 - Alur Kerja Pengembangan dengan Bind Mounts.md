# 💡 DK03.4.3: Alur Kerja Pengembangan dengan Bind Mounts

**Outline:**

- **The Integration (SEEI):** Using Compose to automate the "Hot Reloading" workflow across multiple services.
- **The Sync (PPP):** Mapping your local source code to a container in the `yml` file.
- **Your Mission (Production):** Creating a real-time development stack with Compose.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Integration (SEEI)**

**Why do this in Compose?**
In Module 2, we learned how to use `-v $(pwd):/app` in the CLI. While effective, doing this for 5 different services manually is tedious. Docker Compose allows you to bake this development workflow into your project configuration.

**The Workflow:**

1. You work on your local files.
2. Compose maps those files into the containers.
3. Your app (running in the container) sees the change and restarts.
   _Result: You get the production-like environment with the speed of local development._

### **Part 2: Practice - The Sync (PPP)**

**The YAML configuration:**

```yaml
services:
  api:
    image: node:18-alpine
    volumes:
      - .:/app # Mount the CURRENT folder to /app
      - /app/node_modules # (Pro Tip) Prevent host node_modules from conflicting
    working_dir: /app
    command: npm run dev
```

**Understanding the `node_modules` trick:**
When you mount `.:/app`, it overwrites everything in `/app`. To prevent your laptop's `node_modules` (which might be for wrong OS/architecture) from breaking the container, we add an **Anonymous Volume** specifically for that folder. Docker will prioritize the container-internal `node_modules`.

## 🧠 **Real-World Case Study: "The Multi-Service Refresh"**

- **Scenario:** You are working on a React Frontend and a Node.js API at the same time.
- **The Challenge:** You change a style in CSS and a validation rule in the API. You want to see both changes instantly.
- **The Solution:** Both services are in one `docker-compose.yml`, both have bind mounts to their respective subfolders (`frontend/` and `api/`).
- **The Result:** You save both files in VS Code. Both containers reload in parallel. Your browser updates immediately.
- **The Lesson:** Compose is the "glue" that makes complex local development feel simple.

## 🤔 **Reflective Questions**

1. Why do we use relative paths like `.` or `./api` in Compose volumes? (Answer: Because they are relative to the location of the `yml` file).
2. What is the danger of using Bind Mounts if your host machine has a different OS (like Windows) than the container (Linux)?
3. How can you tell if an app is currently using a bind mount? (Hint: `docker compose inspect`).

## 📚 **Further Reading**

- **Docker Orientation:** [Compose for local development](https://docs.docker.com/compose/gettingstarted/#step-6-edit-the-compose-file-to-add-a-bind-mount)
- **Guide:** [Best practices for Bind Mounts](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#use-bind-mounts)

## 📝 **Mini Task (Production)**

1. Write a service definition for a service named `php-app`.
2. Requirement:
   - Use the `php:8.1-apache` image.
   - Bind mount your current folder to `/var/www/html`.
   - Map port 8082 on your host to port 80 in the container.
3. If you create an `index.php` file in your folder, will it be visible at `localhost:8082`?
4. Explain why you don't need to rebuild the image every time you edit `index.php`.
