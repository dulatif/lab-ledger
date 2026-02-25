# 💡 DK06.4.3: SSH dari GitHub Actions

**Outline:**

- **The Tunnel (SEEI):** Understanding how to securely connect the GitHub runner to your private server.
- **The Configuration (PPP):** Setting up SSH agents and known hosts without compromising your server.
- **Your Mission (Production):** Successfully running a simple `uptime` command on your server from GitHub.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Tunnel (SEEI)**

**The "How" of Deployment**
To deploy, the GitHub Runner needs to send a "Command" to your server. The global standard for this is **SSH (Secure Shell)**.

**The Security Challenge:**
You need to give GitHub your **Private SSH Key**. If you do this incorrectly, any hacker who compromises your GitHub account (or a public repo) can walk into your server.

**The Solution: Secure SSH Secrets**

1. Generate a **Dedicated** SSH key for GitHub (don't use your personal one!).
2. Add the **Public Key** to the server's `~/.ssh/authorized_keys`.
3. Add the **Private Key** to your GitHub repository as a Secret.

### **Part 2: Practice - The Configuration (PPP)**

**The "SSH-Action" Pattern:**
It's easier and safer to use a marketplace action like `appleboy/ssh-action`.

```yaml
- name: 🚀 SSH into Production
  uses: appleboy/ssh-action@master
  with:
    host: ${{ secrets.SERVER_IP }}
    username: ${{ secrets.SERVER_USER }}
    key: ${{ secrets.SSH_PRIVATE_KEY }}
    script: |
      # These commands run ON THE SERVER, not on the runner!
      cd /apps/my-project
      docker-compose pull
      docker-compose up -d
```

**Key Parameters:**

- `host`: Your server's IP address.
- `username`: The Linux user (e.g., `deploy` or `root`).
- `key`: The multi-line content of your private SSH key.
- `script`: The list of commands to run once the tunnel is open.

## 🧠 **Real-World Case Study: "The Password-Prompt Failure"**

- **Scenario:** A developer tried to run a raw `ssh` command: `run: ssh user@ip "deploy.sh"`.
- **The Problem:** The build hung forever and eventually timed out after 6 hours.
- **The Reason:** Because it was the first time connecting, the server asked: "Are you sure you want to connect? (yes/no)". The GitHub Action had no human to type "yes", so it just sat there waiting.
- **The Fix:** They switched to an Action that handles "Known Hosts" automatically, or added `-o StrictHostKeyChecking=no`.
- **The Lesson:** Automation must be **Non-Interactive**. A single "Are you sure?" prompt will kill your pipeline.

## 🤔 **Reflective Questions**

1. Why should you create a specific "Deploy" user on your server instead of using `root`?
2. What happens if your server's IP address changes? (Answer: The build will fail until you update the Secret).
3. Is your SSH key visible in the GitHub Action logs?

## 📚 **Further Reading**

- **GitHub Marketplace:** [SSH Remote Commands Action](https://github.com/marketplace/actions/ssh-remote-commands)
- **Guide:** [Securely managing SSH keys for automation](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/about-security-hardening-with-openid-connect)

## 📝 **Mini Task (Production)**

1. Generate a new SSH key locally: `ssh-keygen -t rsa -b 4096 -f github_deploy_key`.
2. Look at the contents of the files. Which one stays on your machine and which one goes to GitHub?
3. Imagine you are the server. Create a file `~/.ssh/authorized_keys` and paste the public key.
4. Now, imagine you are the GitHub repository. Add the private key to your Secrets.
5. Draft a workflow step that runs `df -h` (Disk space check) on your "virtual" server.
6. Reflect: How much more secure is this than keeping a terminal window open 24/7?
