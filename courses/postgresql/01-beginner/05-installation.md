# Lesson 5: Installation & Setup

## Learning Goals

- Install PostgreSQL on your operating system.
- Understand the difference between the Client (`psql`) and the Server (`postgres`).
- Initialize a data cluster.

## 1. Installation Options

You can run PostgreSQL in three main ways:

1.  **Package Manager** (`apt`, `brew`, `choco`): Best for local development on your machine.
2.  **Docker**: Best for isolated testing and consistent environments.
3.  **Source Code**: For advanced users who need custom build flags (rarely needed).

### Option A: Docker (Recommended for ease)

If you have Docker installed, this is the fastest way to start.

```bash
docker run --name my-postgres -e POSTGRES_PASSWORD=mysecretpassword -p 5432:5432 -d postgres
```

This pulls the official image, sets the password, exposes port 5432, and runs it in the background.

### Option B: Linux (Ubuntu/Debian)

```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
```

This automatically installs the server, creates a `postgres` OS user, and starts the systemd service.

### Option C: macOS (Homebrew)

```bash
brew install postgresql@15
brew services start postgresql@15
```

### Option D: Windows

Download the graphical installer from [postgresql.org](https://www.postgresql.org/download/windows/) and follow the wizard. It includes pgAdmin (a GUI tool).

## 2. Client vs. Server

It is crucial to understand the two parts:

- **The Server (`postgres`)**: The background process that stores data on disk and listens for connections. It runs as a service/daemon.
- **The Client (`psql`)**: A command-line tool you type commands into. It sends those commands to the server over the network (localhost) and prints the results.

You can install the client without the server (e.g., on your laptop to connect to a cloud database).

## 3. Implementation: Initializing a Cluster (Manual)

If you installed via a package manager, this is usually done for you. But if you are building manually, you create the "Database Folder" using `initdb`.

```bash
# syntax: initdb -D [directory]
initdb -D /var/lib/postgres/data
```

This creates the folders and configuration files (`postgresql.conf`, `pg_hba.conf`) needed to run the server.

## Key Takeaways

- Use **Docker** for quick experiments; use **Package Managers** for stable local dev servers.
- The **Server** runs the database; **Clients** (like psql, DBeaver, TablePlus) just talk to it.
- Port **5432** is the default port.
