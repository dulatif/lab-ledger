# 💡 DK01.4: Menjalankan Kontainer Pertama Anda

**Outline:**

- **The Action (SEEI):** Running the "Hello World" of the Docker world.
- **The Mechanism (PPP):** Understanding the 4 steps Docker takes when you run a container.
- **Your Mission (Production):** Triggering your first successful container lifecycle.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Action (SEEI)**

**The "Hello-World" Image**
The simplest way to test your installation is to run an image specifically designed for testing.

**The Command**

```bash
docker run hello-world
```

**The Expected Result**
You will see a message beginning with:
`Hello from Docker! This message shows that your installation appears to be working correctly.`

### **Part 2: Practice - The Mechanism (PPP)**

**What Happens Behind the Scenes?**
When you run `docker run hello-world`, Docker follows this exact flow:

1. **Local Search:** Docker checks if the `hello-world` image is already on your computer.
2. **Download (Pull):** If not found locally, Docker pulls the image from **Docker Hub**.
3. **Container Creation:** Docker creates a new container from that image.
4. **Execution:** Docker starts the container, which runs the internal script and exits.

**Key Concepts to Observe:**

- **Unable to find image... locally:** This only happens the first time.
- **Pulling from library/hello-world:** Downloading from the Registry.
- **Status: Downloaded newer image:** Success.

## 🧠 **Real-World Case Study: "The Vanishing Container"**

- **Scenario:** You run `docker run hello-world` several times.
- **The Observation:** Each time, it runs, prints text, and stops. It doesn't "stay running" because the process inside the hello-world image is designed to finish quickly.
- **The Lesson:** A container's life is tied directly to the process it's running. When the process finishes, the container stops.

## 🤔 **Reflective Questions**

1. Why didn't Docker download the image the second time you ran the command?
2. What would happen if you ran `docker run hello-world` while your internet was disconnected (the first time)?
3. Where does the `hello-world` image come from by default?

## 📚 **Further Reading**

- **Get Started:** [Run your first container](https://docs.docker.com/get-started/)
- **Docker CLI:** [docker run reference](https://docs.docker.com/engine/reference/commandline/run/)

## 📝 **Mini Task (Production)**

1. Run `docker run hello-world` in your terminal.
2. Run it again and notice the lack of "Pulling..." messages.
3. Now, try running a different image: `docker run busybox echo "Hello from Busybox"`
   - Observe how Docker handles a different process (echo).
   - What was different about the output compared to `hello-world`?
