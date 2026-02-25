# 💡 DK04.1.3: Memilih Base Image yang Tepat

**Outline:**

- **The Foundation (SEEI):** Understanding why your choice of `FROM` determines your image's security posture.
- **The Options (PPP):** Comparing Full, Slim, Alpine, and Distroless images.
- **Your Mission (Production):** Migrating an app from a "Heavy" to a "Secure" base image.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Foundation (SEEI)**

**Supply Chain Security**
Your Docker image is only as secure as the image it is built upon. If you start with `FROM ubuntu:latest`, you are inheriting 500+ installed packages, many of which you don't need and some of which have known vulnerabilities (CVEs).

**The "Slimming" Spectrum:**

- **Full Images:** (e.g., `node:latest`) Feature-rich, but full of "CVE noise."
- **Slim Images:** (e.g., `node:slim`) Debian-based, but stripped of non-essential tools.
- **Alpine Images:** (e.g., `node:alpine`) Based on Alpine Linux. Tiny (~5MB base), very secure, but uses `musl` instead of `glibc`.
- **Distroless Images:** (e.g., `gcr.io/distroless/nodejs`) Contains ONLY your app and its runtime dependencies. No shell, no package manager. Maximum security.

### **Part 2: Practice - The Options (PPP)**

**Which one to pick?**

| Type           | Size   | Security  | Ease of Use                   |
| :------------- | :----- | :-------- | :---------------------------- |
| **Standard**   | 400MB+ | Moderate  | High (Everything works)       |
| **Slim**       | ~150MB | Good      | Good (Standard Debian)        |
| **Alpine**     | ~100MB | Excellent | Medium (Musl compatibility)   |
| **Distroless** | ~50MB  | Maximum   | Hard (No shell for debugging) |

**The "Verified" Rule:**
Always prefer images with the **"Docker Official Image"** or **"Verified Publisher"** badge on Docker Hub. These images are audited regularly by the Docker library team.

## 🧠 **Real-World Case Study: "The Python Compatibility Bug"**

- **Scenario:** A data scientist moves their Python script to `python:alpine` to save space.
- **The Issue:** The script crashes during `pip install pandas` with thousands of compilation errors.
- **The Diagnosis:** `pandas` and `numpy` expect `glibc` (found in Debian/Ubuntu). Alpine uses `musl`.
- **The Solution:** They switch to `python:slim`.
- **The Result:** The image is still small (much smaller than the full image), but the compatibility issues vanish.
- **The Lesson:** Smallest is not always best. Choose the smallest image that _still supports your libraries_.

## 🤔 **Reflective Questions**

1. Why do Distroless images have no shell (`/bin/sh`)?
2. What is the risk of using the `:latest` tag in production? (Answer: Non-reproducibility and sudden breaking changes).
3. How do you find the vulnerabilities in a base image before you build upon it?

## 📚 **Further Reading**

- **Google Cloud:** [Distroless container images](https://github.com/GoogleContainerTools/distroless)
- **Snyk Blog:** [Choosing the right base image](https://snyk.io/blog/choosing-the-right-docker-base-image/)

## 📝 **Mini Task (Production)**

1. Open Docker Hub and search for `nginx`.
2. Look at the "Tags" tab.
3. Compare the sizes of:
   - `nginx:latest`
   - `nginx:alpine`
   - `nginx:perl`
4. Which one has the fewest "Vulnerabilities" listed (if using a scanner)?
5. If you wanted to run a simple static website with maximum security, which would you pick?
