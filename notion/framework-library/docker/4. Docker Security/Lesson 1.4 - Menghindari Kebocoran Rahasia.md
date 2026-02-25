# 💡 DK04.1.4: Menghindari Kebocoran Rahasia saat Build

**Outline:**

- **The Leakage (SEEI):** Understanding how secrets (API keys, passwords) accidentally end up in your image layers.
- **The Prevention (PPP):** Using `.dockerignore`, `BuildKit`, and "Secret Mounting" to keep your images clean.
- **Your Mission (Production):** Detecting and removing a leaked secret from an image history.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Leakage (SEEI)**

**The "Burying" Myth**
Many developers think that if they use `ARG` or `ENV` to pass a password, or if they `COPY` a `.env` file and then `RUN rm .env`, the secret is gone.
**This is False.**

- `ARG` values are stored in the image's metadata.
- `ENV` values are visible to anyone who runs `docker inspect`.
- `COPY + rm` leaves the file in a previous layer.

**The Negative Impact:**
Once you push a "Dirty" image to a registry (even a private one), those secrets are compromised. If the registry is hacked, or if an employee leaves the company, your secrets are exposed.

### **Part 2: Practice - The Prevention (PPP)**

**The "Safe Build" Techniques:**

1. **The `.dockerignore`:**
   _Always exclude sensitive files before they are even sent to the Docker daemon._
   ```text
   # .dockerignore
   .env
   .git
   id_rsa
   ```
2. **Docker Build Secret (BuildKit):**
   _Mount a secret during build that is NEVER stored in a layer._
   ```dockerfile
   # Dockerfile
   RUN --mount=type=secret,id=my_secret \
       ./run_authenticated_script.sh $(cat /run/secrets/my_secret)
   ```
   _Command:_ `docker build --secret id=my_secret,src=.env .`

## 🧠 **Real-World Case Study: "The $50,000 Key"**

- **Scenario:** A startup developer pushes an image to Docker Hub containing an AWS Access Key in an `ENV` variable.
- **The Incident:** Within 60 seconds, a bot pulls the image, extracts the key, and starts 500 high-end GPU instances for crypto mining.
- **The Bill:** The company receives a $50,000 bill from AWS the next morning.
- **The Fix:** They delete the image, rotate the AWS key, and switch to using **Runtime Environment Variables** (which aren't stored in the image).
- **The Lesson:** If it's a secret, it **never** belongs in a `Dockerfile` text or an image layer.

## 🤔 **Reflective Questions**

1. Why is `RUN rm secret.txt` ineffective for security?
2. What is the difference between `ARG` and `env` in terms of security? (Answer: Both are insecure for long-term secrets, but `ARG` is only during build).
3. How can you view the history of an image to see if it contains secrets? (`docker history <image>`).

## 📚 **Further Reading**

- **Docker Orientation:** [Build secrets](https://docs.docker.com/build/building/secrets/)
- **Guide:** [Best practices for credentials](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#use-multi-stage-builds)

## 📝 **Mini Task (Production)**

1. Create a file named `secret.txt` with some random text.
2. Build an image that `COPY` the file and then `RUN rm` it.
3. Use `docker history <image-name>` to look at the layers.
4. Download a tool like `dive` (or use `docker save` and inspect the tarball).
5. Can you find the contents of `secret.txt` in the image filesystem?
6. Explain how using `.dockerignore` would have prevented this entire problem.
