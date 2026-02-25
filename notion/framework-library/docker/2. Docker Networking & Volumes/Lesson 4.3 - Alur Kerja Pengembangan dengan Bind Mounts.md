# 💡 DK02.4.3: Alur Kerja Pengembangan dengan Bind Mounts

**Outline:**

- **The Workflow (SEEI):** Using Bind Mounts to eliminate the "Build-Run-Rebuild" loop during development.
- **The Hot-Reloading (PPP):** Configuring your app to watch for changes through the container's volume.
- **Your Mission (Production):** Setting up a real-time development environment for a Node.js app.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Workflow (SEEI)**

**The "Old" Slow way:**

1. Edit code.
2. `docker build -t my-app .` (Takes 30+ seconds).
3. `docker run ...`
4. Test.
   _Total time per change: 1 minute._

**The "Fast" Bind Mount way:**

1. Edit code.
2. The change is instantly reflected inside the running container.
3. The app auto-restarts (via `nodemon` or similar).
4. Test.
   _Total time per change: 2 seconds._

### **Part 2: Practice - The Hot-Reloading (PPP)**

**The Recipe for Success:**

1. **Host-Side:** You have your source code in a folder.
2. **Container-Side:** You run the container and mount your host folder to the image's app directory.
3. **Execution:** You override the image's default command to use a "Watch" tool.

**The Command:**

```bash
docker run -it \
  -v $(pwd):/usr/src/app \
  -w /usr/src/app \
  node:18-alpine \
  sh -c "npm install && npx nodemon index.js"
```

_Note: `-w` sets the working directory, and we run `nodemon` to watch for file changes._

## 🧠 **Real-World Case Study: "The Compiled Language Challenge"**

- **Scenario:** Developing a TypeScript or Go application that needs to be compiled.
- **The Challenge:** How do you use bind mounts if the container runs a binary, not the source?
- **The Solution:** Use a **Multi-Stage Build** (advanced topic) or run a compiler in "watch mode" on your host, mounting the _output_ directory (`dist` or `bin`) to the container.
- **The Result:** You save code -> Host compiler builds it -> Container sees the new binary through the bind mount -> App restarts.

## 🤔 **Reflective Questions**

1. Why shouldn't you use Bind Mounts for production source code? (Hint: Security and predictability).
2. What is the role of `$(pwd)` in the bind mount syntax?
3. If I delete a file on my laptop, what happens to that file inside the container?

## 📚 **Further Reading**

- **Docker Blog:** [How to use Bind Mounts for development](https://www.docker.com/blog/how-to-use-docker-to-develop-apps/)
- **Node.js Guide:** [Develop Node.js apps in containers](https://docs.docker.com/language/nodejs/develop/)

## 📝 **Mini Task (Production)**

1. Take the Node.js project you created in Chapter 1.
2. Run it using a bind mount:
   ```bash
   docker run -it -p 3000:3000 -v $(pwd):/app -w /app node:18-alpine node index.js
   ```
3. Visit `localhost:3000`.
4. Without stopping the container, change the message in your `index.js` file on your host machine.
5. Notice that the command line doesn't update (since we didn't use nodemon).
6. Press `Ctrl+C` and run the command again.
7. Reflection: How would adding `nodemon` improve this specific workflow?
