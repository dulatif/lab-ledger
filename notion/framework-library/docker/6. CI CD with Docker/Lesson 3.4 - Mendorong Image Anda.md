# 💡 DK06.3.4: Mendorong Image Anda

**Outline:**

- **The Broadcast (SEEI):** Understanding the final step of the build phase: making the image available to the world.
- **The Push (PPP):** Implementing the `push` parameter in `docker/build-push-action`.
- **Your Mission (Production):** Completing the loop from "Commit" to "Registry."

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Broadcast (SEEI)**

**Making it Permanent**
A Docker image built inside the GitHub Runner is temporary. As soon as the Runner finishes, the VM is deleted, and your image is gone forever. To save it, you must **Push** it to a Registry.

**The Impact on the Pipeline:**
Once an image is pushed, the "Building" phase of DevOps is over. The "Deployment" phase begins. The Registry acts as the "Safe" where the production-ready tools are stored.

### **Part 2: Practice - The Push (PPP)**

**The "Unified" Build and Push:**
Because pushing is so common, the official `docker/build-push-action` handles both steps in one.

```yaml
- name: 🏗️ Build and Push
  uses: docker/build-push-action@v4
  with:
    context: .
    push: true # This is the magic switch!
    tags: ${{ steps.meta.outputs.tags }}
    labels: ${{ steps.meta.outputs.labels }}
```

**What happens during the push?**

1. Docker checks which layers are already on the registry (Layer caching).
2. It only uploads the "New" or changed layers.
3. It registers the new tag.

**Success Verification:**
A professional workflow always ends with a verification that the image is actually on the registry. You can check this via the Docker Hub website or the GitHub "Packages" tab on your repo.

## 🧠 **Real-World Case Study: "The Incomplete Pipeline"**

- **Scenario:** A developer set up a build pipeline that ran every hour.
- **The Mistake:** They forgot to set `push: true`.
- **The Frustration:** For a whole week, the builds were "Green" (Successful), but the production servers were still running a 7-day-old version of the app.
- **The Result:** They were fixing bugs in the code, but they weren't seeing the fixes in production.
- **The Fix:** They checked the logs, realized the "Push" layer was missing, and enabled it.
- **The Lesson:** A build without a push is just a "Self-Check." If you want to deploy, you must BROADCAST.

## 🤔 **Reflective Questions**

1. Why does pushing an image usually take less time than building it? (Answer: Layer caching).
2. What happens if you try to push to a repository that doesn't exist? (Answer: Swarm/Docker Hub will usually create it for you automatically).
3. If a push fails halfway through, is the registry "brocken"? (Answer: No, Docker only registers the tag if all layers arrive safely).

## 📚 **Further Reading**

- **Docker:** [Pushing images to a registry](https://docs.docker.com/engine/reference/commandline/push/)
- **Action:** [Full example of Build and Push](https://github.com/docker/build-push-action#complete-workflow)

## 📝 **Mini Task (Production)**

1. Update your existing Docker workflow.
2. Ensure the `Login` step works.
3. Set `push: true` in your `build-push-action`.
4. Trigger the build.
5. Visit your Docker Hub or GHCR account.
6. Can you see the new image?
7. Check the **Size** and the **Created Date**.
8. Reflect: You just built a professional, automated software factory. How many minutes of manual work will this save you over the next month?
