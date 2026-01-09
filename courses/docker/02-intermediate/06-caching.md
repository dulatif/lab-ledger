# 6. Layer Caching and Optimization

## 🎯 Learning Goal

Speed up builds from minutes to seconds.

## 🧠 Concept

Docker caches every layer.
When you rebuild, Docker checks: "Has the input for this instruction changed?"

- If **No**: Reuse cached layer. Instant.
- If **Yes**: Re-run instruction AND invalidate cache for **all subsequent instructions**.

## 💻 Deep Dive: The "COPY" Trap

Look at this common mistake:

```dockerfile
# BAD OPTIMIZATION
COPY . .
RUN npm install
```

**Why implies Bad?**:
`COPY . .` copies _everything_, including your source code.
If you change `index.js` (which happens 100 times a day):

1.  `COPY` cache invalidated (because source changed).
2.  `RUN npm install` cache invalidated (because it follows COPY).
3.  Result: You re-download internet dependencies every time you change a semicolon in your code. Slow!

## 💻 The Fix: Dependency Separation

```dockerfile
# GOOD OPTIMIZATION
COPY package.json .
RUN npm install
# Cache of 'npm install' is only broken if package.json changes!

COPY . .
# If source changes, only this COPY layer runs.
# Dependencies are skipped.
```

## 🧩 Activity / Challenge

1.  Take the bad Dockerfile. specific build time. Change code. Build again.
2.  Refactor to good Dockerfile. Build. Change code. Build again.
3.  Observe the second build is near instant. "Using Cache".

## 🔑 Key Takeaways

- Put the least frequently changed things (OS, Deps) at the top.
- Put the most frequently changed things (Source Code) at the bottom.
- Use `.dockerignore` to prevent `node_modules` or `.git` from being COPY'd, which bloats the context.
