# 💡 AT01.4.2: The Build Engine: Setting Up a PWA with Turborepo

**Outline:**

- **The Smart Build System (Presentation):** Introducing Turborepo and its core concepts: caching and task pipelines.
- **Creating Your Monorepo (Practice):** A step-by-step guide to initializing a new Turborepo project.
- **Your First Turbo Build (Production):** Adding a PWA app and running a build across the entire repo.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smart Build System**

A monorepo would be impossibly slow without a smart build system. You can't rebuild every app and every package on every single change. This is where **Turborepo** comes in.

Turborepo is a high-performance build system for JavaScript and TypeScript codebases. It's not a replacement for Vite or Next.js; it's a "meta" tool that sits on top of them and orchestrates all the tasks (like `build`, `lint`, `test`) across your entire repository.

Its two superpowers are:

1. **Caching:** Turborepo "remembers" the output of every task it runs. If you run `turbo build` and the code in your PWA hasn't changed since the last build, Turborepo will skip the work and instantly restore the output from its cache. This is incredibly fast.
2. **Task Pipelines:** You can define dependencies between tasks. For example, you can tell Turborepo that before it can `build` the PWA, it must first `build` the shared `ui` package that the PWA depends on. It understands your repo's structure and runs tasks in the correct order, in parallel whenever possible.

Turborepo is the engine that makes the monorepo dream a reality, keeping your development workflow fast and efficient, even as the codebase grows.

### **Part 2: Practice - Creating Your Monorepo**

Let's initialize a new project. The Turborepo CLI makes this easy.

**Step 1: Create the monorepo**

```bash
npx create-turbo@latest my-pwa-monorepo
```

The CLI will ask you a few questions. Choose your package manager (e.g., `npm`). It will generate a starter project with a basic structure.

**Step 2: Explore the structure** You'll see the `apps/` and `packages/` directories we discussed, along with a key configuration file: `turbo.json`.

```json
// turbo.json
{
  "$schema": "<https://turbo.build/schema.json>",
  "pipeline": {
    "build": {
      // The `build` task depends on the `build` task of its internal dependencies
      "dependsOn": ["^build"],
      // The output of the build task will be in the `dist` or `.next` folder
      "outputs": ["dist/**", ".next/**"]
    },
    "lint": {
      // Linting has no dependencies
    },
    "dev": {
      "cache": false, // We don't want to cache the long-running dev server
      "persistent": true
    }
  }
}
```

The `^build` syntax in `dependsOn` is crucial. It means "the `build` tasks of all the packages this app depends on."

**Step 3: Run a build** From the root of your monorepo, run:

```bash
npm run build
```

This command is usually aliased to `turbo run build`. You'll see Turborepo analyze the dependency graph, run the builds for the packages, and then run the builds for the apps. The first time, it will do the work. If you run it again immediately, it will hit the cache and finish in milliseconds.

## 🧠 **Real-World Case Study: "The 45-Minute CI Pipeline"**

- **The Problem:** A large company had over a dozen related front-end apps and libraries in a monorepo managed by an older tool. Their Continuous Integration (CI) pipeline on every pull request had to rebuild and re-test _everything_ because the tool couldn't determine what had changed. The pipeline took 45 minutes to complete, which was a massive drag on developer productivity.
- **The Migration to Turbo:** They spent a week migrating to Turborepo. They configured `turbo.json` to define their task pipeline and set up remote caching, which shares the build cache across the entire team and the CI environment.
- **The Result:** The average CI pipeline time dropped from 45 minutes to under 5 minutes. Because of remote caching, if a developer's teammate had already built a specific package, CI could just download the result instead of re-doing the work. The team was able to merge pull requests faster, and the cost of their CI servers dropped significantly.

## 🤔 **Reflective Questions**

1. What does the `outputs` array in the `turbo.json` pipeline configuration do? Why is it essential for caching to work correctly?
2. How does Turborepo's caching differ from the cache you might find in a bundler like Vite or Webpack?
3. What's the difference between `dependsOn: ["build"]` and `dependsOn: ["^build"]`?

## 📚 **Further Reading**

- **Turborepo Docs:** [Core Concepts](https://turbo.build/repo/docs/core-concepts)
- **Turborepo Docs:** [Pipelines](https://www.google.com/search?q=https://turbo.build/repo/docs/core-concepts/pipelines)

## 📝 **Mini Task (Production)**

Time to get your hands dirty.

1. Run `npx create-turbo@latest` to scaffold a new monorepo.
2. Delete the starter apps inside the `apps/` directory.
3. Use your preferred tool (e.g., `npx create-vite@latest`) to create a new, blank PWA inside the `apps/` directory. Name it `pwa`.
4. Make sure your new PWA has a `build` script in its `package.json`.
5. From the root of your monorepo, run `npm run build`.
6. Verify that Turborepo successfully finds and executes the build script for your new PWA. Run it a second time to see the "FULL TURBO" cache hit in action.