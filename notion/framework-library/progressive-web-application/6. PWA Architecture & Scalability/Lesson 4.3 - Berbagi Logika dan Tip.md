# 💡 AT01.4.3: The Power of Sharing: Sharing Logic and Types

**Outline:**

- **The DRY Principle (Presentation):** The core motivation for sharing code and how monorepos make it seamless.
- **Creating a Shared Package (Practice):** A step-by-step guide to creating a shared UI package and using it in your PWA.
- **Your First Shared Utility (Production):** Creating a `utils` package and importing a function from it.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The DRY Principle**

"Don't Repeat Yourself" (DRY) is one of the oldest and most important principles in software engineering. In a monorepo, the `packages/` directory is our primary tool for upholding this principle. This is where we create our reusable modules that can be shared across any application in the repository.

The magic that makes this work seamlessly is the concept of **workspaces**, a feature supported by modern package managers (npm, yarn, pnpm). You can define a dependency in your PWA's `package.json` that points to a local package, like this:

```json
// in apps/pwa/package.json
"dependencies": {
  "ui": "workspace:*"
}
```

The `workspace:*` protocol tells the package manager, "Don't look for this on the npm registry; look for a package named `ui` inside this monorepo's workspaces." This creates a direct, symbolic link. When you're developing, any change you make in the `packages/ui` folder is instantly reflected in the PWA, without any need for publishing or versioning. This creates a tight, frictionless feedback loop.

### **Part 2: Practice - Creating a Shared Package**

Let's create a shared `ui` package and use a component from it.

**Step 1: Create the package structure**

```
/packages
└── /ui
    ├── package.json
    ├── index.tsx
    └── Button.tsx
```

**Step 2: Define the package's `package.json`** This file tells the monorepo that this folder is a package.

```json
// packages/ui/package.json
{
  "name": "ui",
  "version": "0.0.0",
  "main": "./index.tsx",
  "types": "./index.tsx",
  "exports": {
    ".": "./index.tsx",
    "./button": "./Button.tsx"
  },
  "devDependencies": {
    "typescript": "^5.0.0"
  }
}
```

**Step 3: Create a component**

```tsx
// packages/ui/Button.tsx
export const Button = () => {
  return <button>Shared Button</button>;
};
```

**Step 4: Export the component from the package's entry point**

```tsx
// packages/ui/index.tsx
export * from './Button';
```

**Step 5: Add the dependency to your PWA** Run this command from the root of your monorepo.

```bash
npm install ui --workspace=pwa
```

This will automatically add `"ui": "workspace:*"` to the `dependencies` in `apps/pwa/package.json`.

**Step 6: Use the component** Now, in any file in your PWA, you can import and use the shared button.

```tsx
// apps/pwa/src/App.tsx
import { Button } from 'ui';

function App() {
  return (
    <div>
      <h1>My PWA</h1>
      <Button />
    </div>
  );
}
```

That's it! Your PWA is now consuming a local, shared package.

## 🧠 **Real-World Case Study: "The Shared Hook"**

- **The Problem:** A team had a PWA and an admin dashboard. Both needed to fetch and display user data. They had duplicated their custom `useUser()` hook in both repositories. When a bug was found in the data-fetching logic, a developer fixed it in the PWA but forgot to update the admin app.
- **The Monorepo Solution:** They created a new package, `packages/hooks`. They moved the `useUser()` hook into it. Both the PWA and the admin app then added `"hooks": "workspace:*"` as a dependency.
- **The Result:** There was now a single source of truth for the data-fetching logic. The code was de-duplicated. Any improvements or bug fixes made to the hook were instantly and safely applied to both applications. They could refactor with confidence, knowing that all consumers of the hook would get the update simultaneously.

## 🤔 **Reflective Questions**

1. Besides UI components and hooks, what are other good candidates for a shared package?
2. What is the purpose of the `"exports"` field in the shared package's `package.json`? How does it improve encapsulation?
3. How can you share TypeScript configurations (`tsconfig.json`) across your monorepo to ensure all packages and apps follow the same compiler rules?

## 📚 **Further Reading**

- **Turborepo Docs:** [Sharing Code](https://turbo.build/repo/docs/handbook/sharing-code)
- **NPM Docs:** [workspaces](https://docs.npmjs.com/cli/v10/using-npm/workspaces)

## 📝 **Mini Task (Production)**

Time to create your own shared utility.

1. In your monorepo, create a new directory: `packages/utils`.
2. Give it a `package.json` that defines its name as `utils`.
3. Create a file `packages/utils/index.ts` and add a simple exported function, like `export const sayHello = () => 'Hello from shared utils';`.
4. Add the `utils` package as a dependency to your PWA (`npm install utils --workspace=pwa`).
5. In your PWA's `App.tsx`, import `sayHello` from `utils` and call it, displaying the result on the screen.
6. This demonstrates the core workflow of creating and consuming a shared logic package.