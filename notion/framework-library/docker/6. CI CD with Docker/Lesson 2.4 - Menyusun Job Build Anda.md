# 💡 DK06.2.4: Menyusun Job Build Anda

**Outline:**

- **The Flow (SEEI):** Understanding how to sequence steps for maximum clarity and speed.
- **The Best Practice (PPP):** Implementing proper naming, error handling, and logical grouping.
- **Your Mission (Production):** Designing a professional-grade Docker build configuration.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Flow (SEEI)**

**"A Messy build is a Hidden build"**
If your workflow has 20 steps with names like `step-1`, `step-2`, it will be impossible to debug when it fails. A professional build job should follow a logical story.

**The "Standard Story" of a Build:**

1. **Prepare:** Checkout code and set up the building tools.
2. **Build:** Use the Dockerfile to create the artifact.
3. **Scan:** Check the new image for security flaws (CVEs).
4. **Finalize:** Tag and prepare for the next job (Registry).

**Logical Grouping:**
Keep related commands in the same step. Use the `name:` field to describe **What** is happening, not **How**.

- _Good:_ `name: Build Production Image`
- _Bad:_ `name: docker build -t app:v1 .`

### **Part 2: Practice - The Best Practice (PPP)**

**The "Professional" Job Template:**

```yaml
jobs:
  docker-pipeline:
    runs-on: ubuntu-latest
    steps:
      - name: 📥 Checkout Repository
        uses: actions/checkout@v3

      - name: 🛠️ Set up Docker Buildx
        uses: docker/setup-buildx-action@v2

      - name: 🏗️ Build and Scan Image
        uses: docker/build-push-action@v4
        with:
          context: .
          load: true # Load the image into the local runner's Docker
          tags: local-image:latest

      - name: 🔍 Security Audit (Dive)
        run: |
          # Example: Use a tool to check image layers
          docker run --rm -v /var/run/docker.sock:/var/run/docker.sock wagoodman/dive --ci local-image:latest
```

**Why include a Scan step?**
Building is just half the battle. If you build a "Red" image (vulnerable), you shouldn't waste time pushing it to a registry.

## 🧠 **Real-World Case Study: "The 30-Minute Debugging Session"**

- **Scenario:** A developer's build failed. The log had 1,000 lines of text.
- **The Issue:** All commands were inside one giant `run` block.
- **The Frustration:** The error was at the top of the logs, but the developer had to scroll for 10 minutes to find it because GitHub didn't know where the "Build" step ended and the "Test" step started.
- **The Fix:** They split the commands into separate named Steps.
- **The Outcome:** The next time a build failed, GitHub automatically "collapsed" the successful steps and highlighted the exact failed step in **Red**. The developer found the error in 5 seconds.
- **The Lesson:** Steps are "Checkpoints." Use them to make failure visible.

## 🤔 **Reflective Questions**

1. Why is it helpful to add emojis 🚀 🛠️ to your step names? (Answer: It makes the GitHub UI much easier to scan at a glance).
2. What happens if a step in the middle of your job fails?
3. How can you skip a specific step if the branch is not `main`? (Answer: Using the `if:` conditional).

## 📚 **Further Reading**

- **GitHub:** [Workflow commands for GitHub Actions](https://docs.github.com/en/actions/using-workflows/workflow-commands-for-github-actions)
- **Guide:** [Optimizing your GitHub Actions workflow](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idstepsid)

## 📝 **Mini Task (Production)**

1. Take your previous "Docker Build" workflow.
2. Add a `name:` to every single step using clear, descriptive language (and emojis if you like!).
3. Add a "Verify" step that runs `docker images` and checks for the size of your new image.
4. Push and look at the "Actions" UI on GitHub.
5. Reflect: Does it look like a professional "Pipeline" now?
6. Experiment: Add a step that purposefully fails (e.g., `run: exit 1`) and see how GitHub highlights it.
