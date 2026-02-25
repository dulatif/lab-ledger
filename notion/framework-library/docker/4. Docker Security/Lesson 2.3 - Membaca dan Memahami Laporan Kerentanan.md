# 💡 DK04.2.3: Membaca dan Memahami Laporan Kerentanan

**Outline:**

- **The Report (SEEI):** Learning how to filter through the noise of a security scan report.
- **The Prioritization (PPP):** Understanding the difference between "Vulnerable" and "Exploitable" in your context.
- **Your Mission (Production):** Deciding which bugs to fix first in a legacy image.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Report (SEEI)**

**Information Overload**
A standard `docker scan` can return hundreds of results. If you try to fix every single "Low" vulnerability, you will never ship any code. You need to develop a strategy for reading these reports.

**Key Components of a Finding:**

1. **Severity:** (Critical, High, Medium, Low).
2. **Package Name:** (e.g., `libxml2`).
3. **Vulnerability ID:** (e.g., `SNYK-DEBIAN11-LIBXML2-321456`).
4. **Remediation:** The advice on how to fix it (usually "Upgrade to version X").

### **Part 2: Practice - The Prioritization (PPP)**

**The "Context" Filter:**
Just because a library is vulnerable doesn't mean _your_ application is exploitable. Ask these questions:

- **Reachability:** Does my app even use the vulnerable function in that library?
- **Exposure:** Is this container exposed to the public internet, or is it an internal worker?
- **Environment:** Is there an existing security control (like a Firewall) that mitigates the risk?

**General Fix Strategy:**

1. **Apply Base Image Updates:** Often, just running `FROM node:18.15` -> `FROM node:18.16` fixes 50% of the report.
2. **Update the Dockerfile:** Add `RUN apt-get update && apt-get upgrade -y [package]` as a last resort.
3. **Accept Risk:** If a "Low" vulnerability has no fix, document it and move on.

## 🧠 **Real-World Case Study: "The Vulnerability Wall"**

- **Scenario:** A security officer tells a developer they cannot deploy because the image has 200 "Medium" vulnerabilities.
- **The Frustration:** The developer argues that none of those packages are used by the app; they are just part of the default Debian OS.
- **The Compromise:** They switch the base image to **Distroless**.
- **The Result:** The vulnerability count drops from 200 to 5. The security officer is happy, and the developer didn't have to change a single line of application code.
- **The Lesson:** Don't fight the report; change the environment to reduce the report's size.

## 🤔 **Reflective Questions**

1. What is the difference between a "Fixed In" version and the "Current" version in a report?
2. If a report says "No remediation available," what should you do?
3. Why are "Critical" vulnerabilities always priority #1?

## 📚 **Further Reading**

- **Snyk:** [How to read a vulnerability report](https://snyk.io/blog/understanding-vulnerability-reports/)
- **Guide:** [Interpreting Docker Scan results](https://docs.docker.com/engine/scan/#interpreting-results)

## 📝 **Mini Task (Production)**

1. Look at this hypothetical scan result:
   - `curl`: 7.64.0 (Vulnerable: CVE-2021-1234, High). Fixed in: 7.74.0.
   - `python-pip`: 20.0 (Vulnerable: CVE-2020-5555, Low). Fixed in: 21.0.
2. Which one would you fix first? Why?
3. Write the `RUN` command you would add to a Debian-based Dockerfile to fix the `curl` vulnerability specifically.
4. If your app doesn't use `curl` at all, should you still fix it or just remove `curl` from the image entirely?
