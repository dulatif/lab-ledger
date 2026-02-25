# 💡 AT01.4: Scaling with Style: The Power of Convention

**Outline:**

- **The Unwritten Rules (Presentation):** Understanding why shared conventions are the glue that holds a large-scale architecture together.
- **Automating Consistency (Practice):** Setting up absolute imports and linters to enforce your architectural rules.
- **Codifying Your Rules (Production):** Creating a basic set of conventions for your team to follow.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Unwritten Rules**

A folder structure is a great start, but a truly scalable architecture also runs on a set of shared **conventions**. These are the agreed-upon rules and patterns that a team follows to ensure the codebase remains consistent, predictable, and easy to navigate, no matter who is writing the code.

Without conventions, entropy takes over. One developer uses `PascalCase.tsx` for components, another uses `kebab-case.js`. One developer uses relative imports (`../../../lib/utils`), another uses absolute paths (`@/lib/utils`). This small-scale chaos adds up, increasing cognitive load and making the codebase harder to maintain.

Establishing strong conventions is about reducing the number of trivial decisions a developer has to make. When you don't have to think about _how_ to name a file or _where_ to import from, you can focus your brainpower on the thing that actually matters: solving business problems. For a growing team, a documented set of conventions is one of the most valuable assets you can create.

### **Part 2: Practice - Automating Consistency**

The best conventions are the ones you don't have to think about because they are enforced automatically.

**1. Absolute Imports:** Relative imports are a scalability nightmare. When you move a file, all its relative imports break. Absolute imports, which start from the `src` directory, are stable. You enable them in your `tsconfig.json` (for TypeScript) or `jsconfig.json` (for JavaScript).

```json
// tsconfig.json
{
  "compilerOptions": {
    "baseUrl": "src", // Sets 'src' as the root for imports
    "paths": {
      "@/*": ["*"] // Creates an alias '@' for the 'src' directory
    }
  },
  "include": ["src"]
}
```

Now, you can write `import { Button } from '@/lib/ui/Button'` from any file in your project. It's clean, readable, and refactor-proof.

**2. Linting Rules for Architecture:** We can use ESLint to enforce our architectural rules. For example, the `eslint-plugin-import` package can be configured to prevent developers from using deep, relative imports that cross feature boundaries.

	```js
// .eslintrc.js excerpt
{
  "rules": {
    "import/no-restricted-paths": [
      "error",
      {
        "zones": [
          {
            "target": "./src/features",
            "from": "./src/features",
            "message": "Cross-feature imports are not allowed. Import from the feature's index.ts or from a shared lib."
          }
        ]
      }
    ]
  }
}
```

This rule would throw an error if a file in `features/checkout` tried to import directly from `features/profile/components/SomeComponent.tsx`, forcing them to use a shared library or the feature's public API.

## **🧠 Real-World Case Study: "The Onboarding Guide"**

- **The Problem:** A PWA project was growing its team from 5 to 15 developers in one quarter. The original developers had a lot of implicit knowledge about the codebase, but new hires were struggling, frequently putting files in the wrong place or introducing inconsistent patterns.
- **The Solution:** The tech lead created a `CONTRIBUTING.md` file in the root of the repository. This document didn't just show the folder structure; it explained the _philosophy_ behind it. It included sections on:
    - Our Architecture (Feature-based).
    - Naming Conventions (`use...` for hooks, `...Page.tsx` for route components).
    - State Management Guidelines.
    - How to create a new feature folder from a template.
    - Rules enforced by the linter.
- **The Result:** Onboarding time for new developers was cut by more than half. Code reviews became faster and more focused on logic because stylistic and architectural debates were already settled by the guide and the linter. The document became the "source of truth" for how to build things "the right way" on their team.

## 🤔 **Reflective Questions**

1. Why are absolute imports so much better for maintainability in a large project compared to relative imports?
2. What is one naming convention you find particularly helpful (e.g., for hooks, components, API functions)?
3. Besides import restrictions, what is another architectural rule you could potentially enforce using an ESLint plugin?

## 📚 **Further Reading**

- **TypeScript Docs:** [Path mapping](https://www.google.com/search?q=https://www.typescriptlang.org/docs/handbook/module-resolution.html%23path-mapping) (Official docs for `baseUrl` and `paths`).
- **ESLint Plugin:** [`eslint-plugin-import`](https://github.com/import-js/eslint-plugin-import) (A powerful tool for controlling your module boundaries).

## 📝 **Mini Task (Production)**

Let's establish your first, most important convention.

1. If you don't already have one, create a `tsconfig.json` or `jsconfig.json` file at the root of your project.
2. Add the `baseUrl` and `paths` configuration to enable absolute imports with an `@/*` alias, as shown in the example above.
3. Find one file deep in your project that uses a messy relative import like `import { something } from '../../../lib/utils'`.
4. Refactor it to use your new, clean absolute import: `import { something } from '@/lib/utils'`.
5. This small change is a huge step towards a more scalable and maintainable codebase.