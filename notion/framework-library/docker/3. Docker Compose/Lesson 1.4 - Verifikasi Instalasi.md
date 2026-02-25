# 💡 DK03.1.4: Verifikasi Instalasi

**Outline:**

- **The CLI (SEEI):** Introducing the `docker compose` command and checking its version.
- **The V1 vs V2 transition (PPP):** Understanding the difference between `docker-compose` (old) and `docker compose` (new).
- **Your Mission (Production):** Confirming your environment is ready for orchestration.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The CLI (SEEI)**

**Is it installed?**
If you have **Docker Desktop** installed (on Windows or macOS), Docker Compose is already included. You don't need to do anything extra. On Linux, you might need to install the plugin separately.

**The Test Command:**

```bash
docker compose version
```

### **Part 2: Practice - The V1 vs V2 transition (PPP)**

**`docker-compose` (Hyphen) vs `docker compose` (Space)**

1. **V1 (Legacy):** Written in Python. Invoked as `docker-compose`.
2. **V2 (Modern):** Written in Go. It is a plugin for the main Docker CLI. Invoked as `docker compose`.

**Which one should I use?**
Always use the **V2** version (`docker compose`). It is faster, better integrated, and is the current industry standard. Docker Desktop now aliases the old hyphenated command to the new one automatically.

**Verification Checklist:**

- [ ] `docker compose version` returns successfully.
- [ ] `docker compose help` shows the list of available subcommands.

## 🧠 **Real-World Case Study: "The Missing Plugin"**

- **Scenario:** A developer moves their project to a fresh Ubuntu server. They install Docker but `docker compose up` returns "command not found."
- **The Diagnosis:** They installed the "Docker Engine" but forgot the "Docker Compose Plugin."
- **The Solution:** They run `sudo apt-get install docker-compose-plugin`.
- **The Lesson:** On Linux, Docker Engine and Docker Compose are often packaged separately. Don't assume having one means you have the other.

## 🤔 **Reflective Questions**

1. How can you tell if you are running V1 or V2 of Docker Compose?
2. In Docker Desktop, why do both `docker-compose` and `docker compose` usually work?
3. Where can you find the official installation guide for Linux if it's missing?

## 📚 **Further Reading**

- **Docker Documentation:** [Install Docker Compose](https://docs.docker.com/compose/install/)
- **Release Notes:** [What's new in Docker Compose V2](https://docs.docker.com/compose/cli-command/#compose-v2-and-the-new-docker-compose-command)

## 📝 **Mini Task (Production)**

1. Open your terminal.
2. Run `docker compose version` and note down the version number.
3. Run `docker-compose version` (with the hyphen). Is the output the same?
4. Run `docker compose help` and find the command used to "Create and start containers." (Hint: It's `up`).
5. Run `docker compose --help` and find the global option to use a specific file name instead of the default `docker-compose.yml`.
