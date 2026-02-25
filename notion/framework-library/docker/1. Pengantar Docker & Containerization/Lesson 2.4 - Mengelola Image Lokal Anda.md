# 💡 DK02.4: Mengelola Image Lokal Anda

**Outline:**

- **The Inventory (SEEI):** How to see and understand the "Image Cache" on your machine.
- **The Housekeeping (PPP):** Listing, inspecting, and deleting images to save space.
- **Your Mission (Production):** Tidying up your local development environment.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Inventory (SEEI)**

**The Image Store**
Every time you run a container or pull an image, Docker stores it in a local cache. Over time, these can take up gigabytes of space.

**Key Information in an Image List:**

- **Repository:** The name of the image (e.g., `node`, `mysql`).
- **Tag:** The version (e.g., `18-alpine`, `latest`).
- **Image ID:** A unique hexadecimal string (the actual identifier).
- **Created:** How long ago the image was built.
- **Size:** How much disk space it occupies.

### **Part 2: Practice - The Housekeeping (PPP)**

**The Tools for Management:**

1. **Listing Images:**
   ```bash
   docker image ls
   ```
2. **Inspecting Images (Metadata):**
   ```bash
   docker image inspect <image-id>
   ```
3. **Removing a Specific Image:**
   ```bash
   docker image rm <image-id-or-name>
   # Or the shortcut:
   docker rmi <image-id-or-name>
   ```
4. **Deep Clean (The Nuclear Option):**
   ```bash
   docker image prune
   ```
   _Deletes all "dangling" images (images with no name/tag that are just leftover layers)._

## 🧠 **Real-World Case Study: "The Disk Space Mystery"**

- **Scenario:** A developer's laptop says "Disk Full." They only have 10 containers.
- **The Investigation:** They run `docker image ls` and realize they have 50 versions of the `node` and `python` images, each taking 900MB.
- **The Solution:** They prune unused images and switch to **Alpine** versions (which are only ~5MB).
- **The Result:** They reclaim 40GB of disk space.
- **The Lesson:** Regularly check your image inventory and prefer "Slim" or "Alpine" base images for production.

## 🤔 **Reflective Questions**

1. What is a "dangling image" and how does it get created?
2. Can you delete an image if a container (even a stopped one) is currently using it?
3. What is the difference between `docker image rm` and `docker image prune`?

## 📚 **Further Reading**

- **Docker CLI:** [docker image reference](https://docs.docker.com/engine/reference/commandline/image/)
- **Best Practices:** [Keep images small](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#exclude-with-dockerignore)

## 📝 **Mini Task (Production)**

1. Run `docker image ls` and identify the largest image on your machine.
2. Try to remove an old image you don't need:
   ```bash
   docker rmi <image-id>
   ```
3. If you get an error saying "image is being used by stopped container," what command should you run _first_?
4. Run `docker image prune` and see if any space was reclaimed.
