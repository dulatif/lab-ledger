# 💡 DK04.3.3: Rahasia Saat Run-Time

**Outline:**

- **The Memory Leak (SEEI):** Understanding why "Standard" environment variables are still slightly insecure for production secrets.
- **The Docker Secret (PPP):** Using Docker Swarm Secrets to mount sensitive data as temporary files.
- **Your Mission (Production):** Transitioning a database password from an `ENV` variable to a mounted Secret file.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Memory Leak (SEEI)**

**Why `docker run -e` isn't enough**
Injection via environment variables is better than hardcoding, but it has one flaw: The secret is stored in the process memory and is visible to anyone who can run `docker inspect` or `docker compose ps` on your machine.

**The "Memory Dump" Risk:**
If your application crashes and creates a "Core Dump," your secrets might be written to disk in plain text. Also, child processes of your application might automatically inherit all environment variables, spreading the secret where it's not needed.

**The Solution: File-based Secrets**
Instead of a variable, the secret is mounted as a file in a special location (usually `/run/secrets/`). The app reads the file, uses the contents, and doesn't store the string in the global environment list.

### **Part 2: Practice - The Docker Secret (PPP)**

**Docker Secrets (Swarm/Compose):**
Docker "Secrets" are encrypted at rest and delivered only to the specific containers that need them.

**The YAML Syntax (Compose):**

```yaml
services:
  app:
    image: my-app
    secrets:
      - db_password

secrets:
  db_password:
    file: ./db_password.txt
```

**Inside the Container:**
The secret becomes a file at `/run/secrets/db_password`.
Your code should be updated to look for secrets in files:

```python
# Instead of: password = os.environ.get('DB_PASS')
with open('/run/secrets/db_password') as f:
    password = f.read().strip()
```

## 🧠 **Real-World Case Study: "The Accidental Log Leak"**

- **Scenario:** A developer was debugging an application and enabled "Full Debug Logs."
- **The Disaster:** The logging library automatically logged every environment variable to the central ELK server to "Help with context."
- **The Result:** The production API key was now stored in clear text in the company's searchable logs, accessible to every employee.
- **The Fix:** They moved the API key to a Docker Secret.
- **The Success:** Because secrets are files (not variables), the logger didn't automatically pick it up. The logs stayed clean and the key stayed private.
- **The Lesson:** Hide your secrets from your own logs. Use files, not variables.

## 🤔 **Reflective Questions**

1. If you run `env` inside a container using Docker Secrets, will you see the secret? (Answer: No, it's a file, not a variable).
2. Why is Docker Swarm considered more secure for secrets than standard Docker Engine? (Answer: Swarm encrypts secrets in transit and at rest).
3. Can a secret be shared between two different projects?

## 📚 **Further Reading**

- **Docker Documentation:** [Manage sensitive data with Docker Secrets](https://docs.docker.com/engine/swarm/secrets/)
- **Guide:** [Using secrets in Docker Compose](https://docs.docker.com/compose/use-secrets/)

## 📝 **Mini Task (Production)**

1. Open a `docker-compose.yml` file.
2. Define a secret named `app_api_key` pointing to a local text file.
3. Grant access to this secret to one of your services.
4. Run `docker compose up -d`.
5. Run `docker compose exec [service] cat /run/secrets/app_api_key`.
6. Reflect: How would you modify your application logic to check if a secret file exists before falling back to a default variable?
