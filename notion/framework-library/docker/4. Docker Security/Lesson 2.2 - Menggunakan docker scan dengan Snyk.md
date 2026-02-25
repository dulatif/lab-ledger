# 💡 DK04.2.2: Menggunakan docker scan dengan Snyk

**Outline:**

- **The Tool (SEEI):** Introducing the built-in vulnerability scanner in the Docker CLI.
- **The Workflow (PPP):** Running your first local scan and interpreting the summary.
- **Your Mission (Production):** Auditing a local image for security flaws.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Tool (SEEI)**

**Docker Scan CLI**
Docker has partnered with Snyk to provide security scanning directly from your terminal. You don't need to upload your image to a website; you can scan it as soon as you build it.

**Why Snyk?**
Snyk maintains one of the most comprehensive vulnerability databases in the industry. By integrating it into `docker scan`, Docker allows developers to "Shift Left"—finding security issues during development instead of waiting for production.

### **Part 2: Practice - The Workflow (PPP)**

**How to run a scan:**

1. **Login (First time only):**
   ```bash
   docker scan --login
   ```
2. **Scan an Image:**
   ```bash
   docker scan my-app:latest
   ```
3. **Filter by Severity:**
   ```bash
   docker scan --severity high my-app:latest
   ```

**What the scanner looks at:**

- **OS Packages:** Outdated versions of `curl`, `apt`, `libssl`.
- **App Dependencies:** (If supported) Vulnerabilities in your `package.json` or `requirements.txt`.
- **Dockerfile Best Practices:** Suggestions like "Don't run as root" or "Use a smaller base image."

## 🧠 **Real-World Case Study: "The False Sense of Security"**

- **Scenario:** A developer builds a custom image based on `node:14`.
- **The Scan:** They run `docker scan my-node-app` and find 400 "Medium" and 12 "High" vulnerabilities.
- **The Fix:** Snyk suggests: "Switch to `node:14-alpine` to reduce vulnerabilities by 90%."
- **The Result:** The developer changes one line in their Dockerfile, rebuilds, and the new scan shows only 2 "Low" vulnerabilities.
- **The Lesson:** The scanner isn't just a "Red Light"; it's a guide to better choices.

## 🤔 **Reflective Questions**

1. Do you need an internet connection to run `docker scan`? (Answer: Yes, to check the latest CVE database).
2. Is `docker scan` free? (Answer: Yes, for a limited number of scans per month).
3. If a scan finds a vulnerability in a base image, can you fix it without changing the base image? (Usually no, you must update the `FROM` line).

## 📚 **Further Reading**

- **Docker Documentation:** [docker scan reference](https://docs.docker.com/engine/scan/)
- **Snyk:** [Docker Hub scanning integration](https://snyk.io/product/container-vulnerability-management/)

## 📝 **Mini Task (Production)**

1. Build a simple image using a "Heavy" base: `FROM node:14`.
2. Run `docker scan <image_name>`.
3. Note down the total number of vulnerabilities.
4. Now, change the base to `FROM node:14-alpine` and rebuild.
5. Scan again.
6. By what percentage did the vulnerabilities decrease?
7. How long did it take you to "Fix" those hundreds of security holes?
