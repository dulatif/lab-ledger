# 💡 DK02.2.4: Studi Kasus Aplikasi dan Database

**Outline:**

- **The Challenge (SEEI):** Connecting a real-world web application to a persistent database using Docker Networking.
- **The Orchestration (PPP):** Step-by-step wiring of a Node.js API and a MongoDB instance.
- **Your Mission (Production):** Deploying a functional multi-tier stack.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Challenge (SEEI)**

In professional development, we rarely run a single container. Most apps consist of at least a Web Server and a Database.

**The Goal:**
The Web App (Application Layer) must be able to securely and reliably communicate with the Database (Data Layer) without needing to know the database's exact IP address.

**The Solution:**

1. Create a dedicated network for secrecy.
2. Put both containers in that network.
3. Use the container name as the connection string.

### **Part 2: Practice - The Orchestration (PPP)**

**Step-by-Step Deployment:**

1. **Create the Network:**
   ```bash
   docker network create app-stack
   ```
2. **Start the Database (MongoDB):**
   ```bash
   docker run -d --name my-db --network app-stack mongo
   ```
3. **Start the App (Node.js):**
   _Note: In the app code, the connection string is `mongodb://my-db:27017/mydb`_
   ```bash
   docker run -d --name my-app --network app-stack -p 3000:3000 my-image
   ```

**How they talk:**
The Node.js app asks for `my-db`. Docker DNS says `my-db` is at `172.x.x.x`. The traffic flows perfectly through the `app-stack` bridge.

## 🧠 **Real-World Case Study: "The Publicly Exposed Database"**

- **The Mistake:** A developer publishes the database port to the internet: `docker run -p 27017:27017 mongo`.
- **The Risk:** Now anyone in the world can try to hack the database directly.
- **The Fix:** Remove the `-p` flag for the database.
- **The Security Logic:** Since the app is in the same Docker network, it can talk to the database on port 27017 _internally_. The database doesn't need to be visible to the public internet at all.
- **The Result:** Maximum security. Only the App is public; the DB is invisible to the outside world.

## 🤔 **Reflective Questions**

1. Why don't we use `-p` for the database container in this stack?
2. What happens if the database container crashes? How does the App reconnect?
3. Could you add a second app to the same `app-stack` network?

## 📚 **Further Reading**

- **Docker Guide:** [Networking in Compose (Preview)](https://docs.docker.com/compose/networking/)
- **Tutorial:** [Connect an app to a database](https://docs.docker.com/get-started/07_multi_container/)

## 📝 **Mini Task (Production)**

1. Create a network `stack-test`.
2. Start a Redis database:
   ```bash
   docker run -d --name my-cache --network stack-test redis
   ```
3. Run an alpine container to "sniff" the connection:
   ```bash
   docker run -it --network stack-test alpine sh
   ```
4. Inside alpine, install `telnet` or `nc` and try to connect to the Redis port:
   ```bash
   apk add --no-cache netcat-openbsd
   nc -zv my-cache 6379
   ```
5. If you see `Connection to my-cache 6379 port [tcp/6379] succeeded!`, your multi-tier communication is working!
