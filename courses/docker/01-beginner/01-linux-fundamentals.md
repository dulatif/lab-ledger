# 1. Linux Fundamentals (Shell, Users, Permissions)

## 🎯 Learning Goal

To communicate effectively with the Operating System (OS) that hosts your containers. Since Docker is natively a Linux tool, understanding basic Linux concepts is non-negotiable for effective troubleshooting and usage.

## 🧠 Concept

Docker containers share the host's kernel but have their own user space. Most Docker images are built on Linux distributions (Alpine, Debian, Ubuntu). Therefore, "speaking Docker" effectively means "speaking Linux".

### Key Concepts:

1.  **The Shell (Bash/Sh)**: The command-line interface where you type commands. It acts as a translator between you and the kernel.
2.  **Users & Groups**: Linux is a multi-user system.
    - **Root (UID 0)**: The superuser with unlimited power. Default user in many containers (which is a security risk).
    - **Standard User**: Has restricted permissions.
3.  **Permissions (chmod/chown)**: Reliability and security depend on who can read/write/execute files.
    - `r` (Read), `w` (Write), `x` (Execute).
4.  **Processes**: Every running program is a process with a PID (Process ID). A container is essentially an isolated process.

## 💻 Implementation & Deep Dive

### 1. Essential Commands

You will run these _inside_ containers constantly to debug.

| Command    | Purpose                                         | Example                            |
| :--------- | :---------------------------------------------- | :--------------------------------- |
| `ls -la`   | List all files (including hidden) with details. | Checking if a config file exists.  |
| `cd /path` | Change Directory.                               | Moving to `/app` directory.        |
| `pwd`      | Print Working Directory.                        | "Where am I currently?"            |
| `cat file` | Display file content.                           | Reading logs or configs.           |
| `ps aux`   | List all running processes.                     | "Is my server actually running?"   |
| `env`      | List environment variables.                     | Debugging configuration injection. |

### 2. Permissions in Depth

When you see `drwxr-xr-x`, it breaks down to:

- `d`: Directory.
- `rwx` (Owner): Read, Write, Execute.
- `r-x` (Group): Read, Execute.
- `r-x` (Others): Read, Execute.

**Common Scenario**: Your app crashes with `EACCES: permission denied`.

- **Cause**: The Node.js app is running as user `node`, but trying to write to a folder owned by `root`.
- **Fix**: `chown -R node:node /app/data`.

### 3. Redirection and Piping

- `|` (Pipe): Takes the output of the left command and feeds it as input to the right.
  - `ps aux | grep node`: Find only Node processes.
- `>` (Redirect): Save output to a file.
  - `echo "hello" > log.txt`.

## 🧩 Activity / Challenge

1.  Open your terminal.
2.  Create a file named `script.sh` with content `echo "Hello World"`.
3.  Try to run it: `./script.sh`. It will fail (`Permission denied`).
4.  Fix it: `chmod +x script.sh`.
5.  Run it again.

## 🔑 Key Takeaways

- **Command Line is King**: GUIs (like Docker Desktop Dashboard) are helpful, but the CLI gives you truth and power.
- **Root is Dangerous**: Understanding users helps you build secure containers that don't run as root.
- **Everything is a File**: In Linux, even hardware configuration is often represented as a file.
