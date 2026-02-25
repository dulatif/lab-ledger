# 💡 DK04.3: Menjalankan Aplikasi Kustom Anda

**Outline:**

- **The Deployment (SEEI):** Moving from a built image to a running, functional container.
- **The Execution Strategy (PPP):** Using the `-d`, `-p`, and `--name` flags for production-like setups.
- **Your Mission (Production):** Running your custom application in the background.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Deployment (SEEI)**

**From Image to App**
Once you have built your custom image (e.g., `my-node-app:v1`), the final step is to run it. In a real-world scenario, you want your application to:

1. Run in the background (**Detached**).
2. Be accessible from the host machine (**Port Mapping**).
3. Have a stable, predictable name (**Naming**).

**The "Production" Command Pattern:**

```bash
docker run -d -p 8080:3000 --name prod-api my-node-app:v1
```

### **Part 2: Practice - The Execution Strategy (PPP)**

**Checking Your Deployment:**

1. **Check Logs:** Since the app is in the background, you can't see the console. Use:
   ```bash
   docker logs -f prod-api
   ```
2. **Check Running Processes:**
   ```bash
   docker top prod-api
   ```
3. **Execute Commands Inside:** To check the file system or run a diagnostic inside:
   ```bash
   docker exec -it prod-api /bin/sh
   ```

## 🧠 **Real-World Case Study: "The Silent Crash"**

- **Scenario:** A dev runs their app with `docker run -d`. Everything looks fine, but the website doesn't load.
- **The Debugging:** They run `docker ps` and see that the container has a status of `Exited (1)`.
- **The Investigation:** They run `docker logs <container-id>` and see a "Database Connection Error."
- **The Lesson:** Always check your logs! Just because a container is created doesn't mean the application inside successfully started. Detached mode (`-d`) hides errors unless you look for them.

## 🤔 **Reflective Questions**

1. What is the difference between `docker run` and `docker exec`? (Answer: `run` starts a NEW container, `exec` runs a command in an ALREADY RUNNING container).
2. If your app listens on port 5000 inside the container, how would you map it to port 80 outside?
3. How can you follow the logs of a container in real-time?

## 📚 **Further Reading**

- **Docker Orientation:** [Run your app in the background](https://docs.docker.com/get-started/02_our_app/#stopping-the-container)
- **CLI Docs:** [docker logs reference](https://docs.docker.com/engine/reference/commandline/logs/)

## 📝 **Mini Task (Production)**

1. Take your `node-hello` image from Chapter 3.
2. Run it in the background with the name `my-running-app` and map port 3000 to port 4000 on your machine.
   ```bash
   docker run -d -p 4000:3000 --name my-running-app node-hello
   ```
3. Verify it's running via `docker ps`.
4. Open your browser at `localhost:4000`.
5. Check the logs: `docker logs my-running-app`.
6. Finally, "peek" into the container's environment:
   ```bash
   docker exec my-running-app env
   ```
