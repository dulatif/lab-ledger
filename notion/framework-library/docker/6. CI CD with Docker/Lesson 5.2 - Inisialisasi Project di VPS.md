# 💡 DK06.5.2: Inisialisasi Project di VPS

**Outline:**

- **The Groundwork (SEEI):** Understanding the "Zero to Hero" steps for setting up a fresh server for Docker.
- **The Automation (PPP):** Scripting the installation of Docker and user permissions.
- **Your Mission (Production):** Transforming a raw Linux server into a production-ready Docker host.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Groundwork (SEEI)**

**"I bought a VPS... now what?"**
Before you can deploy your first container, you need a "Clean Foundation." A raw Ubuntu or Debian server doesn't come with Docker installed.

**The Checklist:**

1. **Security:** Change the SSH port, disable root login, enable Firewall (UFW).
2. **Infrastructure:** Install Docker and Docker Compose.
3. **Environment:** Create the project directories (e.g., `/var/www/my-app`).
4. **Permissions:** Add your deployment user to the `docker` group.

### **Part 2: Practice - The Automation (PPP)**

**The "Scripted" Setup:**
Don't install Docker by hand. Use the official "Convenience Script" provided by Docker for new servers:

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

**Setting up Permissions (Crucial for CI/CD):**
If you don't do this, your GitHub Action will fail with "Permission Denied" when it tries to run Docker commands:

```bash
sudo usermod -aG docker $USER
newgrp docker # Apply changes without logging out
```

**Directory Structure:**
A standard project folder on a VPS usually looks like this:

```text
/app/
├── .env                  # Production secrets (Manually created)
├── docker-compose.yml     # Pulled from Git
└── data/                  # Persistent data (Volumes)
```

## 🧠 **Real-World Case Study: "The Sudo Disaster"**

- **Scenario:** A developer forgot to add the `deploy` user to the `docker` group.
- **The Symptom:** In their GitHub Action, they saw the error: `Got permission denied while trying to connect to the Docker daemon socket`.
- **The "Bad" Fix:** They changed the script to `sudo docker-compose up -d`.
- **The New Problem:** Because `sudo` requires a password, the Action hung forever waiting for input.
- **The "Right" Fix:** They SSH'd in once, ran `sudo usermod -aG docker deploy`, and restarted the server.
- **The Outcome:** The deployment now runs flawlessly without `sudo`, and the security level remains high.
- **The Lesson:** Configure your permissions once, correctly. Never use `sudo` inside an automated CI/CD pipeline.

## 🤔 **Reflective Questions**

1. Why is it better to use a specific user for deployment instead of the `root` user?
2. What is the difference between a "Dangling" volume and a "Persistent" volume?
3. How do you ensure Docker starts automatically if the VPS reboots? (Answer: `sudo systemctl enable docker`).

## 📚 **Further Reading**

- **Docker:** [Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)
- **DigitalOcean:** [Recommended VPS Security](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-22-04)

## 📝 **Mini Task (Production)**

1. If you have a VPS (or a local VM), run `docker version`.
2. Do you need to type `sudo`?
3. If yes, try the `usermod` command to add yourself to the group.
4. Log out and log back in. Try `docker version` again.
5. Create a folder structure in `/var/www/test-app`.
6. Reflect: How much time did it take to set up? Would you want to do this by hand for 10 servers?
