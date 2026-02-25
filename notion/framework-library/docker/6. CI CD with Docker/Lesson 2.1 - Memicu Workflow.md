# 💡 DK06.2.1: Memicu Workflow

**Outline:**

- **The Catalyst (SEEI):** Understanding the different events that can start your automation.
- **The Filter (PPP):** Using paths, branches, and tags to control _when_ a build runs.
- **Your Mission (Production):** Configuring a workflow that only runs when the "Dockerfile" changes.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Catalyst (SEEI)**

**Why "on: push" is not enough**
If you run your Docker build on _every_ push, you might be wasting resources. What if you just updated a README file or a documentation folder? Does a Docker image need to be built then?

**Types of Triggers:**

1. **`push`:** The most common. Best for continuous integration.
2. **`pull_request`:** Essential for code reviews. Build and test the code _before_ it's merged.
3. **`schedule`:** Run a cron job (e.g., "Build every night at midnight to check for new security vulnerabilities").
4. **`workflow_dispatch`:** The "Manual" button in the GitHub UI.

### **Part 2: Practice - The Filter (PPP)**

**Fine-Grained Control:**

1. **Branch Filtering:**
   ```yaml
   on:
     push:
       branches: [main, "feature/*"] # only run on main or feature branches
   ```
2. **Path Filtering (The Gold Standard):**
   _Only build the image if the code or Dockerfile changes!_
   ```yaml
   on:
     push:
       paths:
         - "src/**"
         - "Dockerfile"
         - "package.json"
   ```
3. **Ignoring Paths:**
   ```yaml
   on:
     push:
       paths-ignore:
         - "docs/**"
         - "*.md"
   ```

## 🧠 **Real-World Case Study: "The Resource Burner"**

- **Scenario:** A team had a massive repository with 50 different microservices.
- **The Problem:** Their GitHub Action was set to `on: push`. Every time _any_ developer updated _any_ file, 50 integration tests started, costing the company thousands of minutes / month.
- **The Fix:** They implemented **Path Filtering** per workflow. The "Auth Service" build only triggered if the `/auth/` folder changed.
- **The Result:** Build efficiency increased by 90%. Developers got feedback faster, and the company's bill plummeted.
- **The Lesson:** Be specific. If it doesn't need to run, don't let it run.

## 🤔 **Reflective Questions**

1. Why shouldn't you run a deployment workflow on every `push` to a `feature/` branch?
2. What happens if you specify both `branches` and `paths`? (Answer: Both must match for the workflow to run).
3. How do you trigger a workflow at exactly 2 AM every Monday? (Answer: Using `cron` syntax).

## 📚 **Further Reading**

- **GitHub:** [Events that trigger workflows](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows)
- **Guide:** [Workflow filters for branches and paths](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#onpushpull_requestbranchestagsbranches-ignoretags-ignore)

## 📝 **Mini Task (Production)**

1. Modify your "Hello World" workflow.
2. Add a `paths` filter so it only runs if you change a file in a folder named `docker/`.
3. Push the change.
4. Now, change a dummy file in the root directory (e.g., `test.txt`) and push.
5. Watch the "Actions" tab. Did it run?
6. Now, change a file inside `docker/` and push.
7. Did it run this time?
8. Reflect: How does this help manage a "Monorepo" setup?
