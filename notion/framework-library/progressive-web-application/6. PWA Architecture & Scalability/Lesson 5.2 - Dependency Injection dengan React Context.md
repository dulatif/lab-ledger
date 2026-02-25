# 💡 AT01.5.2: The Universal Toolkit: Dependency Injection with Context

**Outline:**

- **The Coupling Problem (Presentation):** Understanding why hard-coded dependencies make components rigid and hard to test.
- **The Context Provider (Practice):** A step-by-step guide to providing and consuming dependencies with the React Context API.
- **The Mockable Component (Production):** Using the pattern to make a component easily testable.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Coupling Problem**

In a large application, components often need to interact with cross-cutting concerns like an analytics service, a theming engine, or an API client. A common but problematic approach is to have each component import and instantiate these services directly.

```tsx
// Bad: Hard-coded dependency
import { analyticsService } from '../services/analytics';

function BuyButton() {
  const handleClick = () => {
    analyticsService.track('product_purchased');
    // ...
  };
  return <button onClick={handleClick}>Buy Now</button>;
}
```

This component is now **tightly coupled** to the concrete `analyticsService`. This creates two major problems:

1. **It's not flexible:** What if we want to use a different analytics service in another part of the app, or disable it during development? We'd have to change the code in every component.
2. **It's hard to test:** How can we write a unit test for `BuyButton` without actually sending events to our real analytics service? It's very difficult.

The solution is **Dependency Injection (DI)**. Instead of the component creating its own dependencies, the dependencies are provided (or "injected") from a higher level. In React, the simplest and most idiomatic way to achieve this is with the **React Context API**.

### **Part 2: Practice - The Context Provider**

Let's refactor the `BuyButton` to use DI via Context.

**Step 1: Create the Context** This will hold our dependency.

```ts
// contexts/AnalyticsContext.ts
import { createContext, useContext } from 'react';
import { analyticsService } from '../services/analytics'; // The real service

// Create a context with the real service as the default value
export const AnalyticsContext = createContext(analyticsService);

// Create a custom hook for easy consumption
export const useAnalytics = () => useContext(AnalyticsContext);
```

**Step 2: Provide the Context at the top of your app**

```tsx
// App.tsx
import { AnalyticsContext } from './contexts/AnalyticsContext';
import { analyticsService } from './services/analytics';
// ...

function App() {
  return (
    <AnalyticsContext.Provider value={analyticsService}>
      {/* The rest of your app */}
    </AnalyticsContext.Provider>
  );
}
```

**Step 3: Consume the Context in the component** The component is now decoupled from the concrete implementation.

```tsx
// Good: Injected dependency
import { useAnalytics } from '../contexts/AnalyticsContext';

function BuyButton() {
  const analytics = useAnalytics(); // Get the service from context

  const handleClick = () => {
    analytics.track('product_purchased');
    // ...
  };
  return <button onClick={handleClick}>Buy Now</button>;
}
```

Our component is now flexible and easy to test. In a test environment, we can simply wrap it in a `AnalyticsContext.Provider` and pass in a mock service.

## 🧠 **Real-World Case Study: "The Theming Engine"**

- **The Challenge:** A large PWA needed to be "white-labeled" for different clients, each with their own color scheme, logos, and fonts. Hard-coding theme values into hundreds of components was not an option.
- **The Solution:** They created a `ThemeContext`. At the root of the application, they fetched the configuration for the current client and provided a `theme` object via the context provider.
- **The Implementation:** Every component that needed theme information (buttons, headers, etc.) used a `useTheme()` custom hook to access the colors and fonts. The components themselves had no knowledge of which specific client's theme they were rendering.
- **The Result:** They could create a new theme for a new client just by adding a new JSON configuration file. No component code needed to be changed. The system was incredibly flexible and scalable.

## 🤔 **Reflective Questions**

1. Besides an API client or theme, what are other examples of cross-cutting concerns that would be good candidates for DI with Context?
2. What is "prop drilling" and how can the Context API help solve it? What are the potential performance downsides of using Context?
3. How does this pattern relate to the "D" in the SOLID principles (Dependency Inversion Principle)?

## 📚 **Further Reading**

- **React Docs:** [Passing Data Deeply with Context](https://react.dev/learn/passing-data-deeply-with-context)
- **Kent C. Dodds:** [How to use React Context effectively](https://kentcdodds.com/blog/how-to-use-react-context-effectively)

## 📝 **Mini Task (Production)**

Your PWA needs an API client that's configured with the correct base URL for different environments (development vs. production).

1. Create a simple `apiClient` object that has a `get` method.
2. Create an `ApiContext` to hold this client.
3. In your `App.tsx`, instantiate the client with the correct base URL and provide it to the rest of the app using `ApiContext.Provider`.
4. Create a `useApiClient` custom hook.
5. Refactor a component that currently uses `fetch` directly to use the `get` method from the `useApiClient` hook instead. This decouples your component from the global `fetch` API and the specific base URL.