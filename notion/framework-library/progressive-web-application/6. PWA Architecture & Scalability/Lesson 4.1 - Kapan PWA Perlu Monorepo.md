# 💡 AT01.4.1: The Bigger Picture: When Does a PWA Need a Monorepo?

**Outline:**

- **The Scaling Problem (Presentation):** Understanding the point where managing multiple related codebases becomes a bottleneck.
- **The Monorepo Philosophy (Practice):** A conceptual walkthrough of structuring multiple apps and shared packages in one repository.
- **To Monorepo or Not? (Production):** Analyzing a project's needs to justify the architectural shift.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Scaling Problem**

Team, as our projects and organizations grow, we often build more than just a single PWA. We might have:

- The PWA itself.
- A public marketing website.
- An admin dashboard.
- Maybe even a Node.js backend.

Very quickly, you'll find that these separate projects need to share code. They need the same UI components for brand consistency, the same TypeScript types to talk to the API, and the same utility functions to avoid re-writing logic.

The traditional approach is to manage these in separate repositories (a "polyrepo" setup). You'd publish your shared code as private NPM packages. This works, but it creates immense friction. To fix a bug in a shared button, you have to:

1. Make the change in the UI library repo.
2. Increment the version number.
3. Publish it to NPM.
4. Go into your PWA repo and your marketing site repo.
5. Update the package version.
6. Hope you didn't introduce any breaking changes.

This is slow, painful, and error-prone. A **monorepo** solves this by placing all your related projects into a single Git repository. It's not about throwing all your company's code into one giant folder; it's a strategic decision to co-locate projects that share significant logic and dependencies.

### **Part 2: Practice - The Monorepo Philosophy**

A well-structured monorepo isn't just one big `src` folder. It's organized into two key directories:

1. **`apps/`**: Contains the actual applications—things you can build and deploy. Each subfolder is a standalone project.
    - `apps/pwa/`
    - `apps/web/` (your marketing site)
    - `apps/docs/`
2. **`packages/`**: Contains the shared code—the libraries that your apps depend on. These are not deployable on their own.
    - `packages/ui/` (your React component library)
    - `packages/shared-types/` (shared TypeScript interfaces)
    - `packages/utils/` (common helper functions)
    - `packages/eslint-config-custom/` (shared linting rules)

With this structure, making a change is atomic. You can update a component in `packages/ui` and instantly see how it affects both the PWA and the website in a single commit and a single pull request. Tooling like Turborepo (which we'll cover next) makes managing this a breeze, ensuring that only the affected parts of your codebase are rebuilt.

## 🧠 **Real-World Case Study: "The Inconsistent Button"**

- **The Problem:** A company had a beautiful PWA and a Next.js marketing site, each in its own repo. They shared a private NPM package for their UI components. A designer noticed that the primary button in the PWA had a slightly different hover effect than the one on the marketing site.
- **The Investigation:** It turned out that the PWA was using `ui-library@1.2.5` while the marketing site was on `ui-library@1.3.1`. A developer had updated one but forgotten the other. This small inconsistency was a symptom of a larger problem: their codebases were constantly drifting out of sync.
- **The Switch to Monorepo:** They migrated both apps and the UI library into a single monorepo. Now, there was only one version of the UI library—the one currently in the `packages/ui` folder. It was impossible for the apps to be out of sync.
- **The Result:** The UI became perfectly consistent. Refactoring shared components became trivial, encouraging developers to build more reusable code. The entire development process felt more integrated and less fragmented.

## 🤔 **Reflective Questions**

1. What are the potential downsides of using a monorepo? (Hint: think about repository size, build times without smart tooling, and access control).
2. At what point does the pain of managing multiple repos outweigh the initial simplicity of a single-repo project?
3. How does a monorepo improve the developer experience for tasks like end-to-end testing?

## 📚 **Further Reading**

- **Monorepo.tools:** [A collection of resources about monorepos](https://monorepo.tools/)
- **Turborepo Docs:** [What is a Monorepo?](https://turbo.build/repo/docs/core-concepts/monorepos)

## 📝 **Mini Task (Production)**

Imagine you are starting a new project for a client. The project has the following components:

- A complex, offline-first PWA for managing inventory.
- A public-facing e-commerce storefront built with Next.js.
- A shared design system (buttons, forms, layout components).
- A shared set of TypeScript types for the product and inventory APIs.

Your task: On paper or in a markdown file, design the ideal monorepo folder structure for this project. List the folders you would create under `apps/` and `packages/`. This will be the blueprint for your scalable architecture.