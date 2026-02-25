# 💡 DK04.3.2: Rahasia Saat Build-Time

**Outline:**

- **The Paradox (SEEI):** Needing a secret to build the image (e.g., pulling private code) without leaving it in the image.
- **The Modern Way (PPP):** Using BuildKit's `--secret` mount.
- **Your Mission (Production):** Securely pulling a private package during an image build.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Paradox (SEEI)**

**The "Dirty" Build Problem**
Sometimes your image build process _needs_ a secret. For example:

- Pulling a private NPM/NuGet package.
- Fetching a secure token from an internal vault.
- Authenticating with a private Git repository via SSH.

**The Legacy (Insecure) Way:**
Older Docker versions forced developers to use `ARG` or `COPY`ing keys. As we learned in Lesson 1.4, this leaves "Footprints" in the image layers that can never be truly erased.

### **Part 2: Practice - The Modern Way (PPP)**

**BuildKit Secrets (`--mount=type=secret`)**
This is the only 100% secure way to use a secret during a build. The secret is mounted as a temporary file that exists **Only** while that specific `RUN` command is executing. It never becomes part of a layer.

**The Implementation:**

1. **The Dockerfile:**

   ```dockerfile
   # syntax=docker/dockerfile:1
   FROM node:alpine
   WORKDIR /app
   # Mount the secret to a temporary location
   RUN --mount=type=secret,id=npm_token \
       NPM_TOKEN=$(cat /run/secrets/npm_token) npm install
   ```

2. **The Command:**
   ```bash
   # Pass the local file to the builder
   docker build --secret id=npm_token,src=./my_local_token.txt .
   ```

**Why it wins:**

- **Zero Metadata:** No `ARG` or `ENV` visible in `docker inspect`.
- **Zero Layer Trace:** The file `/run/secrets/npm_token` vanishes the instant the `RUN` command finishes.

## 🧠 **Real-World Case Study: "The Leaked NPM Token"**

- **Scenario:** A developer used `ARG NPM_TOKEN` to install private company libraries.
- **The Discovery:** A junior developer ran `docker history my-app` and saw the full token string in the command description of one of the layers.
- **The Risk:** That token could be used to delete every library in the company's private registry.
- **The Fix:** The team enabled BuildKit and switched to `--mount=type=secret`.
- **The Result:** The new image history showed `RUN --mount=type=secret...`, but the actual token value was invisible.
- **The Lesson:** If you can see it in `docker history`, you've already lost. Use Mounts.

## 🤔 **Reflective Questions**

1. What is BuildKit and why is it required for secure secrets?
2. If you don't have BuildKit, what is the "Multi-stage" workaround for build secrets? (Answer: Use a builder stage with the secret, and NEVER copy it to the final stage).
3. Can a build secret be a multi-line file (like an SSH Private Key)?

## 📚 **Further Reading**

- **Docker Documentation:** [Building with secrets](https://docs.docker.com/build/building/secrets/)
- **Guide:** [Using SSH mounts during build](https://docs.docker.com/build/building/secrets/#ssh-mounts)

## 📝 **Mini Task (Production)**

1. Create a file `build_token.txt` with dummy text.
2. Write a Dockerfile that tries to `cat` a secret from `/run/secrets/my_token`.
3. Try to build it normally (`docker build .`). It should fail.
4. Now build it with the `--secret` flag.
5. Once built, run `docker history <image>` and try to find your "dummy text".
6. Why is this more secure than using `COPY build_token.txt /tmp/`?
