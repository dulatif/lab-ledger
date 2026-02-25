# 💡 DK03.4: Menulis Dockerfile untuk Node.js

**Outline:**

- **The Standard Stack (SEEI):** Why Node.js is the perfect fit for containerization.
- **The Professional Template (PPP):** A step-by-step walkthrough of a production-ready Node.js Dockerfile.
- **Your Mission (Production):** "Dockerizing" your first full application.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Standard Stack (SEEI)**

**Why Docker + Node.js?**
Node.js applications often suffer from "dependency hell" (`node_modules`). One machine has `npm 8`, another has `npm 10`, leading to subtle bugs. Docker locks the Node.js version and the exact dependency tree (`package-lock.json`) into the image.

**Key considerations:**

- **Base Image:** Use `node:<version>-alpine` for the smallest, most secure images.
- **Environment:** Distinguish between `development` and `production` dependencies.
- **User Permissions:** Don't run your app as `root` (security risk).

### **Part 2: Practice - The Professional Template (PPP)**

**The "Perfect" Node.js Dockerfile:**

```dockerfile
# 1. Use a lightweight base image
FROM node:18-alpine

# 2. Set the working directory
WORKDIR /usr/src/app

# 3. Copy dependency definitions first (for caching)
COPY package*.json ./

# 4. Install dependencies
RUN npm install

# 5. Copy the actual source code
COPY . .

# 6. Expose the port your app runs on
EXPOSE 3000

# 7. Use the array syntax for the start command
CMD ["npm", "start"]
```

**Pro Tip: `.dockerignore`**
You should never copy `node_modules` from your local machine into the image. Create a `.dockerignore` file:

```
node_modules
npm-debug.log
.git
```

## 🧠 **Real-World Case Study: "The Deployment Disaster"**

- **Scenario:** An app works locally on macOS but fails on a Linux server because one of its native dependencies (like `bcrypt` or `sharp`) was compiled for Mac.
- **The Solution:** The team builds the Docker image _on_ the CI/CD server (which is Linux).
- **The Result:** The native dependencies are compiled inside the Linux container, for the Linux environment. Deployment is now 100% reliable.
- **The Lesson:** Build _inside_ the container, don't just copy pre-built binaries from your local machine.

## 🤔 **Reflective Questions**

1. Why do we COPY `package*.json` separately before `COPY . .`?
2. Why is it better to use a specific version (e.g., `18-alpine`) instead of just `node:latest`?
3. What is the harm in running a Node.js container as the `root` user?

## 📚 **Further Reading**

- **Docker Guide:** [Node.js Web App](https://nodejs.org/en/docs/guides/nodejs-docker-webapp/)
- **Blog:** [Dockerizing a Node.js web app](https://docs.docker.com/language/nodejs/containerize/)

## 📝 **Mini Task (Production)**

1. Initialize a simple Node.js project:
   ```bash
   npm init -y
   npm install express
   ```
2. Create a basic `index.js` that listens on port 3000.
3. Use the "Perfect Template" above to create a `Dockerfile`.
4. Create a `.dockerignore` file and add `node_modules` to it.
5. Build and run your container:
   ```bash
   docker build -t node-hello .
   docker run -p 3000:3000 node-hello
   ```
6. Visit `localhost:3000` to confirm it works!
