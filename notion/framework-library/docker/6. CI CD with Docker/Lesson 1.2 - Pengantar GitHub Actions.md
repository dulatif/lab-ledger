# 💡 DK06.1.2: Pengantar GitHub Actions

**Outline:**

- **The Choice (SEEI):** Comparing GitHub Actions with other CI tools like Jenkins or GitLab CI.
- **The Convenience (PPP):** Understanding why "living next to your code" is the ultimate advantage.
- **Your Mission (Production):** Setting up the environment for your first automated workflow.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Choice (SEEI)**

**Where should your automation live?**
In the past, you needed to set up a separate server (like Jenkins) to run your automation. This was another server you had to patch, secure, and pay for.

**The "Integrated" Revolution:**
GitHub Actions moves the automation directly into your GitHub repository.

- You don't need to manage a server.
- The automation "sees" every event (push, pull request, issue).
- It's free for public repositories and has a generous free tier for private ones.

### **Part 2: Practice - The Convenience (PPP)**

**The "Action" Ecosystem:**
One of the best parts of GitHub Actions is the **Marketplace**. Instead of writing complex scripts for common tasks (like "Login to Docker Hub" or "Setup Python"), you can just "Use" an action written by the community.

**Example:**

- `actions/checkout@v3`: Downloads your code into the runner.
- `docker/login-action@v2`: Handles Docker authentication.

**The "Runner":**
When a workflow starts, GitHub provides a virtual machine (the **Runner**) to execute your code. You can choose Ubuntu, Windows, or macOS. For Docker work, we almost always use `ubuntu-latest`.

## 🧠 **Real-World Case Study: "The Jenkins Migration"**

- **Scenario:** A team was spending 20 hours a month just "Managing" their Jenkins plugins and fixing the Jenkins server when it crashed.
- **The Move:** They migrated all 50 of their builds to GitHub Actions.
- **The Outcome:** They deleted the Jenkins server and saved $200/month in cloud costs. More importantly, they saved those 20 hours of "Ops" work, which they used to build new features.
- **The Lesson:** "Serverless CI" (like GitHub Actions) allows you to focus on your code, not your tools.

## 🤔 **Reflective Questions**

1. Why is it called "GitHub Actions" instead of just "GitHub CI"? (Answer: Because it can do more than just CI, like managing GitHub issues or releasing documentation).
2. Do you have to pay to use GitHub Actions for your personal projects?
3. What happens if you need more RAM than the standard GitHub runner provides? (Hint: Self-hosted runners).

## 📚 **Further Reading**

- **GitHub:** [Actions documentation](https://docs.github.com/en/actions)
- **Marketplace:** [Explore GitHub Actions](https://github.com/marketplace?type=actions)

## 📝 **Mini Task (Production)**

1. Open one of your repositories on GitHub.
2. Click the **"Actions"** tab at the top.
3. Look at the "Suggested" workflows. Does GitHub recognize your programming language?
4. Click "Skip this and set up a workflow yourself."
5. Look at the file location it suggests: `.github/workflows/main.yml`.
6. Why is this specific folder name required?
