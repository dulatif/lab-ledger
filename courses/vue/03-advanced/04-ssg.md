# Lesson 4: Static Site Generation (SSG)

## Learning Goals

- Pre-rendering.
- VitePress.

## 1. What is SSG?

Like SSR, but performed at **Build Time**.
We generate `.html` files for every route.
Great for Docs, Blogs, Marketing sites.

## 2. VitePress

The "Vue" Documentation tool.
It treats Markdown files as Vue components.

`index.md`:

```markdown
# Hello

<CounterComponent />
```

## 3. Nuxt Generating

`nuxt generate` runs your app, crawls every link, and saves the HTML.
You can host the result on any static host (GitHub Pages, Netlify) for free.

## Key Takeaways

- **SSG** = Fastest possible performance (Static HTML) + cheap hosting.
- Bad for dynamic content (User Dashboards) - use pure SPA or SSR for that.
