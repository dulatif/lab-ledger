# 3. Container Security Basics

## 🎯 Learning Goal

Don't get hacked.

## 🧠 Concept

Containers are **NOT** as secure as VMs. They share the kernel. A kernel exploit (Panic) crashes all containers + host.

### 1. The Root Problem

By default, Docker containers run as `root`.
If a hacker escapes the container (e.g., via a runtime vulnerability), they are `root` on your host. Game Over.
**Best Practice**: Create a user in Dockerfile and switch to it.

```dockerfile
RUN adduser -D appuser
USER appuser
CMD ["./app"]
```

### 2. Capabilities

Linux `root` is actually a bundle of ~40 Capabilities (`CAP_CHOWN`, `CAP_NET_ADMIN`, `CAP_SYS_BOOT`, etc.).
Docker drops most by default (you can't reboot host from container).

- `--cap-add`: Give more power (Dangerous).
- `--cap-drop`: Remove power (Hardening).
- `--privileged`: **EXTREME DANGER**. Gives all capabilities and device access. Basically removes isolation. Only use for Docker-in-Docker.

### 3. Rootless Docker

A mode where the Docker Daemon itself runs as a normal user. Mitigates the daemon-as-root risk.

## 🧩 Activity / Challenge

1.  Try to change system time inside a container. It fails. (`CAP_SYS_TIME` is dropped).
2.  Run with `--privileged` and try again. It works (and changes Host time too!). Scary.

## 🔑 Key Takeaways

- Never run as root.
- Never use `--privileged` in production.
