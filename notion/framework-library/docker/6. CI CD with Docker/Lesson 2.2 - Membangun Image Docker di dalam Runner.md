# 💡 DK06.2.2: Membangun Image Docker di dalam Runner

**Outline:**

- **The Environment (SEEI):** Understanding that the GitHub Runner is a real Ubuntu machine with Docker pre-installed.
- **The Build (PPP):** Writing the steps to `checkout` code and run `docker build`.
- **Your Mission (Production):** Automating the creation of a local image on the runner.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Environment (SEEI)**

**"Runner" is just a server**
When GitHub Actions "Runs" your job, it's just spinning up an ephemeral (temporary) virtual machine. Because this VM runs Linux, it has full access to the Docker CLI.

**Steps for a Docker Build:**

1. **Checkout:** The VM starts empty. You must copy your code into it.
2. **Build:** Run the standard `docker build` command.
3. **Verify:** Check if the image exists and can start.

**Wait, what about speed?**
Every time the runner starts, it has a "Cold" Docker cache. Building from scratch every time is slow. In later lessons, we will learn how to "Cache" layers between builds.

### **Part 2: Practice - The Build (PPP)**

**The "Standard" Docker Build Job:**

```yaml
jobs:
  build-docker:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Build Image
        run: docker build -t my-app:${{ github.sha }} .

      - name: Verify Image
        run: docker images
```

**Understanding `${{ github.sha }}`:**
This is a "Global Variable" provided by GitHub. It is the unique 40-character hash of the commit. Using this as a tag ensures every build has a unique, traceable name.

## 🧠 **Real-World Case Study: "The Broken Production Build"**

- **Scenario:** A developer pushed code that "Worked on their machine" but had a missing dependency in the Dockerfile.
- **The Detection:** The GitHub Action tried to build the image. Because the dependency was missing, the `docker build` command returned an error (exit code 1).
- **The Result:** GitHub marked the build as **Failed** (Red X). The merge to `main` was blocked.
- **The Outcome:** The "Broken" code never reached production. The developer fixed the Dockerfile, pushed again, and the Action turned Green.
- **The Lesson:** Automation acts as a "Quality Gate." If it doesn't build in the runner, it shouldn't go anywhere else.

## 🤔 **Reflective Questions**

1. Why do we need the `actions/checkout` step before the `docker build`?
2. Can you run `docker run` inside the GitHub Runner? (Answer: Yes! This is how you run integration tests).
3. If your Dockerfile is in a subfolder named `/app`, how do you change the build command? (Answer: `docker build -t app ./app`).

## 📚 **Further Reading**

- **GitHub Actions:** [Building and testing Docker images](https://docs.github.com/en/actions/publishing-packages/publishing-docker-images)
- **Tool:** [Docker Build Push Action (Advanced)](https://github.com/docker/build-push-action)

## 📝 **Mini Task (Production)**

1. Create a repository with a very simple `Dockerfile` (e.g., `FROM alpine`).
2. Add a workflow that checks out the code and runs `docker build -t test .`.
3. Push and verify the results.
4. Now, intentionally break the Dockerfile (e.g., use an image that doesn't exist: `FROM non_existent_image`).
5. Watch the GitHub Action fail.
6. Explore the "Logs" and find the exact line where Docker stopped.
7. Reflect: How much faster is this feedback compared to "Waiting for a tester to find the bug"?
