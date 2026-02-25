# 💡 DK06.4.1: Strategi Deployment Sederhana

**Outline:**

- **The Last Mile (SEEI):** Understanding the bridge between the Registry and the Production server.
- **The Pull Model (PPP):** Comparing "Push-to-Deploy" (CI triggers server) vs "Pull-to-Deploy" (Server watches Registry).
- **Your Mission (Production):** Selecting a deployment workflow for a single-server setup.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Last Mile (SEEI)**

**"The image is in the registry... now what?"**
Deployment is the final stage of the CI/CD pipeline. It's the moment the software becomes real for the users.

**The "Simple" Deployment Flow:**

1. Log in to the production server.
2. `docker pull` the new image.
3. `docker stop` the old container.
4. `docker run` the new container.

**The Problems with Traditional (Manual) Deployment:**

- **Inconsistency:** You might forget to pull the image or miscalculate the run command.
- **Downtime:** There is a gap between stopping and starting.
- **Security:** You have to share your server's SSH keys with several developers.

### **Part 2: Practice - The Pull Model (PPP)**

**Push-to-Deploy (The CI/CD Way):**
In this model, the GitHub Action finishes the building phase and then immediately connects to your server to trigger the update. This is the most common pattern for small sets of servers.

**What is needed?**

- **SSH Access:** The GitHub Runner needs to be able to talk to your server over port 22.
- **Service Restart Logic:** A simple `docker-compose pull && docker-compose up -d` is often enough to update multiple containers at once.

## 🧠 **Real-World Case Study: "The Midnight Manual Deploy"**

- **Scenario:** A developer had to update a bug fix at midnight.
- **The Mistake:** They SSH'd into the server and accidentally ran `docker rm` on the database container instead of the app container.
- **The Result:** The site was offline for 4 hours while they restored the database from a backup.
- **The Fix:** They moved to an automated "GitHub Action" deployment.
- **The Outcome:** Now, nobody SSH's into the server anymore. They just merge code to `main`. The machine handles the `docker-compose up` command, and it **never** targets the database by mistake.
- **The Lesson:** Keep humans away from the production terminal. Use pipelines.

## 🤔 **Reflective Questions**

1. Why is it better to use `docker-compose` for deployment instead of raw `docker run`?
2. What happens if the deployment fails halfway through?
3. How do you handle database migrations during a deployment? (Answer: This is usually a separate step BEFORE the app update).

## 📚 **Further Reading**

- **Atlassian:** [What is Continuous Deployment?](https://www.atlassian.com/continuous-delivery/continuous-deployment)
- **Tool:** [Watchtower - Automatic Docker container updates](https://github.com/containrrr/watchtower)

## 📝 **Mini Task (Production)**

1. Imagine you have a server.
2. What are the 3 commands you would run manually to update an image?
3. Now, think about how you would put those 3 commands in a bash script.
4. If that script lived on your server, how could GitHub Actions "Trigger" that script?
5. Reflect: What is the biggest "Risk" in this simple setup? (Hint: Handling SSH keys securely).
