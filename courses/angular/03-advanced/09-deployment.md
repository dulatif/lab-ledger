# Lesson 9: Deployment

## Learning Goals

- Build for Production.
- Environments.

## 1. Environments

Manage API URLs for Dev vs Prod.
`src/environments/environment.ts` vs `environment.prod.ts`.

```typescript
import { environment } from "./environments/environment";

http.get(environment.apiUrl + "/users");
```

## 2. Building

```bash
ng build
```

This creates a `dist/my-app` folder.
It performs:

- Ahead-of-Time (AOT) compilation.
- Tree-shaking (removing unused code).
- Minification.

## 3. Hosting

- **Static (SSG/CSR)**: Upload the `browser` folder to Netlify, Vercel, S3, or Firebase Hosting.
- **SSR**: You need a Node.js environment (e.g., Firebase Functions, Docker container).

## Key Takeaways

- Never deploy the `ng serve` version.
- Always inspect your build bundle size (`source-map-explorer`).
