# 💡 DK03.1.3: Anatomi File docker-compose.yml

**Outline:**

- **The Blueprint (SEEI):** Understanding the structure and essential fields of a `docker-compose.yml` file.
- **The Top-Level Sections (PPP):** Detailed walkthrough of `version`, `services`, `networks`, and `volumes`.
- **Your Mission (Production):** Reading and understanding a production-grade Compose file.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Blueprint (SEEI)**

**YAML: The Language of Infrastructure**
Docker Compose uses **YAML** (Yet Another Markup Language). It is indentation-based and designed to be human-readable.

**The Mandatory Sections:**
A typical file looks like this:

```yaml
version: "3.8" # 1. Version of the Compose specification

services: # 2. THE heart of the file (The containers)
  web:
    image: nginx:alpine
    ports:
      - "8080:80"

networks: # 3. Optional: Custom networks
  frontend:

volumes: # 4. Optional: Persistent storage
  web-data:
```

### **Part 2: Practice - The Top-Level Sections (PPP)**

**Breakdown of a Service definition:**
Inside a service (e.g., `web`), you can define everything you would normally include in a `docker run` command:

- **`image`**: The Docker image to pull and use.
- **`ports`**: Mapping host ports to container ports (`Host:Container`).
- **`environment`**: Setting environment variables (like API keys or DB passwords).
- **`volumes`**: Mounting named volumes or bind mounts.
- **`networks`**: Which networks the service should join.
- **`depends_on`**: Specifying the startup order (e.g., "Don't start the app until the database is ready").

## 🧠 **Real-World Case Study: "The Secret Language"**

- **Scenario:** A developer writes a `docker-compose.yml` file but gets a parsing error.
- **The Culprit:** They used TABS instead of SPACES for indentation.
- **The Fix:** Switching the editor settings to "Insert spaces" instead of "Tabs."
- **The Lesson:** In YAML, indentation is logic. A single extra space can break your entire infrastructure. Always use a YAML validator or a plugin in your IDE.

## 🤔 **Reflective Questions**

1. Why is the `version` field important in a Compose file?
2. What is the difference between a list of strings (using `-`) and a key-value pair in YAML?
3. How many services can you define in a single `docker-compose.yml` file? (Answer: No fixed limit, but keep it manageable!).

## 📚 **Further Reading**

- **Docker Orientation:** [Compose file structure](https://docs.docker.com/compose/compose-file/)
- **Cheat Sheet:** [Full list of Compose service attributes](https://docs.docker.com/compose/compose-file/05-services/)

## 📝 **Mini Task (Production)**

1. Look at this snippet:
   ```yaml
   services:
     app:
       image: node:18
       ports:
         - "3000:3000"
       volumes:
         - .:/app
   ```
2. Convert this YAML back into a single `docker run` command.
   - _Example: `docker run -p 3000:3000 -v $(pwd):/app node:18`_
3. Which section is missing from the YAML that would allow other containers to find `app` by its name?
