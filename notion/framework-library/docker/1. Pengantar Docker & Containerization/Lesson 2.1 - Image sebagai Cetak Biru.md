# 💡 DK02.1: Image sebagai Cetak Biru

**Outline:**

- **The Analogy (SEEI):** Comparing Docker Images to architectural blueprints or programming classes.
- **The internal Architecture (PPP):** Understanding layers and the read-only nature of images.
- **Your Mission (Production):** Inspecting the contents and history of an image.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Analogy (SEEI)**

**What is a Docker Image?**
An image is a static, read-only file that contains everything needed to run an application: code, runtime, libraries, environment variables, and config files.

**The Blueprint Analogy**

- **Image:** The blueprint for a house. It defines where the walls go, but you can't live inside a blueprint.
- **Container:** The actual house built from the blueprint. You can have 10 identical houses built from 1 single blueprint.
- **Independence:** If you change the blueprint, existing houses don't change. If you burn down a house, the blueprint remains safe.

### **Part 2: Practice - The internal Architecture (PPP)**

**Layered File System**
Images are built in layers. Each command in a `Dockerfile` creates a new layer.

- **Base Layer:** Usually a minimal OS (like Alpine or Ubuntu).
- **Tool Layers:** Installing Node.js, Python, or Nginx.
- **App layer:** Your specific source code.

**Key Characteristic: Immutability**
Once an image is built, it **never** changes. If you need to update your app, you build a _new_ image. This ensures reproducibility.

**Command to Explore:**

```bash
docker image inspect <image-name>
docker image history <image-name>
```

## 🧠 **Real-World Case Study: "The Golden Image"**

- **Scenario:** A dev team uses a specific version of Node.js and a specific set of security patches.
- **The Solution:** They create a "Golden Image" that contains all these correct versions.
- **The Benefit:** Every developer and every server pulls this exact same "Golden Image."
- **The Result:** Zero "environmental drift." The environment is locked down and predictable.

## 🤔 **Reflective Questions**

1. Why are images read-only? (Answer: To ensure they are always consistent and reusable).
2. What is the benefit of having a layered architecture? (Hint: Think about sharing common layers between different images).
3. If I edit a file inside a running container, does the Image change?

## 📚 **Further Reading**

- **Docker Docs:** [About images and layers](https://docs.docker.com/storage/storagedriver/#images-and-layers)
- **Deep Dive:** [The Docker Image Manifest](https://docs.docker.com/registry/spec/manifest-v2-2/)

## 📝 **Mini Task (Production)**

1. List the images currently on your machine:
   ```bash
   docker image ls
   ```
2. Pick one image (e.g., `hello-world` or `alpine`) and inspect its history:
   ```bash
   docker image history <name>
   ```
3. Look at the output. Can you see the different layers and the commands that created them (e.g., `CMD`, `WORKDIR`)?
