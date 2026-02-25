# 💡 DK04.4.2: Praktik Terbaik Keamanan Host

**Outline:**

- **The Bedrock (SEEI):** Realizing that a "Secure Container" on a "Vulnerable Host" is still unsafe.
- **The Hardening (PPP):** Applying OS-level security (SELinux, AppArmor, and Updates) to protect Docker.
- **Your Mission (Production):** Auditing a host server's readiness for production workloads.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Bedrock (SEEI)**

**Docker is NOT a sandbox**
While Docker provides isolation, it shares the same Kernel as the host. If the host kernel is outdated or the OS is unpatched, a container escape becomes much easier.

**The "Defense in Depth" Strategy:**
You shouldn't rely on just one layer of security. If an attacker gets past your Dockerfile security, they should meet the Host OS security. If they get past that, they should meet the Network security.

**The Major Risks:**

- **Outdated Kernel:** Susceptible to "Dirty Cow" or similar privilege escalation exploits.
- **Open Ports:** Exposing more than just 80/443 (e.g., SSH, RDP, or DB ports) to the internet.
- **Insecure Services:** Running other software on the same host (like an unpatched Mail server) that can be used as a stepping stone.

### **Part 2: Practice - The Hardening (PPP)**

**Host Security Checklist:**

1. **Keep it Minimal:** Use a "Container Optimized OS" (like Fedora CoreOS or Talos) that has no extra fluff.
2. **Enable Mandatory Access Control (MAC):**
   - **AppArmor** (Ubuntu/Debian) or **SELinux** (RHEL/CentOS). These tools limit what processes can do, even if they are root.
3. **Regular Patching:** Automate your security updates using tools like `unattended-upgrades`.
4. **Firewall (iptables/nftables):** Ensure that only necessary traffic reaches the Docker engine.

## 🧠 **Real-World Case Study: "The Kernel Exploit"**

- **Scenario:** A server was running an old version of Linux (Kernel 3.x) with Docker installed.
- **The Breach:** An attacker exploited a vulnerability in a web application inside a container.
- **The Escape:** Because the host kernel was old, the attacker used a known kernel exploit to "Escape" the container and gain root access to the physical hardware.
- **The Fix:** The company migrated to a modern, patched kernel and enabled SELinux in "Enforcing" mode.
- **The Lesson:** Docker security is a collaboration between the App developer and the System Administrator. You cannot have one without the other.

## 🤔 **Reflective Questions**

1. Why is a "Container Optimized OS" better than a standard Ubuntu server for Docker?
2. What is the difference between AppArmor and a standard Firewall?
3. If I patch the host OS, do I need to restart my containers? (Answer: Usually no, unless the Kernel was patched).

## 📚 **Further Reading**

- **Docker Orientation:** [Hardware and OS security](https://docs.docker.com/engine/security/#hardware-and-os-security)
- **CIS Benchmarks:** [Docker Benchmark](https://www.cisecurity.org/benchmark/docker)

## 📝 **Mini Task (Production)**

1. If you are on Linux, run `aa-status` to check if AppArmor is enabled.
2. If you are on a RedHat-base, run `getenforce` to check SELinux.
3. Search for "Docker Bench for Security." This is a script that audits your host.
4. Read the README of that project and list 3 things it checks for regarding the "Host Configuration."
5. Reflect: How many of those checks would your current machine pass?
