# 💡 DK06.5.4: Tips Workflow Produksi yang Aman

**Outline:**

- **The Fortress (SEEI):** Understanding the mentality of "Immutable Infrastructure" and "Least Privilege."
- **The Shield (PPP):** Implementing Protected Branches, Environments, and Manual Approvals.
- **Your Mission (Production):** Hardening your pipeline against accidental human error.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Fortress (SEEI)**

**"Don't Trust Yourself"**
Even the best developers make mistakes. A "Safe Workflow" is designed to stop a human from pushing a button that would destroy production.

**The Rules of Production:**

1. **No Manual SSH:** All changes must come through a Git commit. No "Quick fixes" directly on the server.
2. **Protected Branches:** Never allow direct pushes to `main`. Require a Pull Request and at least 1 review.
3. **Environment Isolation:** Use different secrets for `Staging` and `Production`.

### **Part 2: Practice - The Shield (PPP)**

**GitHub Environments:**
GitHub (Pro/Team) allows you to define "Environments" with specific rules.

1. Create an environment named `Production`.
2. Add a **"Required Reviewer."**
3. Now, even if the building phase is 100% Green, the "Deploy" job will wait until a manager or senior dev clicks **"Approve."**

**The "Pull Request" Pattern:**

```yaml
on:
  pull_request:
    branches: [main]
```

_Always build and test in the PR. Only deploy after the code is merged into `main`._

## 🧠 **Real-World Case Study: "The Accidental Master Push"**

- **Scenario:** A tired developer thought they were on a `feature` branch. They ran `git commit -am "Fix stuff"` and `git push origin main`.
- **The Issue:** Their project didn't have Branch Protection. The CI/CD pipeline immediately deployed their "Work in Progress" code to 100,000 users.
- **The Result:** The "Work in Progress" code had a broken layout. The company lost sales for 15 minutes.
- **The Fix:** They enabled **Branch Protection** on GitHub.
- **The Outcome:** The next time that developer tried to push to `main`, GitHub said: "Denied. Please create a Pull Request."
- **The Lesson:** Technical safeguards are better than human memory.

## 🤔 **Reflective Questions**

1. Why should you use a "Wait" time between Staging and Production deployments?
2. What is an "Artifact" in GitHub Actions, and why is it safer than building a new image for every environment?
3. How do you revoke an SSH key if a team member leaves the company?

## 📚 **Further Reading**

- **GitHub:** [Using environments for deployment](https://docs.docker.com/en/actions/deployment/targeting-different-environments/using-environments-for-deployment)
- **Guide:** [CI/CD Security Best Practices](https://www.cisa.gov/resources-tools/resources/defending-continual-integration-continual-delivery-pipelines)

## 📝 **Mini Task (Production)**

1. Go to your GitHub Repository Settings -> Branches.
2. Add a "Branch Protection Rule" for `main`.
3. Check **"Require a pull request before merging."**
4. Check **"Require status checks to pass before merging"** and select your Docker Build workflow.
5. Reflect: How does this "Gate" change your feeling when you write code? Does it make you feel more restricted or more confident?
6. Successfully complete the Docker course! You are now a Container Orchestration professional!
