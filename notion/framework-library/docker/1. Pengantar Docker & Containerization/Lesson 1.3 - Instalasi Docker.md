# 💡 DK01.3: Instalasi Docker

**Outline:**

- **The Setup (SEEI):** Choosing the right version of Docker (Desktop vs. Engine) for your OS.
- **The Verification (PPP):** Confirming a successful installation using the command line.
- **Your Mission (Production):** Checking your system's readiness for containerization.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Setup (SEEI)**

**Docker Versions**
Depending on your operating system, the installation method differs slightly:

1. **Docker Desktop (Windows & macOS):** A full-featured application that includes Docker Engine, a GUI, and Kubernetes support. It uses a lightweight VM (WSL2 on Windows) to run the Linux containers.
2. **Docker Engine (Linux):** The native installation for Linux distributions (Ubuntu, CentOS, etc.). It runs directly on the kernel.

**Minimum Requirements**

- **Windows:** WSL2 (Windows Subsystem for Linux) is highly recommended.
- **Mac:** Apple Silicon (M1/M2/M3) or Intel.
- **Linux:** 64-bit kernel and support for virtualization.

### **Part 2: Practice - The Verification (PPP)**

**The Verification Checklist**
Once installed, open your terminal (Command Prompt, PowerShell, or Bash) and run these commands to ensure everything is working:

1. **Check Version:**
   ```bash
   docker --version
   ```
2. **Check System Info:**
   ```bash
   docker info
   ```
   _Note: If you get a "permission denied" or "daemon not running" error, ensure Docker Desktop is started or your user is in the `docker` group (on Linux)._

## 🧠 **Real-World Case Study: "The WSL2 Revolution"**

- **Before (Docker on Windows):**
  Windows developers had to use VirtualBox or the "Hyper-V" backend, which was clunky and slow.
- **After (WSL2):**
  Docker now integrates directly with Linux running alongside Windows via WSL2.
- **The Result:** Near-native Linux performance on Windows. Files are shared seamlessly, and commands are fast.

## 🤔 **Reflective Questions**

1. Why does Docker Desktop need a Virtual Machine to run on Windows?
2. What is the difference between `docker --version` and `docker info`?
3. What should you do if the Docker daemon is not running?

## 📚 **Further Reading**

- **Installation Guides:** [Download Docker Desktop](https://www.docker.com/products/docker-desktop)
- **Linux Setup:** [Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)

## 📝 **Mini Task (Production)**

1. Install Docker on your primary machine.
2. Run `docker info` and identify:
   - Your Kernel Version.
   - The total amount of CPUs/Memory available to Docker.
3. Take a screenshot of the output (or copy the text) to prove your environment is ready.
