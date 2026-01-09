# 4. CLI Mastery: Managing Containers/Images

## 🎯 Learning Goal

Go beyond `run` and `stop`. Manage disk space and debug.

## 💻 Deep Dive: Essential Commands

### 1. Debugging

- `docker logs <id>`: View stdout/stderr.
  - `-f`: Follow (tail) the logs in real-time.
  - `--tail 100`: Only last 100 lines.
- `docker exec -it <id> <cmd>`: Run a command inside a running container.
  - `docker exec -it my-db bash` (or `sh` if bash isn't installed).
- `docker inspect <id>`: Returns a massive JSON of metadata (IP, Mounts, Environment, State). Use `grep` to find what you need.

### 2. Cleanup (Pruning)

Docker never deletes anything automatically. Disk usage grows indefinitely.

- `docker system df`: Show disk usage.
- `docker container prune`: Delete all Stopped containers.
- `docker image prune`: Delete "Dangling" images (images with no tag, usually leftovers from failed builds).
- `docker system prune -a`: **Nuclear Option**. Deletes all stopped containers, unused networks, and _all_ images not used by a running container.

### 3. Monitoring

- `docker stats`: Like "Task Manager" for containers. CPU%, Mem Usage, Net I/O. Live update.

## 🧩 Activity / Challenge

1.  Run `docker run -d nginx`.
2.  Inspect it: `docker inspect <id>`. Find the `IPAddress` field.
3.  Check logs: `docker logs <id>`.
4.  Kill it.
5.  Run `docker system prune` to clean it up.

## 🔑 Key Takeaways

- If your disk is full, `docker system prune` is your friend.
- `docker exec` is your primary debugging tool.
