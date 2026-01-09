# 2. Data Persistence (Volume vs Bind Mounts)

## 🎯 Learning Goal

Learn how to make data survive the death of a container.

## 🧠 Concept

To persist data, we must punch a hole through the container's filesystem out to the Host's filesystem. There are two main ways to do this.

### 1. Docker Volumes (The "Managed" Way)

- **Concept**: Docker creates a folder somewhere on your host (e.g., `/var/lib/docker/volumes/my-vol/_data`). You don't care exactly where. You just refer to it by name.
- **Pros**: Managed by Docker CLI. Easy to back up. High performance. Works on Linux/Windows/Mac seamlessly.
- **Use Case**: Database storage (`/var/lib/mysql`).

### 2. Bind Mounts (The "Direct" Way)

- **Concept**: You map a specific path on your Host (e.g., `C:\Users\Me\Project`) to a path in the Container.
- **Pros**: You can see and edit files on your host, and they instantly change inside the container.
- **Use Case**: **Code Development**. You edit source code in VS Code, and the container sees the changes immediately.

## 💻 Implementation

### Using Volumes

```bash
# 1. Create a named volume
docker volume create my-db-data

# 2. Mount it (-v VolumeName:ContainerPath)
docker run -d \
  -v my-db-data:/var/lib/postgresql/data \
  postgres
```

Now, even if you `docker rm` the postgres container, `my-db-data` remains safe. Next time, mount it to a new container, and your data is back.

### Using Bind Mounts

```bash
# -v HostPath:ContainerPath
docker run -d \
  -v $(pwd)/src:/app/src \
  node:18-alpine start-app.sh
```

- `$(pwd)` is "Print Working Directory" (Current folder).
- Any change in `./src` on your laptop is reflected in `/app/src` inside.

## 🧩 Activity / Challenge

1.  **Volume Lab**:
    - Run an Nginx container with a volume mounting to `/usr/share/nginx/html`.
    - Create an `index.html` inside.
    - Destroy container.
    - Start new Nginx with same volume.
    - Observe unmodified `index.html` is still there.

## 🔑 Key Takeaways

- **Production**: Use Volumes (Databases, State).
- **Development**: Use Bind Mounts (Source Code).
