# 4. Installing Docker (Desktop vs Engine)

## 🎯 Learning Goal

Get a working Docker environment on your specific Operating System.

## 🧠 Concept

Because Docker containers share the host Linux kernel, the installation differs significantly depending on whether your host is Linux, Windows, or macOS.

### 1. Docker Engine (Native Linux)

- **Where**: Ubuntu, Debian, CentOS, Fedora servers.
- **How**: Installs `dockerd` directly on the host OS.
- **Performance**: Maximum. No VM overhead.
- **Use Case**: Production Servers, Linux Developer workstations.

### 2. Docker Desktop (Windows & Mac)

- **The Challenge**: Windows and macOS do not have a Linux Kernel. Therefore, they _cannot_ run Linux containers natively.
- **The Solution**: Docker Desktop installs a lightweight Linux Virtual Machine (WSL2 on Windows, LinuxKit on Mac) in the background. The Docker Daemon runs _inside_ that hidden VM. The Docker CLI on your host talks to the Daemon in the VM.
- **Features**: GUI Dashboard, Kubernetes built-in, Extensions.
- **Licensing**: **Important**. Docker Desktop requires a paid subscription for large companies (Enterprise/Business). Docker Engine (Linux) is free/open-source.

## 💻 Implementation: Windows (WSL 2)

This is the modern standard for Windows.

1.  **Enable WSL 2**: Windows Subsystem for Linux. It runs a real Linux kernel inside Windows.
2.  **Install Docker Desktop**: Select "Use WSL 2 based engine".
3.  **Result**: You can run `docker` commands from PowerShell OR from your Ubuntu WSL terminal. They talk to the same Daemon.

## 💻 Implementation: Linux (Ubuntu)

Do not use `snap` or `apt install docker.io` (often outdated). Use the official repository:

```bash
# 1. Remove old versions
sudo apt-get remove docker docker-engine docker.io containerd runc

# 2. Add Docker's official GPG key & Repo
# ... (Follow official docs)

# 3. Install
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

## 🧩 Activity / Challenge

1.  **Verification**: Run `docker run hello-world`.
2.  **Output Analysis**:
    - "Unable to find image 'hello-world:latest' locally" -> Daemon checked cache, missed.
    - "Pulling from library/hello-world" -> Daemon hit Docker Hub.
    - "Hello from Docker!" -> Container started, logic ran, printed to stdout, container exited.

## 🔑 Key Takeaways

- On Windows/Mac, you are technically using a VM, but Docker abstracts it away so it feels native.
- Always ensure your Virtualization features (BIOS/UEFI) are enabled.
