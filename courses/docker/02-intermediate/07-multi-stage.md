# 7. Multi-stage Builds

## 🎯 Learning Goal

Create tiny production images by throwing away build tools.

## 🧠 Concept (The Problem)

To build a Go or Java app, you need the Compiler (Golang SDK, JDK, Maven). These are huge (500MB+).
To _run_ the app, you only need the compiled binary (10MB).
If you ship the Compiler in production:

1.  Image is huge (slow download/storage).
2.  Security risk (Attacker can compile malicious code).

## 💻 Implementation (The Solution)

Use two `FROM` instructions in one Dockerfile.

```dockerfile
# Stage 1: Builder
FROM golang:1.20 AS builder
WORKDIR /app
COPY . .
RUN go build -o myapp main.go
# This stage is huge (contains Go SDK)

# Stage 2: Runner
FROM alpine:latest
WORKDIR /root/
# Copy ONLY the artifact from Stage 1
COPY --from=builder /app/myapp .
CMD ["./myapp"]
# This stage is tiny (Alpine + Binary only)
```

## 🧩 Result

The final image is based on Stage 2. Stage 1 is discarded automatically.
Resulting image size: ~15MB (vs ~800MB).

## 🔑 Key Takeaways

- Essential for compiled languages (Go, Java, Rust, C++).
- Also useful for Frontend (Stage 1: Node/NPM build. Stage 2: Nginx hosting static HTML/JS).
