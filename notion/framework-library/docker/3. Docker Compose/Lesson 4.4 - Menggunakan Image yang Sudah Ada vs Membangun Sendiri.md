# 💡 DK03.4.4: Menggunakan Image yang Sudah Ada vs Membangun Sendiri

**Outline:**

- **The Choice (SEEI):** Deciding between `image` and `build` in your Service definitions.
- **The Optimization (PPP):** Understanding how Compose handles image creation and updates.
- **Your Mission (Production):** Orchestrating a mix of off-the-shelf and custom-built services.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Choice (SEEI)**

In a typical application stack:

1. **Third-Party Services** (like Redis, Postgres, Nginx): You use the **`image`** key to pull a ready-made version from Docker Hub.
2. **Your Application** (the code YOU write): You use the **`build`** key to tell Compose to create a custom image from your `Dockerfile`.

**The Anatomy of `build`:**

```yaml
services:
  my-app:
    build:
      context: . # Location of the code
      dockerfile: Dockerfile # Name of the Dockerfile (optional if default)
```

### **Part 2: Practice - The Optimization (PPP)**

**The Build Lifecycle:**

- **`docker compose up`**: If the image exists, it uses it. If not, it builds it.
- **`docker compose build`**: Explicitly re-builds all services with a `build` section.
- **`docker compose up --build`**: Rebuilds the images THEN starts the containers. Use this if you have changed your source code but NOT your volumes.

**Combining both?**
Yes, you can specify both `build` and `image`. Compose will build the image from the context and give it the name specified in the `image` field.

```yaml
services:
  api:
    build: .
    image: my-internal-registry.com/my-api:v1
```

## 🧠 **Real-World Case Study: "The Stale Image"**

- **Scenario:** A developer makes a change to their `package.json` and runs `docker compose up`.
- **The Issue:** The application crashes because the new dependency is missing.
- **The Confusion:** "I updated my code, why is the container still using the old version?"
- **The Fix:** They realize that `up` doesn't automatically rebuild if an image with that name already exists. They run `docker compose up --build`.
- **The Lesson:** When in doubt, use `--build` to ensure your containers are running the latest version of your `Dockerfile` logic.

## 🤔 **Reflective Questions**

1. Why would you want to specify an `image` name even if you are using the `build` key?
2. What is the difference between the "Context" and the "Dockerfile" path?
3. In a multi-service stack, which services should usually be `build` and which should be `image`?

## 📚 **Further Reading**

- **Docker Reference:** [Build section in Compose](https://docs.docker.com/compose/compose-file/build/)
- **Guide:** [Build, build-context and dockerfile](https://docs.docker.com/compose/compose-file/05-services/#build)

## 📝 **Mini Task (Production)**

1. Write a `docker-compose.yml` that defines two services:
   - `postgres`: Use the official `postgres:15-alpine` image.
   - `web`: Build from the current directory.
2. Requirement:
   - Provide a custom name for the `web` image: `my-training-app:latest`.
3. If you run `docker compose build`, check your local images with `docker images`.
4. Did the `my-training-app` image get created?
5. Why is this more efficient than running a manual `docker build` and then a `docker run`?
