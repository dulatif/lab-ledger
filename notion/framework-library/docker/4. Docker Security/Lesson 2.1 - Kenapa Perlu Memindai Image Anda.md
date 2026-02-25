# 💡 DK04.2.1: Kenapa Perlu Memindai Image Anda

**Outline:**

- **The Awareness (SEEI):** Understanding that "If it builds, it doesn't mean it's safe."
- **The CVE (PPP):** Defining Common Vulnerabilities and Exposures and their impact on containers.
- **Your Mission (Production):** Realizing the hidden risks in your preferred base images.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Awareness (SEEI)**

**The "Iceberg" of Code**
Your application code might only be 1,000 lines, but your Docker image contains millions of lines of Linux code (OpenSSL, glibc, Bash, etc.). Just because you wrote secure application code doesn't mean your _environment_ is secure.

**Why Scan?**
New vulnerabilities are discovered every day. An image that was "Safe" yesterday might be "Vulnerable" today because of a newly discovered bug in a common library (like Log4j or Heartbleed).

**The Negative Impact of NOT Scanning:**

- **Unknown Risk:** You are running a ticking time bomb in production.
- **Compliance Failure:** Many industries (Finance, Healthcare) require proof of regular vulnerability scanning.
- **Easy Exploits:** Hackers use automated tools to find outdated versions of software in public-facing containers.

### **Part 2: Practice - The CVE (PPP)**

**What is a CVE?**
CVE stands for **Common Vulnerabilities and Exposures**. It is a unique ID (e.g., `CVE-2023-1234`) assigned to a specific security hole.

**The CVSS Score:**
Scanners assign a score from 0.0 to 10.0 to each CVE:

- **Low (0-3.9):** Minor risk.
- **Medium (4-6.9):** Requires attention.
- **High (7-8.9):** Potentially dangerous.
- **Critical (9-10.0):** Stop everything and fix this NOW.

## 🧠 **Real-World Case Study: "The Log4j Crisis"**

- **Scenario:** In December 2021, a critical bug was found in a tiny Java library called `log4j`.
- **The Scale:** Millions of Docker images worldwide were suddenly vulnerable.
- **The Chaos:** Companies that didn't have a "Scan First" policy had no idea which of their 1,000 containers actually contained the library.
- **The Solution:** Companies with automated scanning identified every vulnerable image within minutes and patched them.
- **The Lesson:** You can't fix what you can't see. Scanning provides the "Manifest" of your risk.

## 🤔 **Reflective Questions**

1. If my app uses no libraries, can my container still have vulnerabilities? (Answer: Yes, in the OS layers like `bash` or `zlib`).
2. How often should you scan your production images?
3. What is the difference between a "Security Hole" in your code and a CVE in a library?

## 📚 **Further Reading**

- **NIST:** [The National Vulnerability Database](https://nvd.nist.gov/)
- **Docker Blog:** [Vulnerability scanning for Docker](https://www.docker.com/blog/vulnerability-scanning-for-docker/)

## 📝 **Mini Task (Production)**

1. Visit [Snyk.io](https://snyk.io/vuln) or a similar CVE database.
2. Search for `alpine` or `debian`.
3. Look at the most recent vulnerabilities.
4. Try to find a vulnerability with a "Critical" score.
5. Read what an attacker could do if they exploited that specific bug.
6. Reflect: Does "Alpine" really have fewer vulnerabilities than "Ubuntu"? Why?
