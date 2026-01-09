# Lesson 9: Deployment

## Learning Goals

- Building for Production.
- Serving SPA.

## 1. Building

```bash
npm run build
```

Produces a `dist/` folder containing:

- `index.html`
- `assets/*.css`
- `assets/*.js`

## 2. Serving (SPA Issue)

Since it's an SPA (Single Page App), all routes must fallback to `index.html`.
If you refresh `/about` on a standard server, you get 404 (because `about.html` doesn't exist).

**Nginx Config**:

```nginx
location / {
  try_files $uri $uri/ /index.html;
}
```

**Netlify/Vercel**: handled automatically via `_redirects` or config.

## 3. Docker

```dockerfile
# Build Stage
FROM node:18 as build
WORKDIR /app
COPY . .
RUN npm install && npm run build

# Production Stage
FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html
```

## Key Takeaways

- The `dist` folder is all you need.
- Ensure your server handles the SPA History Fallback.
