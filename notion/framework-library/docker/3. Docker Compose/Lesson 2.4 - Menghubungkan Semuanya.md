# 💡 DK03.2.4: Menghubungkan Semuanya

**Outline:**

- **The Integration (SEEI):** Combining services, networks, and volumes into a single source of truth.
- **The Project Name (PPP):** Understanding how Compose groups resources under a project namespace.
- **Your Mission (Production):** Deploying a full stack with a single command.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Integration (SEEI)**

**The Final Piece of the Puzzle**
Now that we know how to define services, networks, and volumes, we put them all together. A complete `docker-compose.yml` is essentially the "Architecture Diagram" of your application translated into text.

**The Magic of `docker compose up`:**

1. It validates the YAML syntax.
2. It pulls any missing images.
3. It creates the infrastructure (Networks/Volumes).
4. It starts the containers in a logical order.
5. It starts streaming the combined logs of all services to your terminal.

### **Part 2: Practice - The Project Name (PPP)**

**Namespacing for Isolation:**
Docker Compose automatically prefixes your resources with the name of the folder containing the YAML file.

- If your folder is named `awesome-project`, your network becomes `awesome-project_default`.
- This allows you to run the same app multiple times on the same host (in different folders) without name conflicts.

**The Override:**
You can set a custom project name using the `-p` flag or the `COMPOSE_PROJECT_NAME` environment variable.

```bash
docker compose -p my-custom-name up
```

## 🧠 **Real-World Case Study: "The Multi-Tenant Deployment"**

- **Scenario:** A freelancer manages 3 different clients who all use the same software stack (WordPress + MySQL).
- **The Problem:** Running 3 instances of WordPress on one server usually leads to port and database name conflicts.
- **The Solution:** They put each client's code and `docker-compose.yml` in a separate folder (`client1/`, `client2/`, `client3/`).
- **The Result:** Even though the services in the YAML are all named `wordpress` and `db`, Compose keeps them separate using project prefixes.
- **The Lesson:** Compose projects are independent silos. Use directory structure to organize your different environments.

## 🤔 **Reflective Questions**

1. If you run `docker ps`, can you see which project a container belongs to? (Hint: Look at the labels).
2. What happens if you run `docker run` manually on a container that Compose created?
3. How do you find the combined logs of all services in a project?

## 📚 **Further Reading**

- **Docker Reference:** [Compose command-line interface](https://docs.docker.com/compose/reference/)
- **Guide:** [Organize your Compose projects](https://docs.docker.com/compose/project-name/)

## 📝 **Mini Task (Production)**

1. Create a folder named `my-web-stack`.
2. Inside, create a `docker-compose.yml` with a single Nginx service.
3. Run `docker compose up -d`.
4. Now, run `docker network ls`. Can you find the network that was automatically created? What is its name?
5. Stop it with `docker compose down`.
6. Run `docker network ls` again. What happened to the network? Why?
