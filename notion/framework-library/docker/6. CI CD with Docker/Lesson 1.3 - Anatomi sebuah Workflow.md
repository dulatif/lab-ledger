# 💡 DK06.1.3: Anatomi sebuah Workflow

**Outline:**

- **The Blueprint (SEEI):** Dissecting the structure of a YAML file for GitHub Actions.
- **The Hierarchy (PPP):** Understanding the relationship between Workflows, Jobs, and Steps.
- **Your Mission (Production):** Designing the "Skeleton" of an automation process.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Blueprint (SEEI)**

**YAML as a Language of Choice**
GitHub Actions uses YAML files to define your automation. These files must be located in `.github/workflows/`.

**The Core Concepts:**

1. **Workflow:** The top-level automation process (e.g., "Main Build Pipeline").
2. **Events (on):** What triggers the workflow? (e.g., `on: push`).
3. **Jobs:** A set of steps that run on the same runner. A workflow can have multiple jobs running in parallel or sequence.
4. **Steps:** Individual tasks that run a command or an action.

### **Part 2: Practice - The Hierarchy (PPP)**

**The "Stack" Visualization:**

- **Workflow** (file level)
  - **Job** (VM level)
    - **Step** (Command level)
      - `run`: A simple shell command.
      - `uses`: A pre-built action from the marketplace.

**Example Structure:**

```yaml
name: CI Process # name of the workflow
on: [push] # trigger

jobs:
  build: # job ID
    runs-on: ubuntu-latest # runner environment
    steps:
      - name: Checkout # step name
        uses: actions/checkout@v3
      - name: Test
        run: echo "Testing..."
```

**Key Data structures:**

- `jobs`: A dictionary where each key is a unique ID for a job.
- `steps`: A list (array) of tasks to be performed in order.

## 🧠 **Real-World Case Study: "The Parallel Speedup"**

- **Scenario:** A project had 200 unit tests and 50 integration tests. Running them all in one job took 20 minutes.
- **The Optimization:** The team split the tests into two separate **Jobs**: one for unit tests and one for integration tests.
- **The Result:** GitHub Actions ran both jobs on two separate virtual machines at the same time.
- **The Outcome:** The total build time dropped from 20 minutes to 8 minutes.
- **The Lesson:** Understand your Hierarchy. If things don't depend on each other, make them separate Jobs to save time.

## 🤔 **Reflective Questions**

1. Can you trigger a workflow manually without pushing code? (Answer: Yes, using the `workflow_dispatch` event).
2. What is the difference between a `run` step and a `uses` step?
3. If Step 1 fails, will Step 2 still run? (Answer: No, the job stops immediately unless you specifically tell it to continue).

## 📚 **Further Reading**

- **GitHub:** [Workflow syntax reference](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions)
- **Guide:** [Understanding GitHub Actions](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions)

## 📝 **Mini Task (Production)**

1. Open a text editor.
2. Draft a YAML structure for a workflow that:
   - Is named "Production Build."
   - Triggers only on a pull request (Event: `pull_request`).
   - Runs on Ubuntu.
   - Has two steps: "Linting" and "Security Scan."
3. Notice: Which part of your draft is the "Specific Work" and which part is the "Framework" (Boilerplate)?
4. Reflect: Why is YAML better than writing a bash script for the entire process?
