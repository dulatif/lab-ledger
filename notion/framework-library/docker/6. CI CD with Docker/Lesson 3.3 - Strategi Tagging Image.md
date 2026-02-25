# 💡 DK06.3.3: Strategi Tagging Image

**Outline:**

- **The Identification (SEEI):** Understanding that a tag is not just a label, but a version contract.
- **The Pattern (PPP):** Implementing semantic versioning, Git SHAs, and branch-based tags.
- **Your Mission (Production):** Designing a tagging system that makes auditing easy.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Identification (SEEI)**

**"Wait, which version is running?"**
If you only use the tag `:latest`, you have no idea what code is actually running on your server. If a bug appears, you can't "Roll back" because the previous version was also named `:latest`.

**The Three Primary Strategies:**

1. **The Commit Hash (SHA):** (e.g., `my-app:8f2a1c6`)
   - **Pros:** Unique for every build. Extremely precise.
   - **Cons:** Impossible to remember or read.
2. **Semantic Versioning (SemVer):** (e.g., `my-app:v1.2.0`)
   - **Pros:** Human-readable. Communicates "How big" the change is.
   - **Cons:** Requires manual effort to update (usually via Git Tags).
3. **The Branch Name:** (e.g., `my-app:main` or `my-app:dev`)
   - **Pros:** Simple for internal testing.
   - **Cons:** Mutable (changes every time the branch is updated).

### **Part 2: Practice - The Pattern (PPP)**

**The "Hybrid" Strategy:**
Professional pipelines usually tag an image with **Multiple** tags at once.

- `my-app:9f3a1d4` (The precise one)
- `my-app:v2.1` (The human one)
- `my-app:latest` (The "Latest working" one)

**Automating Tags in GitHub Actions:**
Docker provides an official action called `docker/metadata-action` specifically for this:

```yaml
- name: 🏷️ Extract Metadata
  id: meta
  uses: docker/metadata-action@v4
  with:
    images: my-docker-hub-user/my-app
    tags: |
      type=semver,pattern={{version}}
      type=sha
      type=ref,event=branch
```

## 🧠 **Real-World Case Study: "The Silent Regression"**

- **Scenario:** A dev team was using the `:dev` tag for their staging server.
- **The Bug:** A developer pushed "Buggy" code to the `dev` branch. The CI build pushed the new `:dev` image.
- **The Mystery:** Another developer was testing an unrelated feature on the staging server. Everything was breaking, but they didn't know why because the tag hadn't "Changed" from their perspective.
- **The Fix:** They switched to **SHA-based** tagging for all environments.
- **The Success:** Now, every time they looked at the staging server, they could see the exact Git Commit Hash. They identified the buggy commit in seconds.
- **The Lesson:** Always have a "Unique" tag. Don't rely solely on names like `dev` or `latest`.

## 🤔 **Reflective Questions**

1. Why is the `:latest` tag considered dangerous for production?
2. How do you find the Git Commit that corresponds to a specific image tag in your registry?
3. Should you use the "Date" (e.g., `app:2023-10-25`) as a tag?

## 📚 **Further Reading**

- **SemVer.org:** [Semantic Versioning 2.0.0](https://semver.org/)
- **GitHub:** [Docker Metadata Action](https://github.com/docker/metadata-action)

## 📝 **Mini Task (Production)**

1. Look at your current Docker project.
2. How are you naming your images?
3. Research the `github.sha` variable in GitHub Actions.
4. Try to write a `docker build` command that uses the first 7 characters of the SHA as the tag. (Hint: `${GITHUB_SHA::7}`).
5. Reflect: If you had 1,000 images in your registry, which tagging strategy would help you find the "Oldest" one fastest?
