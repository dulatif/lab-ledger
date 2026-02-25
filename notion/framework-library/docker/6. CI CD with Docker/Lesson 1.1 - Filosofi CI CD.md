# 💡 DK06.1.1: Filosofi CI/CD

**Outline:**

- **The Friction (SEEI):** Understanding the "Manual Deployment" bottleneck and how it slows down innovation.
- **The Pipeline (PPP):** Defining Continuous Integration, Continuous Delivery, and Continuous Deployment.
- **Your Mission (Production):** Mapping your current manual steps to an automated flow.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Friction (SEEI)**

**"Deploying is scary"**
In many traditional teams, deploying a new feature involves a 10-page Word document, 3 hours of manual work, and a "Code Freeze" on weekends. This is slow, error-prone, and leads to burnout.

**The Solution: CI/CD**
CI/CD is a philosophy of automating every step between "Code being written" and "Code being used by customers."

**The Goals:**

- **Speed:** Push fixes in minutes, not days.
- **Reliability:** The machine never forgets a step.
- **Confidence:** Automated tests prove the code works before it's deployed.

### **Part 2: Practice - The Pipeline (PPP)**

**The "Three Sisters" of Automation:**

1. **Continuous Integration (CI):**
   - Every `git push` triggers an automated build and test.
   - **Goal:** Catch bugs as soon as they are written.
2. **Continuous Delivery (CD):**
   - The code is automatically tested and ready to be deployed at any time.
   - **Action:** A human still clicks a "Deploy" button.
3. **Continuous Deployment (CD):**
   - The "Holy Grail." Every change that passes tests goes straight to production automatically.
   - **Goal:** Zero human intervention in the shipping process.

## 🧠 **Real-World Case Study: "The Knight Capital Disaster"**

- **Scenario:** In 2012, a major trading firm tried to deploy new software manually to 8 servers.
- **The Mistake:** A human forgot to copy the new code to the 8th server.
- **The Result:** The old code on that one server started executing "Test" trades using real money. The company lost **$440 million in 45 minutes**.
- **The Remediation:** They went bankrupt.
- **The Lesson:** Automation isn't just about "Convenience"; it's about "Survival." Humans are bad at repetitive tasks; machines are perfect at them.

## 🤔 **Reflective Questions**

1. Why do people often fear automated deployment?
2. What is the difference between "Delivering" and "Deploying"?
3. If your automated tests fail, should your CI/CD pipeline proceed to the next step? (Answer: No! Stop immediately).

## 📚 **Further Reading**

- **Atlassian:** [What is CI/CD?](https://www.atlassian.com/continuous-delivery/ci-vs-ci-vs-cd)
- **RedHat:** [CI/CD pipeline explained](https://www.redhat.com/en/topics/devops/what-is-ci-cd)

## 📝 **Mini Task (Production)**

1. Take a piece of paper (or a digital note).
2. List all the steps you currently take to deploy your app (e.g., "SSH into server", "Pull git", "Restart Docker").
3. For each step, ask: "Could a computer do this if I gave it the right script?"
4. Identify which step is the most "Dangerous" or "Boring."
5. Reflect: How would your life change if you never had to do that boring step again?
