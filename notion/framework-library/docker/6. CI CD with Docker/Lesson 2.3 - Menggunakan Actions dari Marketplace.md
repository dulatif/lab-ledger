# 💡 DK06.2.3: Menggunakan Actions dari Marketplace

**Outline:**

- **The Library (SEEI):** Understanding that you don't have to write everything from scratch.
- **The Docker Stack (PPP):** Implementing `docker/setup-buildx-action` and `docker/build-push-action`.
- **Your Mission (Production):** Modernizing your pipeline using official Docker actions.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Library (SEEI)**

**Why "uses" is better than "run"**
While you _can_ write raw bash commands like `docker build`, it's often better to use official "Actions." These are reusable blocks of code that handle complex edge cases (like multi-platform builds, caching, and registry logins) automatically.

**The "Marketplace" Mindset:**
If it's a common task, someone has already written an Action for it.

- **Docker official:** `docker/build-push-action`
- **Snyk security:** `snyk/actions/docker@master`
- **AWS support:** `aws-actions/amazon-ecr-login`

### **Part 2: Practice - The Docker Stack (PPP)**

**The "Modern" Docker Build Workflow:**

```yaml
steps:
  - name: Checkout
    uses: actions/checkout@v3

  - name: Set up Docker Buildx
    uses: docker/setup-buildx-action@v2
    # This enables advanced features like multi-stage caching and multi-platform builds

  - name: Build only
    uses: docker/build-push-action@v4
    with:
      context: .
      push: false # We haven't logged into a registry yet!
      tags: my-app:latest
```

**Why use Buildx?**
Buildx is the next-generation Docker builder. It allows you to build images for multiple architectures (like Intel/AMD AND Apple/ARM) in a single command.

## 🧠 **Real-World Case Study: "The Apple M1 Compatibility"**

- **Scenario:** A developer with an Apple Silicon (M1) laptop built an image and pushed it.
- **The Issue:** The production server (Intel-based) couldn't run the image! It gave an `exec format error`.
- **The Fix:** They updated their GitHub Action to use `docker/setup-buildx-action` and specified `platforms: linux/amd64,linux/arm64`.
- **The Result:** GitHub Actions built two versions of the image. The Intel server pulled the Intel version, and the developer pulled the ARM version. Everything worked.
- **The Lesson:** Marketplace Actions give you "Superpowers" (like multi-arch support) with just 2 lines of configuration.

## 🤔 **Reflective Questions**

1. What is the difference between `v2`, `v3`, and `master` in an action's name? (Answer: These are version tags. Always prefer specific versions over `master` for stability).
2. Can you use a Marketplace action that isn't made by Docker or GitHub? (Yes, but check the star count and reputation!).
3. How do you pass parameters to an action? (Answer: Using the `with:` block).

## 📚 **Further Reading**

- **GitHub Marketplace:** [Official Docker Actions](https://github.com/marketplace?type=actions&query=docker)
- **Blog:** [Advanced Docker builds with GitHub Actions](https://www.docker.com/blog/multi-arch-build-and-images-the-simple-way/)

## 📝 **Mini Task (Production)**

1. Visit the [Docker Build-Push Action](https://github.com/marketplace/actions/build-and-push-docker-images) on the marketplace.
2. Look at the "Usage" example.
3. What is the purpose of `docker/metadata-action`? (Hint: It handles tagging logic like "branch name" or "v1.2.3").
4. Try to incorporate `docker/setup-qemu-action` into a draft workflow. What does "QEMU" do for Docker builds? (Hint: Hardware emulation).
5. Reflect: Is the YAML file becoming more readable or more complex by using these actions?
