# Lesson 2: Project Setup (CLI)

## Learning Goals

- Install Angular CLI.
- Create and Run a project.
- Understand the File Structure.

## 1. The Angular CLI

The Command Line Interface is your best friend. It generates code, serves the app, and runs tests.

```bash
npm install -g @angular/cli
```

## 2. Creating a Project

```bash
ng new my-app
# Prompts:
# - Which stylesheet format? (Select CSS or SCSS)
# - Enable Server-Side Rendering? (No/Yes)
```

## 3. Running the App

```bash
cd my-app
ng serve --open
```

Access at `http://localhost:4200`.

## 4. File Structure

- `angular.json`: Configuration for the workspace (assets, builds, environments).
- `src/main.ts`: Entry point. Bootstraps the app.
- `src/index.html`: The single HTML page.
- `src/app/`: Where your code lives.
  - `app.component.ts`: The root component.
  - `app.routes.ts`: Router configuration.

## Key Takeaways

- Always use the CLI. Do not manually create files.
- `ng serve` uses Webpack/Vite under the hood to compile TypeScript on the fly.
