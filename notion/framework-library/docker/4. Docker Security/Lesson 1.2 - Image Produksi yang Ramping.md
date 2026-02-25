# 💡 DK04.1.2: Image Produksi yang Ramping

**Outline:**

- **The bloat (SEEI):** Understanding why "Heavy" images are a security risk.
- **The Solution (PPP):** Using Multi-Stage builds to create the smallest possible production image.
- **Your Mission (Production):** Stripping away the "Build Tools" to leave only the "Run Tools".

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The bloat (SEEI)**

**Attack Surface Area**
Every file, library, and tool in your image is a potential vulnerability. If your production image contains a compiler (gcc), a package manager (npm/pip), or a shell, an attacker can use those tools against you.

**The "Heavy" Image Problem:**

- **Slow Deploys:** Large images take longer to push/pull.
- **More Vulnerabilities:** A 1GB image likely has 10x more "Critical" vulnerabilities than a 50MB image.
- **Operational Noise:** Security scanners will flag things you don't even use.

**The Solution: Multi-Stage Builds**
You use one "Heavy" stage to build your code, and then COPY only the final binary or build artifacts into a "Light" stage for production.

### **Part 2: Practice - The Solution (PPP)**

**The Multi-Stage Recipe (Example with Go/Node):**

```dockerfile
# STAGE 1: The Builder (Large, has all tools)
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build # Creates a 'dist' folder

# STAGE 2: The Final Image (Lean, no tools)
FROM nginx:alpine
# Only copy the result from the 'builder' stage
COPY --from=builder /app/dist /usr/share/nginx/html
# No npm, no source code, only static files!
```

**Result:**

- Image size drops from ~400MB to ~20MB.
- `npm` is not present in production.
- Attackers cannot re-build or re-install packages.

## 🧠 **Real-World Case Study: "The Leftover SSH Key"**

- **Scenario:** A developer needed to pull a private dependency during build, so they copied their SSH key into the container.
- **The Mistake:** They deleted the key at the end of the Dockerfile: `RUN rm /root/.ssh/id_rsa`.
- **The Problem:** Because of Docker's layer system, the key is still "buried" in the previous layer. Anyone who pulls the image can extract the key.
- **The Fix:** Using a Multi-Stage build. The key only exists in the `builder` stage. It is **never** copied to the final stage.
- **The Lesson:** If it's not in the final stage, it's not in the image. Multi-stage builds are a physical barrier for secrets.

## 🤔 **Reflective Questions**

1. Why does a large image make an attacker's job easier?
2. If I use `RUN rm` in a Dockerfile, is the file actually gone from the total image size? (Answer: No, it's just hidden in the top layer).
3. How do you name a stage in a Multi-Stage build? (Answer: Using the `AS` keyword).

## 📚 **Further Reading**

- **Docker Orientation:** [Multi-stage builds](https://docs.docker.com/build/building/multi-stage/)
- **Guide:** [Best practices for production images](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)

## 📝 **Mini Task (Production)**

1. Take a project you have.
2. Build it normally and check the size: `docker images`.
3. Now, rewrite the Dockerfile into two stages (Build and Run).
4. Build again and compare the size.
5. Try to run `docker exec <final_image> ls`. Are the source files still there? What about the `node_modules` or build tools?
6. Explain why this image is now "Secure by Default".
