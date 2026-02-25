# 💡 DK03.2: Instruksi-Instruksi Esensial

**Outline:**

- **The Keywords (SEEI):** Mastering the "Big 6" instructions: `FROM`, `RUN`, `COPY`, `WORKDIR`, `CMD`, and `EXPOSE`.
- **The Syntax rules (PPP):** Understanding the difference between Build-time (`RUN`) and Runtime (`CMD`) commands.
- **Your Mission (Production):** Building a comprehensive Dockerfile for a static website.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Keywords (SEEI)**

To build a professional image, you need these essential building blocks:

1. **`FROM`**: The starting point. Every Dockerfile must start with `FROM` (except in rare cases).
2. **`RUN`**: Executes commands inside the container _during the build process_. Used for installing packages.
3. **`COPY`**: Copies files/folders from your local machine into the image.
4. **`WORKDIR`**: Sets the current working directory (like `cd`). It's better than using multiple `RUN cd ...`.
5. **`CMD`**: The default command that runs _when the container starts_.
6. **`EXPOSE`**: Documents which port the application is listening on.

### **Part 2: Practice - The Syntax rules (PPP)**

**`RUN` vs `CMD` (Common Confusion):**

- **`RUN npm install`**: Happens once when you build the image. It "bakes" the dependencies into the layers.
- **`CMD ["npm", "start"]`**: Happens every time you start a container. It doesn't "bake" anything; it just starts the process.

**Best Practice: The `JSON Array` Syntax**
Always prefer the array syntax for `CMD` and `ENTRYPOINT`:

- ✅ `CMD ["node", "app.js"]` (Exec mode - handles signals correctly)
- ❌ `CMD node app.js` (Shell mode - wraps the process in `/bin/sh -c`)

## 🧠 **Real-World Case Study: "The Clean Dockerfile"**

- **Poor Dockerfile:**
  ```dockerfile
  FROM ubuntu
  RUN apt-get update
  RUN apt-get install -y nginx
  COPY . /
  CMD nginx -g "daemon off;"
  ```
- **Professional Dockerfile:**
  ```dockerfile
  FROM nginx:alpine
  WORKDIR /usr/share/nginx/html
  COPY index.html .
  # No need for RUN or CMD because the base image already has them optimized!
  ```
- **The Lesson:** Always look for specialized base images (like `nginx:alpine` or `node:18`). They are smaller, more secure, and require less custom configuration.

## 🤔 **Reflective Questions**

1. What happens if you have two `CMD` instructions in one Dockerfile? (Answer: Only the last one counts).
2. What is the difference between `COPY` and `ADD`? (Hint: `ADD` can download URLs and extract `.tar` files, but `COPY` is generally preferred for its simplicity).
3. Why is it important to use `WORKDIR` instead of many `RUN cd` commands?

## 📚 **Further Reading**

- **Docker orientation:** [Best practices for writing Dockerfiles](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
- **Cheat Sheet:** [Dockerfile Instructions list](https://docs.docker.com/engine/reference/builder/)

## 📝 **Mini Task (Production)**

Create a Dockerfile for a simple custom static page:

1. Create an `index.html` with your name.
2. Create a `Dockerfile`:
   - `FROM nginx:alpine`
   - `WORKDIR /usr/share/nginx/html`
   - `COPY index.html .`
   - `EXPOSE 80`
3. Build it: `docker build -t my-website .`
4. Run it: `docker run -d -p 8080:80 my-website`.
5. Verify it at `localhost:8080`.
