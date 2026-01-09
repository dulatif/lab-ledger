# 5. Running Containers (`docker run` basics)

## 🎯 Learning Goal

Master the most important command in the ecosystem: `docker run`. This command launches a new container from an image.

## 🧠 Concept

`docker run` is actually a combination of three commands:

1.  `docker pull` (download image if missing).
2.  `docker create` (set up the container layer).
3.  `docker start` (execute the process).

## 💻 Implementation: Anatomy of a Command

```bash
docker run -d -p 8080:80 --name my-web-server nginx:latest
```

Let's dissect every flag. This is crucial.

### 1. Detached Mode (`-d` or `--detach`)

- **Default**: Docker attaches your terminal to the container's output. If you close the terminal, the container dies (usually).
- **-d**: Runs in background. Prints the Container ID and returns control to you.
- **When to use**: Web servers, databases, long-running services.

### 2. Port Mapping (`-p HOST:CONTAINER`)

- **The concept**: Containers execute in their own network namespace. Their ports are **closed** to the outside world by default.
- **-p 8080:80**: "Take traffic hitting port **8080** on my laptop (Host) and forward it to port **80** inside the container."
- **Result**: You open `localhost:8080` in Chrome -> Docker forwards to Nginx port 80.

### 3. Naming (`--name`)

- **Default**: Docker assigns a random funny name (e.g., `sad_turing`, `hopeful_wozniak`).
- **--name**: Assigns a static name. Easier to reference later (`dockerstop my-web-server`).

### 4. The Image (`nginx:latest`)

- The template to use. `nginx` is the image name, `latest` is the tag (version).

## 🧩 Activity / Challenge

1.  Run the command above: `docker run -d -p 8080:80 --name my-web-server nginx`.
2.  Open Browser: `http://localhost:8080`. You should see "Welcome to nginx!".
3.  Check status: `docker ps`.
4.  Stop it: `docker stop my-web-server`.
5.  Remove it: `docker rm my-web-server`. (You cannot reuse the name until you remove the old container).

## 🔑 Key Takeaways

- Port mapping is where 90% of beginner errors occur. Remember: **Host Port : Container Port**.
- The container is ephemeral. If you remove it, any file you wrote inside it is gone (unless using Volumes - see Intermediate course).
