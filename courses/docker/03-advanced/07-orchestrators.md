# 7. Orchestrators: Swarm vs Kubernetes

## 🎯 Learning Goal

Managing clusters of Docker Hosts.

## 🧠 Concept

**Docker Swarm**:

- Built-in to Docker. Simple.
- Uses `docker-compose.yml` (Stack) format.
- Good for small teams/clusters.
- Feature frozen (Legacy feeling).

**Kubernetes (K8s)**:

- Google's child. The Industry Standard.
- Complex. Steep learning curve. Verbose YAML.
- Massive ecosystem (Helm, CRDs).
- Decoupled from Docker (Runs Containerd/CRI-O).

## 💻 Implementation

K8s doesn't use `docker run`. It uses Pods. A Pod dictates the container execution.

## 🧩 Activity / Challenge

1.  Don't learn K8s until you master Docker.
2.  K8s creates complexity you might not need for a simple startup.

## 🔑 Key Takeaways

- If you know Docker, Swarm is easy. K8s is a new career path.
