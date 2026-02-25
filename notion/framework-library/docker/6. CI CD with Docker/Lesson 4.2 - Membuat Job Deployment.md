# 💡 DK06.4.2: Membuat Job Deployment

**Outline:**

- **The Separation (SEEI):** Understanding why "Build" and "Deploy" should be two separate jobs.
- **The Dependency (PPP):** Using the `needs:` keyword to ensure deployment only happens if the build succeeds.
- **Your Mission (Production):** Architecting a multi-job workflow.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Separation (SEEI)**

**"Don't put all your eggs in one basket"**
It is tempting to put the "Deployment" steps at the end of your build job. But this is bad practice.

**Why Separate Build and Deploy?**

1. **Reusability:** You might want to build once but deploy to three different environments (Dev, Staging, Prod).
2. **Clarity:** If the build passes but the deployment fails, you want to see exactly which phase of the "Pipeline" is broken.
3. **Environment Isolation:** Deployment jobs often need different secrets (like production SSH keys) than build jobs.

### **Part 2: Practice - The Dependency (PPP)**

**The `needs:` Keyword:**
By default, GitHub Actions runs all jobs in parallel. We use `needs` to create a serial sequence.

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Build and Push
        run: echo "Building..."

  deploy:
    runs-on: ubuntu-latest
    needs: build # 👈 MAGIC KEYWORD: Wait for build to finish!
    steps:
      - name: Deploy to Server
        run: echo "Deploying..."
```

**The "Success" Gate:**
The `deploy` job will **only** start if the `build` job returns a Green checkmark. This prevents you from accidentally deploying a "Fail" build to your users.

## 🧠 **Real-World Case Study: "The Broken Build Deployment"**

- **Scenario:** A team had a single giant job. The "Test" step failed, but because of a typo in the shell script, the "Deploy" step continued anyway.
- **The Result:** They deployed broken code to production.
- **The Fix:** They split the workflow into two jobs: `job_test` and `job_deploy`. They added `needs: job_test`.
- **The Outcome:** The next time a test failed, GitHub Actions instantly "Canceled" the deployment job. The production server was safe.
- **The Lesson:** Use the machine's hierarchy to protect your production environment. `needs:` is your digital insurance policy.

## 🤔 **Reflective Questions**

1. Can one job "need" more than one other job? (Answer: Yes!).
2. What is the difference between sequential jobs and parallel jobs?
3. How do you pass a variable (like the new version number) from the build job to the deploy job? (Hint: Workflow outputs).

## 📚 **Further Reading**

- **GitHub:** [Jobs that depend on other jobs](https://docs.github.com/en/actions/using-jobs/using-jobs-in-a-workflow#defining-prerequisite-jobs)
- **Guide:** [Storing workflow data as artifacts](https://docs.github.com/en/actions/using-workflows/storing-workflow-data-as-artifacts)

## 📝 **Mini Task (Production)**

1. Modify your previous workflow to have two jobs: `build` and `test`.
2. Make `test` depend on `build`.
3. In `build`, add a step that `echo "BUILD_VERSION=v1.0"`.
4. Run the workflow. Notice how the visual diagram on GitHub now shows an arrow connecting the two boxes.
5. Now, make the `build` job fail (e.g., `run: exit 1`).
6. What happened to the `test` job? Did it even start?
7. Reflect: How does this "Visual Workflow" help you explain your work to your manager?
