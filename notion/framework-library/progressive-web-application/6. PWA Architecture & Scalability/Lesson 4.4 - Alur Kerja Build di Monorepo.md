# 💡 AT01.4.4: The Assembly Line: The Monorepo Build Workflow

**Outline:**

- **The Dependency Graph (Presentation):** How Turborepo understands the relationships between tasks in your monorepo.
- **Building the Pipeline (Practice):** A deep dive into configuring `turbo.json` for a robust build process.
- **Caching in Action (Production):** A hands-on experiment to see the performance benefits of Turbo's cache.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Dependency Graph**

The real power of Turborepo comes from how it understands your project's structure. It reads the `package.json` of every app and package to build a **dependency graph**. It knows, for example, that `apps/pwa` depends on `packages/ui`.

When you define a task pipeline in `turbo.json`, Turborepo uses this graph to execute tasks in the most efficient way possible.

Let's look at this configuration again:

```json
// turbo.json
"build": {
  "dependsOn": ["^build"],
  "outputs": ["dist/**"]
}
```

When you run `turbo run build --filter=pwa`, Turborepo performs the following steps:

1. **Analyze:** It looks at the `pwa` project.
2. **Check Dependencies:** It sees that `pwa` depends on `ui`.
3. **Resolve Pipeline:** The `build` task `dependsOn` `^build`. The `^` symbol tells it to look "up" the dependency graph.
4. **Execute:** Before it runs `build` for `pwa`, it first runs the `build` task for the `ui` package.
5. **Cache:** Once the `ui` package is built, Turborepo caches the result. Then it builds the `pwa` and caches its result.

This ensures that everything is always built in the correct order. The next time you run the command, if nothing has changed, it will find everything in the cache and finish instantly.

## **Part 2: Practice - Building the Pipeline**

Let's create a more realistic pipeline. We want to ensure that before any app is built, all its dependencies are linted and built first.

```json
// turbo.json
{
  "$schema": "<https://turbo.build/schema.json>",
  "pipeline": {
    "build": {
      // An app's build depends on its dependencies' `build` tasks.
      "dependsOn": ["^build"],
      "outputs": ["dist/**", ".next/**"]
    },
    "lint": {
      // Linting can run on its own.
    },
    "#lint-and-build": {
      // This is a "transient" task, just for defining a dependency.
      // It depends on the local `lint` and `build` tasks.
      "dependsOn": ["lint", "build"]
    },
    "dev": {
      "cache": false,
      "persistent": true
    }
  }
}
```

We can define more complex relationships. For example, we could make the top-level `build` task depend on the `test` task, ensuring that no code is built unless all tests are passing. The `turbo.json` file is the central nervous system that orchestrates your entire development and CI workflow.

## **🧠 Real-World Case Study: "The Vercel Monorepo"**

- **The Setup:** The team at Vercel (the creators of Next.js and Turborepo) uses a massive monorepo to manage their projects. They have dozens of apps and shared packages.
- **The Workflow:** They use Turborepo's remote caching feature, connected to their own Vercel account. When a developer runs `turbo build` on their laptop, the build artifacts are uploaded to the remote cache.
- **The Payoff:** When a different developer pulls the latest code and runs `turbo build`, Turborepo first checks the remote cache. If the code they are trying to build has already been built by their teammate, it just downloads the artifacts instead of re-running the build. The same thing happens in CI. This means they almost never build the same code twice across the entire organization. This saves thousands of developer hours and drastically reduces CI costs.

## 🤔 **Reflective Questions**

1. How does Turborepo know if a file has changed and a task needs to be re-run? What does it use to create the cache key?
2. What does `turbo run build --filter=pwa` do? How is it different from just `turbo run build`?
3. How could you use Turborepo to manage deployment scripts for your different applications?

## 📚 **Further Reading**

- **Turborepo Docs:** [Filtering Workspaces](https://www.google.com/search?q=https://turbo.build/repo/docs/reference/command-line-reference%23--filter-glob)
- **Turborepo Docs:** [Remote Caching](https://www.google.com/search?q=https://turbo.build/repo/docs/core-concepts/remote-caching)

## 📝 **Mini Task (Production)**

Let's see the cache in action and feel the speed.

1. From the root of your monorepo, run the build command: `npm run build`. Note how long it takes.
2. Open a file in your shared `ui` package (e.g., `packages/ui/Button.tsx`) and make a small, trivial change (like adding a comment).
3. Run `npm run build` again.
4. Observe the output in your terminal. You should see that Turborepo found the `pwa` and `ui` packages in its cache but re-ran the build for `ui` because you changed a file. Then, because `pwa` depends on `ui`, it also re-ran the build for `pwa`.
5. Now, run `npm run build` one more time _without changing any files_. Observe that it hits "FULL TURBO" for everything and finishes in milliseconds. This is the core magic of Turborepo's build system.