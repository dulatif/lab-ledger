# 1. Container Filesystem and Ephemeral Storage

## 🎯 Learning Goal

Understand the "Copy-on-Write" (CoW) nature of the container filesystem and why your data disappears when a container stops (or more accurately, when it is removed).

## 🧠 Concept

### Image Layers (Read-Only)

A Docker image is like a stack of pancakes (layers).

1.  Base Layer (e.g., Ubuntu).
2.  Add files Layer (e.g., Python installed).
3.  App Code Layer.

These layers are **Read-Only**. They cannot be changed once built. This allows Docker to share layers. If you have 10 containers using the same Ubuntu base, Docker stores that Ubuntu layer only once on disk.

### The Container Layer (Read-Write)

When you run `docker run`, Docker adds a thin **Read-Write layer** on top of the image stack.

- **Write**: When you create a new file, it goes into this thin layer.
- **Modify**: When you modify an existing file from the image, Docker performs a **Copy-on-Write**. It copies the file from the Read-Only layer up to the Read-Write layer, and then modifies the copy. The original remains untouched in the image.

### Ephemeral Nature

This top Read-Write layer is tied to the **lifecycle of the container**.

- **Stop/Start**: The layer persists. Data is safe.
- **Remove (`docker rm`)**: The layer is deleted. **All data written to it is lost forever.**

## 💻 Deep Dive: Where is it on disk?

On Linux, this is usually managed by the `overlay2` storage driver in `/var/lib/docker/overlay2`.
It's a complex directory of hashed folders representing the diffs.

## 🧩 Activity / Challenge

1.  Start a container: `docker run -it --name test-fs ubuntu`.
2.  Create a file: `echo "Important Data" > /data.txt`.
3.  Exit and remove: `docker rm test-fs`.
4.  Start a new one from same image: `docker run -it ubuntu`.
5.  Check file: `cat /data.txt`. It's gone.
6.  This demonstrates why you **never** store database files inside the container's writable layer.

## 🔑 Key Takeaways

- Containers are stateless by default.
- The writable layer is meant for temporary files (`/tmp`, logs).
- For persistent data, we need **Volumes** (Next Lesson).
