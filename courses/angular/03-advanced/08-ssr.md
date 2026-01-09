# Lesson 8: Server Side Rendering (SSR)

## Learning Goals

- Why SSR? (SEO, Performance).
- Angular Hydration.

## 1. Adding SSR

```bash
ng add @angular/ssr
```

This converts your app to run on a Node.js server.

## 2. Hydration (Angular 16+)

Previously, SSR would destroy the server-rendered DOM and replace it (flicker).
Non-Destructive Hydration reuses the DOM and just attaches event listeners.
It's enabled by default in new projects:

```typescript
providers: [provideClientHydration()];
```

## 3. Gotchas

- **No `window` or `document`**: On the server, these don't exist.
- Wrap platform-specific code:
  ```typescript
  if (isPlatformBrowser(this.platformId)) {
    // Access localStorage
  }
  ```

## Key Takeaways

- SSR is critical for public-facing sites (SEO).
- Hydration makes the startup instant and smooth.
