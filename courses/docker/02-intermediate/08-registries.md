# 8. Working with Registries (Push/Pull)

## 🎯 Learning Goal

Sharing your creation.

## 🧠 Concept

**Registry**: A server storing named images.

- **Docker Hub**: Public default.
- **Private**: AWS ECR, Google Artifact Registry, GitHub Container Registry (GHCR).

## 💻 Implementation

### 1. Tagging for Remote

To push to a registry, the image name **must** include the registry URL (unless it's Docker Hub).

```bash
# Docker Hub
docker tag my-app:v1 bjang/my-app:v1
docker push bjang/my-app:v1

# Private Registry (e.g., AWS)
docker tag my-app:v1 123456.dkr.ecr.us-east-1.amazonaws.com/my-app:v1
docker push ...
```

### 2. Authentication

```bash
docker login
# or
docker login ghcr.io -u USER -p TOKEN
```

Credentials are stored in `~/.docker/config.json`.

## 🧩 Activity / Challenge

1.  Create a repo on Docker Hub.
2.  Tag your `hello-world` image.
3.  Push it.
4.  Ask a friend to `docker pull bjang/hello-world`.

## 🔑 Key Takeaways

- The naming convention `registry/namespace/image:tag` tells Docker where to push.
