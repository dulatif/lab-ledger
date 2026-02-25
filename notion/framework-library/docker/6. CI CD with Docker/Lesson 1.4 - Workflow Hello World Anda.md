# 💡 DK06.1.4: Workflow Hello World Anda

**Outline:**

- **The First Light (SEEI):** Overcoming the "Blank Page" fear by writing a working automation script.
- **The Execution (PPP):** Committing, pushing, and watching the live logs on GitHub.
- **Your Mission (Production):** Verifying the environment and runner capabilities.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The First Light (SEEI)**

**Keep it Simple**
The biggest mistake you can make is trying to build a complex Docker pipeline on your first try. Start by proving that the "Plumbing" works.

**The "Hello World" Goal:**

- Prove that GitHub can see your workflow file.
- Prove that the Ubuntu runner can start.
- Prove that you can see the terminal output.

### **Part 2: Practice - The Execution (PPP)**

**The "Hello World" Workflow:**

1. **Create the file:** `.github/workflows/hello.yml`.
2. **Add the content:**

```yaml
name: Hello GitHub Actions
on: [push]

jobs:
  greet:
    runs-on: ubuntu-latest
    steps:
      - name: Say Hello
        run: echo "Hello World! I am running on a GitHub Runner!"
      - name: Check Versions
        run: |
          node -v
          docker -v
          git --version
```

**How to trigger it:**

1. `git add .github/workflows/hello.yml`
2. `git commit -m "Add hello actions"`
3. `git push origin main`

**How to watch it:**

1. Go to your repository on GitHub.com.
2. Click **Actions**.
3. You will see a yellow spinning circle. Click on the commit message.
4. Click on the **"greet"** job on the left.
5. Expand the "Say Hello" step to see your message!

## 🧠 **Real-World Case Study: "The Secret Runner Tools"**

- **Scenario:** A developer was trying to figure out if they needed to "Install" Docker inside the GitHub Actions runner.
- **The Test:** They added `docker -v` to their "Hello World" workflow.
- **The Surprise:** The output showed `Docker version 20.10.x`.
- **The Discovery:** Most standard tools (Docker, Node, Python, AWS CLI) are **pre-installed** on the GitHub runners.
- **The Lesson:** Always use your "Hello World" to explore the environment. You might not need to install things you think you do.

## 🤔 **Reflective Questions**

1. Why do we put multiple commands in one `run` block using the `|` symbol?
2. What happens if you make a typo in the word `runs-on`?
3. How long did it take for the workflow to start after you pushed the code?

## 📚 **Further Reading**

- **GitHub:** [Quickstart for GitHub Actions](https://docs.github.com/en/actions/quickstart)
- **Guide:** [Finding your first workflow run](https://docs.github.com/en/actions/monitoring-and-troubleshooting-workflows/checking-workflow-run-status)

## 📝 **Mini Task (Production)**

1. Follow the steps in "Part 2" to create and push a "Hello World" workflow to one of your repositories.
2. Locate the "Live Logs" on GitHub.
3. Check the version of `docker-compose` on the runner.
4. Reflect: Does the runner have more processing power (CPUs/RAM) than your local machine? Search for "GitHub Actions runner specifications" to find out.
5. Successfully "Pass" your first automation build!
