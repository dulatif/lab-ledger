# 10. Hot Reloading and Debugging in Containers

## 🎯 Learning Goal

Making the development loop feel native.

## 🧠 Concept

By default, if you copy code into an image, you must rebuild the image to see changes. This takes 30s+. Too slow for coding.
**Solution**: Bind Mounts.
Overwrite the `/app` folder inside the container with your live local folder.

## 💻 Implementation (Compose)

```yaml
services:
  api:
    build: .
    command: npm run dev # Use nodemon/watch mode
    volumes:
      - ./src:/app/src # Sync source code
      - /app/node_modules # HACK: Use the container's node_modules, don't overwrite with host's (empty) one.
```

### The Node Modules Hack

Host (Laptop) doesn't have `node_modules` (or acts differently). Container does.
`- /app/node_modules` is an anonymous volume. It tells Docker: "Preserve the folder inside the container, don't let the bind mount from host hide it."

## 🧩 Activity / Challenge

1.  Set up a React/Node app with Compose.
2.  Enable Bind Mount.
3.  Change text in `App.js`. Save.
4.  Watch the container log. Validates it recompiles instantly.

## 🔑 Key Takeaways

- Development Docker setup is different from Production Docker setup.
- Dev = Bind Mounts + Watch mode (nodemon).
- Prod = COPY + Multi-stage + Optimized.
