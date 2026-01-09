# Lesson 1: Bootstrapping a Project

## Learning Goals

- The Vue Build Toolchain.
- Scaffolding with `create-vue`.
- Running the Dev Server.

## 1. The Toolchain

Modern Vue ignores the legacy "@vue/cli" (Webpack) in favor of **Vite** (pronounced "veet").
Vite is extremely fast because it uses native ES modules in the browser during development.

## 2. Creating a Project

To create a new project, run:

```bash
npm create vue@latest
```

This installs `create-vue`, the official scaffolding tool. You will be prompted with choices:

- TypeScript? (Recommended: Yes)
- Vue Router? (Yes for SPAs)
- Pinia? (Yes for State)
- Vitest? (Yes for Testing)

## 3. Running the App

```bash
cd <your-project-name>
npm install
npm run dev
```

The app will start at `http://localhost:5173`.

## 4. File Structure (Minimal)

- `index.html`: The entry point (Vite serves this).
- `src/main.ts`: The JavaScript entry point.
- `src/App.vue`: The root component.

## Key Takeaways

- Always use `npm create vue@latest`.
- Vite is the build engine.
