# 💡 DK04.4: Menjaga Konteks Build Tetap Ramping (.dockerignore)

**Outline:**

- **The Problem (SEEI):** Large build contexts leading to slow builds and security risks.
- **The Solution (PPP):** Using the `.dockerignore` file to exclude unnecessary data.
- **Your Mission (Production):** Optimizing your project's build context.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Problem (SEEI)**

**The Build Context**
When you run `docker build .`, the Docker client sends **all files** in the current directory to the Docker daemon. If your project has a massive `node_modules` folder, huge media files, or `.git` history, this step can take minutes and waste gigabytes of bandwidth.

**Risks of a "Fat" Context:**

1. **Security:** You might accidentally copy private keys, `.env` files, or SSH configs into your public image.
2. **Speed:** Larger contexts mean longer wait times before the first step even runs.
3. **Cache Invalidation:** If you change a generic file (like a log), it might invalidate layers and force a full rebuild.

### **Part 2: Practice - The Solution (PPP)**

**The `.dockerignore` File**
This file works exactly like `.gitignore`. It tells Docker which files and folders to **ignore** when sending variables to the daemon.

**Standard Node.js Example:**
Create a file named `.dockerignore` in your project root:

```
# Dependency folders
node_modules
jspm_packages

# Debug logs
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# Git and OS files
.git
.gitignore
.DS_Store

# Sensitive info
.env
.aws
```

## 🧠 **Real-World Case Study: "The Secret Leak"**

- **Scenario:** A developer builds an image and pushes it to Docker Hub. They forgot to exclude their `.env` file containing the production database password.
- **The Consequence:** Anyone who pulls the image can use `docker history` or `docker exec` to find that sensitive file in the image layers.
- **The Fix:** Adding `.env` to `.dockerignore` ensures it never even reaches the Docker daemon during the build process.
- **The Lesson:** `.dockerignore` is not just for performance—it's a critical security boundary.

## 🤔 **Reflective Questions**

1. If a file is in `.dockerignore`, can the `COPY` instruction in the Dockerfile see it? (Answer: No, because it was never sent to the builder).
2. What is the most common folder to ignore in a Node.js project?
3. How does `.dockerignore` help with layer caching?

## 📚 **Further Reading**

- **Docker orientation:** [The .dockerignore file](https://docs.docker.com/engine/reference/builder/#dockerignore-file)
- **Best Practices:** [Building small, secure images](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)

## 📝 **Mini Task (Production)**

1. Check the size of your current directory (including `node_modules`).
2. Run `docker build -t test-size .` and look at the first line of output: `Sending build context to Docker daemon...` How many MB/GB is it?
3. Create a `.dockerignore` file and add `node_modules`.
4. Run the build again. Notice the massive reduction in the context size.
5. List at least 3 other file types or folders in your current OS that should be in a global `.dockerignore`.
