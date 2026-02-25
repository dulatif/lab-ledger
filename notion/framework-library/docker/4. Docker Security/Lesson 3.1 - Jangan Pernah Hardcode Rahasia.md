# 💡 DK04.3.1: Jangan Pernah Hardcode Rahasia

**Outline:**

- **The Disaster (SEEI):** Understanding the catastrophic consequences of embedding credentials in source code or Dockerfiles.
- **The Decoupling (PPP):** Moving from "Hardcoded" strings to "Environment-driven" configuration.
- **Your Mission (Production):** Refactoring a "Dirty" Dockerfile into a "Clean" one.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Disaster (SEEI)**

**"It's just for testing!"**
This is the most dangerous sentence in DevOps. A password hardcoded in a Dockerfile or a Python script often "Leaks" into production.

**The Risk Spectrum:**

- **Source Leak:** Anyone with access to your Git repo (or a public fork) has your keys.
- **Image Metadata Leak:** Even if you delete the source code, `docker inspect` or `docker history` can reveal variables passed during build.
- **Runtime Leak:** If an attacker gets shell access, they can simply type `env` to see all your secrets.

**The Negative Impact:**

- **Financial Loss:** Stolen cloud API keys lead to crypto mining bills.
- **Data Breach:** Stolen database passwords lead to customer data theft.
- **Reputational Ruin:** Explaining a "Hardcoded password" breach to customers is impossible.

### **Part 2: Practice - The Decoupling (PPP)**

**The "Rule of One":**
A Docker image should contain **Zero** secrets. Secrets should be injected ONLY when the container is starting.

**The Transformation:**

_Bad (Hardcoded):_

```dockerfile
ENV DB_PASSWORD="supersafepassword123"
```

_Good (Externalized):_

```dockerfile
# No ENV or ARG for secrets in the Dockerfile!
# The app should read DB_PASSWORD from the environment at runtime.
```

**How to inject properly:**

```bash
docker run -e DB_PASSWORD=$MY_REAL_PASS my-app
```

## 🧠 **Real-World Case Study: "The Accidental Public Repo"**

- **Scenario:** A team moving from private GitLab to public GitHub accidentally made their repository public for 30 minutes.
- **The Incident:** Bots scanned the repo and found a `docker-compose.yml` with a production database password hardcoded.
- **The Result:** The database was wiped and replaced with a "Pay 0.1 BTC to get your data back" note within those 30 minutes.
- **The Remediation:** The team changed every password, rotated every key, and implemented a **Hard-Rule**: "All secrets must come from an un-tracked `.env` file or a Secret Manager."
- **The Lesson:** Assume your images and code will be public one day. Design for it.

## 🤔 **Reflective Questions**

1. Why is an `ENV` variable in a Dockerfile considered "Hardcoded"?
2. If you need a secret to build an image (e.g., to pull a private package), where should you put it?
3. What is the "12-Factor App" philosophy regarding configuration?

## 📚 **Further Reading**

- **12factor.net:** [Config - Store config in the environment](https://12factor.net/config)
- **Docker Blog:** [How to keep secrets out of your Docker images](https://www.docker.com/blog/how-to-keep-secrets-out-of-your-docker-images/)

## 📝 **Mini Task (Production)**

1. Create a fake `.env` file with `SECRET_API_KEY=ABC123XYZ`.
2. Write a simple Python/Node script that prints "Connecting with key: [value]" using an environment variable.
3. Run this script in a container WITHOUT using `ENV` in the Dockerfile.
4. Verify that you can change the key just by changing the `docker run -e` command.
5. Reflect: Why is this image now "Generic" and "Secure"?
