# 💡 DK01.2: Tiga Pilar Docker

**Outline:**

- **The Concept (SEEI):** Defining the three essential components: Image, Container, and Registry.
- **The Lifecycle (PPP):** How these components interact to build and run applications.
- **Your Mission (Production):** Visualizing the flow from code to a running container.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Concept (SEEI)**

**The Core Components**
To master Docker, you must understand the relationship between these three terms:

1. **Docker Image:** An immutable, read-only template that contains the instructions for creating a Docker container. Think of it as a "Class" in OOP or a "Recipe" in cooking.
2. **Docker Container:** A runnable instance of an image. You can start, stop, move, or delete it. Think of it as an "Object" in OOP or the "Dish" prepared from the recipe.
3. **Docker Registry:** A storage and distribution system for images. The most famous one is **Docker Hub**. Think of it as "GitHub for Images."

**The Analogy**

- **Dockerfile:** The blueprint.
- **Docker Image:** The static snapshot of the environment.
- **Docker Container:** The actual running application.

### **Part 2: Practice - The Lifecycle (PPP)**

**The Workflow: Build -> Ship -> Run**

1. **Build:** You write a `Dockerfile` and build it to create an **Image**.
   ```bash
   docker build -t my-app .
   ```
2. **Ship:** You push your **Image** to a **Registry** so others can use it.
   ```bash
   docker push my-user/my-app
   ```
3. **Run:** Someone else pulls the image and starts a **Container**.
   ```bash
   docker run my-app
   ```

## 🧠 **Real-World Case Study: "The LEGO Analogy"**

- **The Mold (Image):** A LEGO factory has a mold to create a specific brick. No matter how many bricks you make, they are all identical because they came from the same mold.
- **The Brick (Container):** Each brick you pop out of the mold is an instance. You can paint one red and another blue (though images are read-only, containers have a "writable layer").
- **The Warehouse (Registry):** A store where all the molds are kept for anyone to order.

## 🤔 **Reflective Questions**

1. Can you run a container without an image?
2. What happens to the Image when you delete a Container?
3. Why do we say that Images are "Immutable"?

## 📚 **Further Reading**

- **Docker Orientation:** [Images and Containers](https://docs.docker.com/get-started/overview/#docker-objects)
- **Docker Hub:** [Explore Repositories](https://hub.docker.com/search?q=)

## 📝 **Mini Task (Production)**

Create a simple mapping for your current project:

1. What would be in your **Image**? (e.g., Node.js, your source code, node_modules).
2. How many **Containers** would you need? (e.g., 1 for the app, 1 for the database).
3. Where would you push your image if you wanted to deploy it? (e.g., Docker Hub, AWS ECR).
