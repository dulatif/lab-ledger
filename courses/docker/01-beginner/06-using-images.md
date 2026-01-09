# 6. Using 3rd Party Images

## 🎯 Learning Goal

Leverage the massive ecosystem of the Docker Hub. Don't reinvent the wheel.

## 🧠 Concept

**Docker Hub (hub.docker.com)** is the GitHub of container images. It hosts millions of images.

- **Official Images**: Maintained by Docker and the upstream software vendor (e.g., `python`, `node`, `mongo`). These are secure, small, and optimized.
- **Community Images**: Uploaded by users (e.g., `bjang/my-app`).

## 💻 Implementation & Deep Dive

### 1. Image Versions (Tags)

An image name usually follows: `repository:tag`.

- `node:18`: A specific major version. Recommended for stability.
- `node:latest`: The absolute newest version. **Avoid in production** because it changes unexpectedly.
- `node:18-alpine`: The **Alpine** variant.
  - **Concept**: Alpine Linux is a tiny distro (5MB).
  - **Standard**: Based on Debian/Ubuntu (100MB+).
  - **Why use Alpine?**: Faster downloads, smaller attack surface (security).
  - **Gotcha**: Alpine uses `musl` instead of `glibc`. Some compiled binaries (C++) might not work without recompilation.

### 2. Running a Database (Postgres)

Databases are complex to install manually. With Docker, it's one command.

```bash
docker run -d \
  --name my-db \
  -e POSTGRES_PASSWORD=secret \
  -p 5432:5432 \
  postgres:15-alpine
```

- `-e`: Environment Variables. Used to configure the software inside. The Postgres image _requires_ a password var to start.

### 3. Interactive Containers (`-it`)

Sometimes you want to go _inside_ a container to explore or run a Python shell.

- `-i` (Interactive): Keep Stdin open.
- `-t` (TTY): Allocate a pseudo-terminal (a text window).

```bash
docker run -it python:3.9
# You are now inside the Python REPL
>>> print("Hello")
```

## 🧩 Activity / Challenge

1.  Go to Docker Hub. Search for `redis`.
2.  Read the "How to use this image" section. It documents the available Environment Variables.
3.  Run a Redis instance.
4.  Connect to it using another container: `docker run -it --network host redis redis-cli`.

## 🔑 Key Takeaways

- Always check the **Official Image** documentation on Hub to know which Environment Variables (`-e`) are needed.
- Prefer specific tags (`:14.2`) over `:latest` for reproducibility.
- Prefer Alpine tags (`:alpine`) for size, unless you hit compatibility issues.
