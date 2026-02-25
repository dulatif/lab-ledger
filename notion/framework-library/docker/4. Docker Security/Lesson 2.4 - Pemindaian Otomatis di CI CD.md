# 💡 DK04.2.4: Pemindaian Otomatis di CI/CD

**Outline:**

- **The Gatekeeper (SEEI):** Transitioning from manual scanning to automated security "Gates."
- **The Failure (PPP):** Configuring your pipeline to "Break the build" if vulnerabilities are found.
- **Your Mission (Production):** Designing a DevSecOps pipeline logic.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Gatekeeper (SEEI)**

**Scanning is a chore**
If scanning depends on a developer remembering to run a command, it won't happen. To be truly secure, scanning must be part of the "Mechanical" process of building software.

**The DevSecOps Loop:**

1. Developer pushes code.
2. CI/CD builds the image.
3. **CI/CD scans the image.**
4. If the scan fails (e.g., Critical found), the image is NOT pushed to the registry.
5. The developer is notified to fix it.

### **Part 2: Practice - The Failure (PPP)**

**The "Fail-on" Threshold:**
Most CI/CD scan plugins allow you to set a "Severity Threshold."

- `fail-on: high`: The build passes if only "Medium" and "Low" are found.
- `fail-on: all`: The build only passes if the image is 100% clean. (Warning: This is very hard to maintain).

**Example Scenario (GitHub Actions):**
You use an action like `snyk/actions/docker@master`. You configure it with your Snyk token and the name of the image you just built. It will return an exit code of `1` if it finds vulnerabilities above your threshold, which stops the workflow.

## 🧠 **Real-World Case Study: "The Midnight Patch"**

- **Scenario:** A zero-day vulnerability is announced at 2 AM.
- **The Automated Victory:** A developer pushes a tiny bug fix for an unrelated feature at 9 AM. The CI/CD pipeline runs, scans the image, and **automatically rejects the build** because it detected the new zero-day library vulnerability.
- **The Observation:** The developer didn't even know about the zero-day yet, but the CI/CD "Gatekeeper" prevented the vulnerable code from ever reaching the production server.
- **The Lesson:** Automation provides security even when humans are tired or unaware.

## 🤔 **Reflective Questions**

1. Why is it better to fail a build _before_ pushing to the registry?
2. If a build fails due to a vulnerability, should the developer be allowed to "bypass" it?
3. How do you handle "False Positives" in an automated pipeline?

## 📚 **Further Reading**

- **Snyk:** [Scanning Docker images in GitHub Actions](https://support.snyk.io/hc/en-us/articles/360010906237-Scan-Docker-images-in-GitHub-Actions)
- **Guide:** [CI/CD security best practices](https://docs.docker.com/develop/devsecops/)

## 📝 **Mini Task (Production)**

1. Imagine you are writing a script for a CI/CD pipeline.
2. Step 1: `docker build -t app:latest .`
3. Step 2: `docker scan --severity high app:latest`
4. Step 3: `docker push registry.com/app:latest`
5. What happens if Step 2 returns an error? Will Step 3 still run?
6. Research the command `docker scan --exit-code`. How does this help in a shell script?
7. Draft a small logic (in words) on how you would notify the security team if a scan fails.
