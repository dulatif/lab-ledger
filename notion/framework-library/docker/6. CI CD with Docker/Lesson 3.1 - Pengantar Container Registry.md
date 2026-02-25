# 💡 DK06.3.1: Pengantar Container Registry

**Outline:**

- **The Storage (SEEI):** Understanding the Registry as the central hub for sharing and storing your images.
- **The Options (PPP):** Comparing Docker Hub, GitHub Container Registry (GHCR), and Private Cloud Registries.
- **Your Mission (Production):** Deciding where to store your company's proprietary code.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Storage (SEEI)**

**"It's like Git for Images"**
A Container Registry is a server that stores your Docker images. Just as GitHub stores your source code, a Registry (like Docker Hub) stores your compiled environment.

**Why do you need one?**

1. **Sharing:** To move an image from your laptop to a production server, you "Push" it to a registry and then "Pull" it from the server.
2. **Versioning:** The registry keeps a history of your image tags (v1, v2, v3).
3. **Collaboration:** Your team can pull the exact same environment you built.

### **Part 2: Practice - The Options (PPP)**

**Which Registry should you use?**

| Registry          | Best For        | Why?                                                      |
| :---------------- | :-------------- | :-------------------------------------------------------- |
| **Docker Hub**    | Public Projects | The industry standard. Easiest to use.                    |
| **GHCR (GitHub)** | Teams on GitHub | High speed (same network as Actions). Close to your code. |
| **ECR/GCR/ACR**   | Cloud Native    | Maximum security. Integrated with AWS/Google/Azure.       |

**The "Private" Rule:**
For any company project, **never** use a public repository. Always ensure your registry is private and requires authentication.

## 🧠 **Real-World Case Study: "The Accidental Open Source"**

- **Scenario:** A startup pushed their innovative Fintech app to Docker Hub.
- **The Mistake:** They forgot to set the repository to "Private."
- **The Incident:** A competitor noticed the new image, pulled it, and used `docker history` and `dive` to reverse-engineer the proprietary algorithm.
- **The Result:** The startup lost its competitive edge within a week.
- **The Lesson:** "Secure by Default" starts with the registry. Check your privacy settings **before** your first push.

## 🤔 **Reflective Questions**

1. If you delete an image from your local machine, is it still available in the Registry? (Answer: Yes!).
2. What is the difference between a "Repository" and a "Registry"? (Answer: A Registry contains many Repositories).
3. Why is GitHub Container Registry (GHCR) becoming more popular than Docker Hub for many developers?

## 📚 **Further Reading**

- **Docker Documentation:** [Docker Hub overview](https://docs.docker.com/docker-hub/)
- **GitHub:** [Working with the Container registry](https://docs.github.com/en/packages/working-with-a-github-packages-site/working-with-the-container-registry)

## 📝 **Mini Task (Production)**

1. Visit [hub.docker.com](https://hub.docker.com/).
2. Create a free account if you don't have one.
3. Search for the `nginx` image.
4. Notice the "Official Image" badge.
5. Search for a random app (e.g., `minecraft-server`).
6. Notice how many different people have uploaded versions of it.
7. Reflect: Which one would you trust to run in a production environment? Why?
