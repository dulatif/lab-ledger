# 💡 DK03.3: Optimasi Build dengan Layer Caching

**Outline:**

- **The Cache Mechanism (SEEI):** Understanding how Docker reuses previous build results to save time.
- **The Optimization Rule (PPP):** Reordering commands (Most stable -> Most volatile) to maximize cache hits.
- **Your Mission (Production):** Speeding up a slow build by 90%.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Cache Mechanism (SEEI)**

**Docker Layers are Cached**
Every instruction in a `Dockerfile` (FROM, RUN, COPY, etc.) creates a new layer. Docker caches these layers locally. When you run `docker build` again, Docker checks: "Has this instruction or the files involved changed?"

- **If NO:** It reuses the old layer (**CACHED**). This is instantaneous.
- **If YES:** It rebuilds that layer and **every single layer after it**.

**The Chain Reaction**
Caching is invalid-sensitive: once a layer is invalidated (because a file changed or a command was edited), all subsequent layers must be rebuilt.

### **Part 2: Practice - The Optimization Rule (PPP)**

**The "Bottom-Up" Volatility Rule:**
Place commands that change **least often** at the top, and commands that change **most often** (like your source code) at the bottom.

**❌ Poor Caching (Slow):**

```dockerfile
FROM node:18
WORKDIR /app
COPY . .           # 1. Any change in any file invalidates this...
RUN npm install    # 2. ...and forces a slow npm install every time!
CMD ["node", "index.js"]
```

**✅ Optimized Caching (Fast):**

```dockerfile
FROM node:18
WORKDIR /app
COPY package.json .  # 1. Only re-runs if package.json changes.
RUN npm install       # 2. Re-uses the node_modules cache!
COPY . .             # 3. Source code change only affects this small layer.
CMD ["node", "index.js"]
```

## 🧠 **Real-World Case Study: "The 10-Minute Coffee Break"**

- **The Problem:** A CI/CD pipeline takes 15 minutes to build a small app because every commit triggers a full `npm install`.
- **The Solution:** The developer reorders the `Dockerfile` to copy `package.json` first and run `npm install` _before_ copying the rest of the source code.
- **The Result:** Build time drops from **15 minutes** to **30 seconds** for most commits.
- **The Lesson:** Caching is the difference between an agile development flow and a frustrating waiting game.

## 🤔 **Reflective Questions**

1. Why does changing a single line in `index.js` trigger a rebuild of everything _below_ the `COPY` instruction?
2. What happens if you run `apt-get update` in a separate `RUN` instruction before `apt-get install`? (Hint: The update might get stale and cached).
3. How can you tell if a layer was pulled from cache during a build? (Hint: Look for the `---> Using cache` message in the output).

## 📚 **Further Reading**

- **Docker Orientation:** [Leverage build cache](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#leverage-build-cache)
- **Deep Dive:** [Under the hood of Docker layers](https://docs.docker.com/storage/storagedriver/#images-and-layers)

## 📝 **Mini Task (Production)**

1. Take the "Poor Caching" example above and build it. Observe the time it takes.
2. Change a single comment in your `index.js` file and build again. Notice how long `npm install` takes.
3. Now, adopt the "Optimized Caching" structure. Build once (it will be slow).
4. Change the same comment in `index.js` and build again.
5. Identify the "CACHED" lines in your terminal output. How much time did you save?
