# 💡 AT01.3: The Clean Code Blueprint: Organizing Your Features

**Outline:**

- **The Feature's Public API (Presentation):** Using `index.ts` barrels to define clear boundaries for your features.
- **The Anatomy of a Feature (Practice):** A detailed breakdown of the subfolders and their responsibilities.
- **Building Your Shared Library (Production):** Identifying and extracting shared code into a common `lib` folder.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Feature's Public API**

A feature folder shouldn't be a free-for-all where other parts of your app can reach in and grab any file they want. This would re-create the same coupling problem we're trying to solve. Instead, we should treat each feature like a well-defined package or library. It has a **public API**, and the rest of the app should only interact with what the feature explicitly exposes.

We enforce this discipline using a simple but powerful convention: an `index.ts` file at the root of the feature folder, often called a "barrel file." This file has one job: to re-export the specific components, hooks, and functions that this feature intends to share with the outside world.

Other features will then import from the feature's root (`@/features/profile`), not from a deep, internal path (`@/features/profile/components/Avatar`). This creates a stable interface. You can refactor the internal file structure of your feature as much as you want, and as long as you don't change the exports in your `index.ts`, you won't break any other part of the application.

### **Part 2: Practice - The Anatomy of a Feature**

Let's define a robust, scalable template for the subdirectories inside a feature folder.

```
/features/checkout/
├── api/             # Contains functions for API calls (e.g., postOrder) and data transformation.
│   └── checkoutApi.ts
├── assets/          # Images, icons, or other static files used only by this feature.
├── components/      # React components used to build the feature's UI.
│   ├── ShippingForm.tsx
│   └── PaymentDetails.tsx
├── hooks/           # Custom hooks containing the feature's business logic.
│   └── useCheckout.ts
├── state/           # State management logic (e.g., a Redux slice for the checkout process).
│   └── checkoutSlice.ts
├── types/           # TypeScript types and interfaces specific to the checkout domain.
│   └── index.ts
├── utils/           # Helper functions used only within this feature.
│   └── validation.ts
└── index.ts         # The Public API of the checkout feature.

```

**The Public API (`index.ts`):** This file might look like this:

```ts
// features/checkout/index.ts

// We only expose the main component, not the internal form details.
export { CheckoutPage } from './components/CheckoutPage';

// We might expose a hook if other features need to interact with checkout state.
export { useCheckoutStatus } from './hooks/useCheckoutStatus';
```

This blueprint provides a clear separation of concerns _within_ the feature itself, making it easy to navigate and maintain.

## **🧠 Real-World Case Study: "The Shared UI Kit"**

- **The Problem:** A team using a feature-based architecture noticed that the `authentication`, `profile`, and `settings` features all had their own, slightly different implementations of a `<Button />` and `<InputField />` component. This led to UI inconsistencies and duplicated code.
- **The Realization:** They understood that some code doesn't belong to any single feature but is a foundational element used by _many_ features.
- **The Solution:** They created a new top-level directory: `src/lib/` (or `src/shared/`). Inside it, they built a `ui/` sub-folder to house their common design system components (`<Button>`, `<Card>`, `<Modal>`, etc.). They also created a `utils/` folder for globally useful helper functions (like `formatDate`).
- **The Result:** Features could now import from `@/lib/ui` to get consistent, reusable components. Code duplication was eliminated, and the entire PWA had a more polished and unified look and feel.

## 🤔 **Reflective Questions**

1. What is the main advantage of using an `index.ts` barrel file over allowing deep imports into a feature?
2. In the template above, what's the difference between `utils/` inside a feature and a top-level `lib/utils/` folder?
3. How can you use TypeScript's module resolution to enforce that features can't import from other features' internal files?

## 📚 **Further Reading**

- **Blog Post:** [Scaling your React/TypeScript project with feature-based architecture](https://www.google.com/search?q=https://dev.to/nicolaserny/scaling-your-react-typescript-project-with-feature-based-architecture-470a)
- **Barrel Files:** [Effective Code Organization with Barrel Files in TypeScript](https://www.google.com/search?q=https://medium.com/%40tomaszferens/effective-code-organization-with-barrel-files-in-typescript-8b6539563234)

## 📝 **Mini Task (Production)**

It's time to start extracting shared logic.

1. Look at your codebase and identify one UI component (like a custom button, card, or spinner) that is used in more than one feature or page.
2. Create a new top-level folder: `src/lib/ui/`.
3. Move the shared component's file into this new directory.
4. Update all the files that were importing it to use the new path (e.g., `import { Button } from '@/lib/ui/Button'`). This is your first step toward building a reusable component library.