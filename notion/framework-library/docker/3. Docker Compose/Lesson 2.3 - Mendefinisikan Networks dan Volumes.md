# 💡 DK03.2.3: Mendefinisikan Networks dan Volumes

**Outline:**

- **The Infrastructure (SEEI):** Using top-level `networks` and `volumes` blocks to define shared resources.
- **The Global Scope (PPP):** Understanding how to create resources that multiple services can share.
- **Your Mission (Production):** Designing the plumbing for a database and cache cluster.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Infrastructure (SEEI)**

**Centralized Resources**
In Module 2, we learned how to create networks and volumes one-by-one using the CLI. In Compose, we define them centrally so that any service in the file can reference them by name.

**The Workflow:**

1. You list the resources at the BOTTOM of the file.
2. You reference which ones each service should "join" inside the service definition.

### **Part 2: Practice - The Global Scope (PPP)**

**The YAML Syntax:**

```yaml
services:
  database:
    image: postgres
    volumes:
      - pg-data:/var/lib/postgresql/data # Using the named volume
    networks:
      - backend # Joining the backend network

volumes: # Define named volumes here
  pg-data:

networks: # Define named networks here
  backend:
```

**Customizing Networks:**
You can specify the driver or even use a network that was created outside of Compose:

```yaml
networks:
  external-net:
    external: true # This network already exists; don't try to create it.
```

## 🧠 **Real-World Case Study: "The Micro-segmentation"**

- **Scenario:** You have a Web App, a Public API, and a Database.
- **The Design:**
  - `frontend-net`: Shared by Web App and Public API.
  - `backend-net`: Shared by Public API and Database.
- **The Implementation:** You define both networks at the bottom of the `docker-compose.yml`.
- **The Security Gain:** Even though all services are in the same file, the Database is logically invisible to the Web App because they don't share a network.
- **The Lesson:** Use Compose to enforce "Least Privilege" networking from day one.

## 🤔 **Reflective Questions**

1. What happens if you use a volume in a service but forget to define it in the top-level `volumes` section? (Answer: Compose will throw a configuration error).
2. Why does Compose prefix network and volume names with the project directory name? (e.g., `myapp_pg-data`).
3. When should you use the `external: true` flag?

## 📚 **Further Reading**

- **Docker Orientation:** [Volumes in Compose](https://docs.docker.com/compose/compose-file/07-volumes/)
- **Guide:** [Networks in Compose](https://docs.docker.com/compose/compose-file/06-networks/)

## 📝 **Mini Task (Production)**

1. Write the YAML code to define two services: `redis` and `exporter`.
2. Both services must share a network named `monitoring`.
3. The `redis` service must also use a volume named `redis-snapshots`.
4. Define the top-level `networks` and `volumes` correctly.
5. Verify your indentation: Are `networks` and `volumes` at the same indentation level as `services`?
