# 💡 DK06.3.2: Login ke Registry dari Workflow

**Outline:**

- **The Key (SEEI):** Understanding how to securely pass credentials from GitHub to a Docker registry.
- **The Secret (PPP):** Using GitHub Secrets and the `docker/login-action`.
- **Your Mission (Production):** Successfully authenticating your runner with Docker Hub or GHCR.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Key (SEEI)**

**"Access Denied"**
To push an image to a private registry, your GitHub Action runner must "Log in" as you. But you **must never** hardcode your password in the YAML file.

**The Solution: GitHub Secrets**
You store your username and password (or Personal Access Token) in a secure "Vault" inside the GitHub repository settings. These values are encrypted and never shown in the logs.

**Why use a Token instead of a Password?**

- You can revoke a token without changing your main password.
- You can give the token "Read/Write" access only to the registry, not your whole account.

### **Part 2: Practice - The Secret (PPP)**

**The "Login" Workflow Step:**

1. **On GitHub:** Go to Settings -> Secrets and variables -> Actions.
2. **Add Secrets:** `DOCKER_USERNAME` and `DOCKER_PASSWORD`.
3. **In your YAML:**

```yaml
- name: 🔑 Login to Docker Hub
  uses: docker/login-action@v2
  with:
    username: ${{ secrets.DOCKER_USERNAME }}
    password: ${{ secrets.DOCKER_PASSWORD }}
```

**Logging into GHCR (GitHub):**
If you use GitHub's own registry, you don't even need to create a secret! GitHub provides a temporary token automatically:

```yaml
- name: 🔑 Login to GHCR
  uses: docker/login-action@v2
  with:
    registry: ghcr.io
    username: ${{ github.actor }}
    password: ${{ secrets.GITHUB_TOKEN }}
```

## 🧠 **Real-World Case Study: "The Compromised Password"**

- **Scenario:** A developer accidentally printed their Docker password in a shell script during a build: `echo $PASSWORD`.
- **The Protection:** Because they used **GitHub Secrets**, GitHub Actions automatically detected the secret string in the output and replaced it with `***`.
- **The Result:** Even though the developer made a mistake, the password was never exposed in the public logs.
- **The Lesson:** Use the built-in Secrets system. It's smarter than you think.

## 🤔 **Reflective Questions**

1. What happens if you try to `docker push` without the login step?
2. Why is `${{ github.actor }}` useful for the username? (Answer: It automatically uses the username of the person who pushed the code).
3. Can a secret be shared across multiple repositories in an organization?

## 📚 **Further Reading**

- **GitHub:** [Encrypted secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets)
- **Docker:** [Login action documentation](https://github.com/docker/login-action)

## 📝 **Mini Task (Production)**

1. Generate a **Personal Access Token (PAT)** on Docker Hub or GitHub.
2. Add it as a Secret to one of your repositories.
3. Add a "Login" step to your Docker workflow.
4. Run the workflow and check the logs.
5. Does it say "Login Succeeded"?
6. Look at the logs for the Login step. Can you see your password?
7. Reflect: How much more "Professional" does this feel than manually typing `docker login` in your terminal?
