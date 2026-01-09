# 5. Writing Dockerfiles

## 🎯 Learning Goal

Automate the creation of custom images. A `Dockerfile` is a recipe text file.

## 🧠 Concept

Instructions in a Dockerfile are capitalized conventions (`FROM`, `RUN`, `COPY`).
They run **sequentially**. Each instruction creates a new **Layer**.

## 💻 Implementation: A Node.js Example

```dockerfile
# 1. Base Image (The OS + Runtime)
FROM node:18-alpine

# 2. Working Directory (Where are we inside the image?)
WORKDIR /app

# 3. Copy Dependency definitions
COPY package.json package-lock.json ./

# 4. Install Dependencies
RUN npm install

# 5. Copy Source Code
COPY . .

# 6. Documentation (Optional, but good practice)
EXPOSE 3000

# 7. Start Command (The process to run)
CMD ["node", "src/index.js"]
```

### Explaining the Steps

1.  **FROM**: Always start here.
2.  **WORKDIR**: Like `cd` but persistent.
3.  **COPY**: `HostPath` -> `ContainerPath`.
4.  **RUN**: Executes command during **Build** time (e.g., compile code, install libs).
5.  **CMD**: Executes command during **Run** time (Start the server). There can be only one CMD.

## 💻 Building it

```bash
docker build -t my-node-app:v1 .
```

- `-t`: Tag (Name).
- `.`: Context (Where to look for files).

## 🧩 Activity / Challenge

1.  Create a file `main.py` with `print("Hello from Docker")`.
2.  Write a Dockerfile:
    - FROM python:alpine
    - COPY main.py .
    - CMD ["python", "main.py"]
3.  Build and Run.

## 🔑 Key Takeaways

- Order matters (see next lesson on Caching).
- `RUN` is for building. `CMD` is for starting.
