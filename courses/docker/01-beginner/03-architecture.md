# 3. Architecture: Docker and OCI

## 🎯 Learning Goal

Understand that "Docker" is a platform composed of several tools, and understand the standardization efforts (OCI) that prevent lock-in.

## 🧠 Concept

### The Docker Engine

When you install "Docker", you are actually installing a client-server application:

1.  **Docker Daemon (`dockerd`)**: The background service (Server). It manages Docker objects (images, containers, networks, volumes). It listens for API requests.
2.  **Docker Client (`docker` CLI)**: The command-line tool you use. It sends REST API requests to the Daemon.
    - _Note_: On Linux, the client talks to the daemon via a Unix Socket (`/var/run/docker.sock`).
3.  **Docker Desktop**: A GUI wrapper that bundles the Engine, CLI, and a Linux VM (on Windows/Mac) to host the daemon.

### The OCI (Open Container Initiative)

In the early days, Docker was the only game in town. To prevent monopoly and allow other tools to exist, the industry created a standard: **OCI**.

- **Image Spec**: "What does a container image look like file-by-file?" (A standard tarball format).
- **Runtime Spec**: "How do you unpack that image and run it?" (Standard arguments for the kernel).

**Significance**:

- Docker produces OCI-compliant images.
- This means an image built with Docker can run on Kubernetes, Podman, or AWS Fargate without Docker being installed there.

## 💻 Deep Dive: The Container Lifecycle

When you type `docker run nginx`:

1.  **Client**: Parses command. Sends API request `POST /containers/create` to Daemon.
2.  **Daemon**: Checks if `nginx` image exists locally.
    - If no -> Pulls from Registry (Docker Hub).
3.  **Daemon**: Creates the container environment (namespaces, cgroups).
4.  **Runtime (runc)**: This is the actual low-level tool (OCI compliant) that interfaces with the Linux Kernel to spawn the process.
5.  **Process Starts**: Nginx begins listening on port 80.

## 🧩 Activity / Challenge

1.  On your machine, run `docker version`.
2.  Notice the output is split into **Client** and **Server**.
3.  If the Server section is missing or says "Cannot connect", it means the Daemon is down, but the CLI binary is fine.

## 🔑 Key Takeaways

- Docker is just a high-level manager.
- **runc** is the low-level worker.
- **OCI** ensures your skills and images are portable across the entire cloud ecosystem, not just Docker.
