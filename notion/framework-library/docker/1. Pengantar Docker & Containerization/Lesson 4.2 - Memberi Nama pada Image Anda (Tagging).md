# 💡 DK04.2: Memberi Nama pada Image Anda (Tagging)

**Outline:**

- **The Syntax (SEEI):** Understanding the `repository:tag` structure used to identify images.
- **The Strategy (PPP):** Using tagging for versioning and environment management (dev vs. prod).
- **Your Mission (Production):** Implementing a SemVer (Semantic Versioning) strategy for your images.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Syntax (SEEI)**

**What is a Tag?**
A tag is a label applied to a Docker image. It allows you to have multiple versions of the same repository.

**The Full Name Structure:**
`[registry-url/][username/][repository]:[tag]`

- **Repository:** The name of the application (e.g., `web-api`).
- **Tag:** The specific version (e.g., `1.0.0`, `alpine`, `latest`).

**The "Latest" Trap:**
If you don't specify a tag, Docker defaults to `latest`. Note: `latest` is just a string; it doesn't automatically mean it's the newest build—it's just the tag used if none other is provided.

### **Part 2: Practice - The Strategy (PPP)**

**The `docker tag` command:**
You can create a new tag for an existing image without rebuilding it.

```bash
docker tag old-image:v1 new-user/my-app:v1.2.3
```

**Common Tagging Strategies:**

1. **Semantic Versioning:** `app:1.0.0`, `app:1.0`, `app:1`.
2. **Environment Based:** `app:dev`, `app:staging`, `app:prod`.
3. **Architecture:** `app:amd64`, `app:arm64`.
4. **Base OS:** `app:bullseye`, `app:alpine`.

## 🧠 **Real-World Case Study: "The Rollback Rescue"**

- **The Mistake:** A company always uses the `latest` tag for production. They deploy a broken build, and `latest` is now broken. They don't have the previous image ID handy.
- **The Fix:** They start using versioned tags: `api:v1.0.1`, `api:v1.0.2`.
- **The Result:** When `v1.0.3` fails, they simply run `docker run api:v1.0.2` to instantly rollback.
- **The Lesson:** Never use `latest` in production. Always use specific, immutable version tags for stability.

## 🤔 **Reflective Questions**

1. Why is `latest` considered dangerous for deployment?
2. Does `docker tag` create a copy of the image on your disk? (Answer: No, it just creates a new pointer to the same layers).
3. How can you have one image with four different tags?

## 📚 **Further Reading**

- **Docker guide:** [Manage tags](https://docs.docker.com/engine/reference/commandline/tag/)
- **Best Practices:** [Tagging your images](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#tag-your-images)

## 📝 **Mini Task (Production)**

1. Build an image named `my-app:v1.0`.
2. Now, tag that same image as `my-app:latest`:
   ```bash
   docker tag my-app:v1.0 my-app:latest
   ```
3. Run `docker image ls`. Notice that `my-app:v1.0` and `my-app:latest` share the **exact same Image ID**.
4. Push (conceptually) a new version: build a `v2.0` and tag it as `latest`.
5. Verify that `latest` now points to the new Image ID, while `v1.0` still points to the old one.
