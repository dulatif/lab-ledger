# Lesson 3: Server-Side Rendering (SSR)

## Learning Goals

- Why SSR?
- The Nuxt.js Framework.

## 1. The Need for SSR

By default, Vue is a Client-Side app (CSR). The browser gets an empty `<div id="app">` and JS fills it.
**Result**:

- Bad for SEO (Crawlers see nothing).
- Slow "First Contentful Paint" (FCP).

**SSR**: The server sends fully rendered HTML.

## 2. Nuxt.js

Use **Nuxt**. Do not try to manually configure Vue SSR unless you are building a framework.
Nuxt provides:

- File-system routing (`pages/index.vue`).
- Auto-imports.
- SSR / SSG modes.

```bash
npx nuxi@latest init my-app
```

## 3. Hydration

The server sends HTML.
The client downloads the JS (Vue).
Vue "wakes up" the static HTML and makes it interactive. This is **Hydration**.

## Key Takeaways

- If you need SEO, use **Nuxt**.
- Write code that works on both Node.js and Browser (avoid `window` on server).
