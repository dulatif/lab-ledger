# 💡 DK01.1: Kenapa Perlu Docker?

**Outline:**

- **The Problem (SEEI):** The "It works on my machine" phenomenon and environment inconsistency.
- **The Solution (PPP):** How Docker provides a portable, consistent environment for your applications.
- **Your Mission (Production):** Understanding why we choose Docker over traditional Virtual Machines.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Problem (SEEI)**

**The Conflict: Development vs. Production**
Every developer has faced the nightmare where code works perfectly on their laptop but crashes in production. This is usually caused by "Environmental Drift"—differences in OS versions, libraries, or configuration files between machines.

**The Negative Impact**

- **Wasted Time:** Developers spend hours debugging environment issues instead of writing code.
- **"It Works on My Machine":** This phrase becomes a barrier to collaboration and deployment.
- **Scaling Hell:** Hard to replicate complex environments quickly for new team members or scaling servers.

**Example of the Problem**
Imagine a Node.js app that needs version 18.x but the server only has version 14.x. Or an app that works on Windows but fails on Linux due to file path differences.

### **Part 2: Practice - The Solution (PPP)**

**The Technique: Containerization**
Docker solves this by "packaging" the application along with all its dependencies into a single unit called a **Container**.

1. **Isolation:** Each container runs in its own isolated environment.
2. **Portability:** If it runs in a Docker container on your laptop, it _will_ run exactly the same way on any server that has Docker installed.
3. **Efficiency:** Containers share the host OS kernel, making them much lighter and faster than traditional Virtual Machines (VMs).

## 🧠 **Real-World Case Study: "VM vs. Container"**

- **Before (Virtual Machines):**
  Each VM needs a full Guest OS (GBs of size), taking minutes to boot. High resource overhead.
- **After (Docker Containers):**
  Containers share the Host OS kernel. They are small (MBs), boot in seconds, and use minimal resources.
- **The Result:** We can run dozens of containers on a single server that might only support 3-4 VMs.

## 🤔 **Reflective Questions**

1. Why is "Portability" considered the most important feature of Docker?
2. What are the key differences between a Container and a Virtual Machine?
3. How does Docker help in a Microservices architecture?

## 📚 **Further Reading**

- **Docker Docs:** [What is a Container?](https://www.docker.com/resources/what-container)
- **Article:** [Docker vs. VM: What's the Difference?](https://www.atlassian.com/microservices/cloud-computing/docker-vs-vm)

## 📝 **Mini Task (Production)**

Think about a project you've worked on recently.

1. List 3 dependencies (e.g., Database, Runtime, Libraries) that caused issues during setup.
2. Explain how Docker could have simplified the onboarding process for a new developer joining that project.
