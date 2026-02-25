# 💡 DK03.4.1: Konfigurasi dengan Environment Variable

**Outline:**

- **The Variables (SEEI):** Using `.env` files and the `environment` block to make your stack flexible.
- **The Precedence (PPP):** Understanding which values win when there are multiple sources.
- **Your Mission (Production):** Securing sensitive data in a Compose stack.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Variables (SEEI)**

**"Don't Hardcode!"**
Settings like database passwords, API keys, and port numbers change between environments (Dev, Staging, Prod). Hardcoding them in your `yml` is a security risk and a maintenance nightmare.

**The Solution:**
Docker Compose can read variables from several places:

1. **The `.env` file:** A simple file in the same folder as your `yml`.
2. **The `environment` section:** For hardcoding specific app values.
3. **The Shell:** You can pass variables directly from your terminal.

### **Part 2: Practice - The Precedence (PPP)**

**The "Winning" Order (Highest to Lowest):**

1. **Shell Variables:** `DB_PASS=secret docker compose up`. (Manual overrides).
2. **Environment Block:** Values defined directly in the `yml`.
3. **The `.env` File:** The default configuration storage.
4. **Image metadata:** Default values set inside the Dockerfile.

**Example Implementation:**

```yaml
# .env file
PORT=3000
DB_PASS=mypass123

# docker-compose.yml
services:
  web:
    image: node-app
    ports:
      - "${PORT}:3000"
    environment:
      - DATABASE_PASSWORD=${DB_PASS}
```

## 🧠 **Real-World Case Study: "The Accidental GitHub Leak"**

- **Scenario:** A developer includes their real database password in `docker-compose.yml` and pushes it to a public GitHub repo.
- **The Consequence:** Automated bots find the password within minutes and start scanning the server.
- **The Fix:** Move the password to a `.env` file and **add `.env` to `.gitignore`**.
- **The Result:** The `yml` file now only contains placeholders like `${DB_PASS}`, making it safe to share.
- **The Lesson:** Keep your "Logic" (yml) separated from your "Secrets" (env).

## 🤔 **Reflective Questions**

1. Why should you always `.gitignore` your `.env` file?
2. What happens if a variable is defined in the `.env` file but NOT used in the `yml`? (Answer: Nothing, it is just ignored).
3. How do you provide a default value if an environment variable is missing? (Hint: `${VAR:-default}`).

## 📚 **Further Reading**

- **Docker Orientation:** [Environment variables in Compose](https://docs.docker.com/compose/environment-variables/)
- **Guide:** [Best practices for secret management](https://docs.docker.com/compose/use-secrets/)

## 📝 **Mini Task (Production)**

1. Create a `.env` file with `APP_VERSION=1.0.0`.
2. Write a `docker-compose.yml` snippet that uses this variable in the image name: `my-app:${APP_VERSION}`.
3. Run `docker compose config`. This command doesn't start anything; it just shows the final "parsed" YAML.
4. Did the variable resolve correctly?
5. Try running `APP_VERSION=2.0.0 docker compose config`. Which version "won"?
6. Why is `docker compose config` a powerful tool for debugging complex configurations?
