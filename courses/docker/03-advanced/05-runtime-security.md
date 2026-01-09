# 5. Runtime Security

## 🎯 Learning Goal

Catching bad behavior live.

## 🧠 Concept

Scanning catches static bad libs. Runtime Security catches "Wait, why is my Nginx process spawning a Shell?" (Reverse Shell Attack).

### Tools

- **Falco**: Behavioral monitoring. Rules engine.
  - Rule: "Shell spawned in container" -> Alert.
- **Seccomp (Secure Computing Mode)**: Firewall for Syscalls.
  - Whitelist: "This container can only call `read`, `write`, `exit`".
  - If it calls `mount`, kernel blocks it.
- **AppArmor / SELinux**: Profile-based file/resource blocking.

## 💻 Deep Dive

Docker enables a default Seccomp profile that blocks ~44 risky syscalls out of 300+.

## 🧩 Activity / Challenge

1.  This is mostly relevant for Kubernetes / Production environments.
2.  Understand that "Defense in Depth" is the strategy.

## 🔑 Key Takeaways

- Monitor unexpected processes and network connections.
