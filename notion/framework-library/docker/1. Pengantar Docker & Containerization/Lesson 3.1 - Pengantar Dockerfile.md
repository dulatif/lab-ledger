# 💡 DK03.1: Pengantar Dockerfile

**Outline:**

- **The Concept (SEEI):** Defining the Dockerfile as the source code for your environment.
- **The Core Syntax (PPP):** Understanding the basic structure: Instruction + Argument.
- **Your Mission (Production):** Transforming a manual setup into an automated blueprint.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Concept (SEEI)**

**What is a Dockerfile?**
A Dockerfile is a simple text document that contains all the commands a user could call on the command line to assemble an image. It is essentially the "Source Code" of your system environment.

**Why Not Just Use `docker commit`?**
While you _could_ manually install things inside a container and "save" it, this is bad practice because:

1. **No Version Control:** You can't track changes to the environment.
2. **Not Reproducible:** How did you install that library? Nobody knows.
3. **Heavy:** Manual changes lead to bloated, inefficient images.

**The Solution:** Use a `Dockerfile`. It is declarative, version-controllable, and readable by both humans and Docker.

### **Part 2: Practice - The Core Syntax (PPP)**

**The Anatomy of a Dockerfile:**
Instructions are usually written in **UPPERCASE** to distinguish them from arguments.

**A Very Simple Example:**

```dockerfile
# 1. Start from a base image
FROM alpine

# 2. Run a command during the buildup
RUN apk add --no-cache curl

# 3. Define the default command when the container starts
CMD ["curl", "https://google.com"]
```

**Key Steps:**

1. Create a file named `Dockerfile` (no extension).
2. Write your instructions.
3. Build the image: `docker build -t my-custom-image .`

## 🧠 **Real-World Case Study: "The Secret Sauce"**

- **Scenario:** A company has a legacy Java application that requires a specific version of the JDK and 10 environment variables to be set.
- **The Problem:** Setting this up manually on every developer's machine takes 2 days.
- **The Solution:** A 15-line `Dockerfile` that specifies the JDK version, copies the `.jar` file, and sets the `ENV` variables.
- **The Result:** Onboarding time goes from **2 days** to **5 minutes** (the time it takes to build the image).

## 🤔 **Reflective Questions**

1. Why is it better to have a Dockerfile than a manual setup guide in a PDF?
2. What is the difference between `FROM` and `CMD`?
3. What happens if you run `docker build` but haven't created a file named `Dockerfile` yet?

## 📚 **Further Reading**

- **Docker Orientation:** [Dockerfile reference](https://docs.docker.com/engine/reference/builder/)
- **Guide:** [Build your own image](https://docs.docker.com/get-started/02_our_app/)

## 📝 **Mini Task (Production)**

1. Create a folder named `docker-lab`.
2. Inside, create a file named `Dockerfile`.
3. Write a Dockerfile that:
   - Starts `FROM ubuntu`.
   - `RUN apt-get update && apt-get install -y figlet`.
   - `CMD ["figlet", "Hello Docker"]`.
4. Build it: `docker build -t hello-figlet .`
5. Run it: `docker run hello-figlet`.
   - Do you see the ASCII art?
