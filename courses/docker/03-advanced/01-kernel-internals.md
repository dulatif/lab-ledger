# 1. Kernel Internals: Cgroups and Namespaces

## 🎯 Learning Goal

Demystify the "Magic". Understand that Docker is just a fancy wrapper around Linux primitives.

## 🧠 Concept

When you ask for a container, Docker asks the Kernel for two main things.

### 1. Namespaces (Isolation - "What you can See")

Namespaces partition Kernel resources such that one set of processes sees one set of resources, while another set of processes sees a different set.

- **PID Namespace**: You are PID 1 inside the container. You cannot see PIDs from the host or other containers.
- **MNT (Mount) Namespace**: You see a different root filesystem (`/`).
- **NET Namespace**: You have your own `eth0` interface and IP.
- **UTS Namespace**: You have your own Hostname.
- **USER Namespace**: You can be `root` inside buffer, but `nobody` outside (Complex, but powerful).

### 2. Control Groups (Cgroups) (Limitations - "What you can Use")

Cgroups limit, account for, and isolate the resource usage (CPU, Memory, Disk I/O, Network) of a collection of processes.

- "Container A gets max 512MB RAM".
- "Container B gets 20% CPU shares".
- If Container A exceeds RAM -> OOM Killer (Out of Memory) kills it.

## 💻 Deep Dive

You can see cgroups on your Linux machine at `/sys/fs/cgroup`.
Docker creates a cgroup folder for each container.

## 🧩 Activity / Challenge

1.  Run a container: `docker run -d --name test nginx`.
2.  On Host, run `ps aux | grep nginx`. You will see the process ID (e.g., 12345).
3.  Inside container, `docker exec test ps aux`. You will see PID 1.
4.  This is the Namespace magic. Same process, different PIDs depending on perspective.

## 🔑 Key Takeaways

- There is no "Container" object in the Kernel. Just Namespaces and Cgroups applied to a Process.
