# 💡 DK04.1: Perintah docker build

**Outline:**

- **The Command (SEEI):** Understanding `docker build` as the compiler for your Docker environment.
- **The Execution (PPP):** Knowing the significance of the "Build Context" and the `-t` flag.
- **Your Mission (Production):** Transforming a text Dockerfile into a usable Image.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Command (SEEI)**

**The "Compilation" of Images**
Just as you compile code into a binary, you "build" a Dockerfile into an **Image**. This process executes each instruction in the Dockerfile one by one, creating a layer for each step.

**The Anatomy of the Command:**
`docker build [OPTIONS] PATH | URL | -`

### **Part 2: Practice - The Execution (PPP)**

**The "Big Two" Arguments:**

1. **`-t` (Tag):** Gives your image a human-readable name.
   - `docker build -t my-app-name .`
2. **The Build Context (`.`):** This is the path to the folder containing your Dockerfile and the files you want to `COPY`.
   - The dot (`.`) means "use the current directory as the context."

**What Happens During the Build?**

1. **Sending Context:** Docker sends all files in the directory to the Docker Daemon.
   - _Warning: If your folder is huge, this step will be slow!_
2. **Step execution:** Docker runs each line (Step 1/X).
3. **Success:** You get an "Image ID" and a "Tag."

## 🧠 **Real-World Case Study: "The Secret Context"**

- **Scenario:** A developer runs `docker build .` from their home directory.
- **The Problem:** The build takes forever and then fails with "Out of Disk Space."
- **The Discovery:** Because they used `.` in their home directory, Docker tried to send their entire "Documents," "Downloads," and "Desktop" folders to the Docker daemon.
- **The Lesson:** Always run `docker build` from the specific project folder or use a `.dockerignore` file. The Build Context is powerful—don't overload it!

## 🤔 **Reflective Questions**

1. Why do we need the `-t` flag? What happens if you omit it? (Answer: You only get an Image ID, which is hard to remember).
2. What does the `.` at the end of the command represent?
3. If you change a file _outside_ the build context folder, can the Dockerfile `COPY` it?

## 📚 **Further Reading**

- **Docker CLI:** [docker build reference](https://docs.docker.com/engine/reference/commandline/build/)
- **Guide:** [Build an image from a Dockerfile](https://docs.docker.com/develop/develop-images/baseimages/)

## 📝 **Mini Task (Production)**

1. Go to your `docker-lab` folder.
2. Build your image with a specific name and version tag:
   ```bash
   docker build -t lab-image:v1.0 .
   ```
3. Run `docker image ls` to see your newly named image.
4. Try building it again without the `-t` flag. Notice the output. How would you run a container from that nameless image?
