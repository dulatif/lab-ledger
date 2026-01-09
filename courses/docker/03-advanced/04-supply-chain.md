# 4. Supply Chain Security

## 🎯 Learning Goal

Trusting what you run.

## 🧠 Concept

How do you know `node:18` hasn't been tampered with by a hacker?

### 1. Vulnerability Scanning

Tools (Trivy, Snyk, Docker Scout) scan the packages installed in your image against CVE databases (Common Vulnerabilities and Exposures).

- "Your image uses OpenSSL 1.1 which has Heartbleed bug." -> Upgrade Base Image.

### 2. Image Signing (Docker Content Trust / Cosign)

Cryptographic signing of images.

- Publisher signs image with Private Key.
- Consumer (Docker) verifies signature with Public Key.
- Ensures integrity and origin.

## 💻 Implementation

Integration in CI:

```bash
docker build -t app .
trivy image app --exit-code 1 --severity CRITICAL
# If critical bugs found, build fails.
```

## 🧩 Activity / Challenge

1.  Install Trivy.
2.  Scan an old image (e.g., `node:10`). Watch the red screen of death (hundreds of vulnerabilities).
3.  Scan `node:18-alpine`. Much cleaner.

## 🔑 Key Takeaways

- Scan early (CI).
- Use minimal base images (Distroless or Alpine) to reduce attack surface.
