# 💡 DK04.4.4: Mempercayai Image Anda (Content Trust)

**Outline:**

- **The Trust Gap (SEEI):** Understanding the "Man-in-the-Middle" risk when pulling images from registries.
- **The Signature (PPP):** Enabling Docker Content Trust (DCT) to verify the authenticity of images.
- **Your Mission (Production):** Ensuring your production cluster only runs "Signed" images.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Trust Gap (SEEI)**

**Who built this image?**
When you run `docker pull nginx`, how do you know you are getting the real Nginx image and not a fake one injected by a hacker who compromised the registry or redirected your DNS?

**The "Poisoned Image" Threat:**
An attacker could upload an image named `nginx` to a compromised registry that looks identical but contains a hidden backdoor or a credential stealer.

**The Solution: Digital Signatures**
Docker Content Trust (DCT) allows image publishers to digitally sign their images. When a user pulls a signed image, Docker verifies that the signature is valid and that the image hasn't been tampered with since it was signed.

### **Part 2: Practice - The Signature (PPP)**

**Enabling Content Trust:**
By default, DCT is disabled. You can enable it globally in your shell session.

**The Environment Variable:**

```bash
export DOCKER_CONTENT_TRUST=1
```

**The Impact:**
Once enabled, any command that interacts with a registry (`pull`, `push`, `build`, `run`) will enforce signing.

- If you try to pull an **unsigned** image, the command will **Fail**.
- If you try to pull a signed image that was tampered with, the command will **Fail**.

**Signing an Image (Publisher side):**
When you push an image with DCT enabled, Docker will prompt you to create keys (Root key and Tagging key) and will sign the image automatically.

## 🧠 **Real-World Case Study: "The Typosquatting Attack"**

- **Scenario:** A developer tries to pull `alpine` but accidentally types `aplne`.
- **The Trap:** An attacker had registered the name `aplne` on Docker Hub and uploaded a malicious image.
- **The Protection:** If the developer had `DOCKER_CONTENT_TRUST=1` enabled, the pull would have failed because the `aplne` image was not signed by a trusted authority.
- **The Result:** The developer sees an error, realizes their typo, and stays safe.
- **The Lesson:** Trust is not a default; it's a configuration. Content Trust is the "SSL for Docker Images."

## 🤔 **Reflective Questions**

1. What happens if the private keys used to sign an image are lost? (Warning: You lose the ability to update that image's tags!).
2. Does Docker Hub support signing for all images, or just "Official" ones?
3. How do you verify the signatures of an image manually? (`docker trust inspect`).

## 📚 **Further Reading**

- **Docker Documentation:** [Content trust in Docker](https://docs.docker.com/engine/security/trust/)
- **Guide:** [Manage keys for content trust](https://docs.docker.com/engine/security/trust/trust_key_mng/)

## 📝 **Mini Task (Production)**

1. In your terminal, run `export DOCKER_CONTENT_TRUST=1`.
2. Try to pull a very obscure, old image from a random user on Docker Hub.
3. Does the pull fail or succeed?
4. Now, pull an official image: `docker pull redis`. Does it succeed?
5. Run `docker trust inspect redis` and look at the "Signatures" section.
6. Reflect: Why is this one of the last things most teams implement, even though it's one of the most important?
