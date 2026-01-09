# Lesson 3: App Config & Global Properties

## Learning Goals

- Bootstrapping the App instance.
- Global Error Handlers.

## 1. Mounting the App

In `main.ts`:

```typescript
import { createApp } from "vue";
import App from "./App.vue";

// 1. Create instance
const app = createApp(App);

// 2. Configure (Plugins, globals)
// app.use(router)

// 3. Mount to DOM
app.mount("#app");
```

## 2. Global Error Handling

You can catch errors from _any_ component.

```typescript
app.config.errorHandler = (err, instance, info) => {
  console.error("Global Error:", err);
  console.log("Info:", info); // e.g., "native event handler"
  // Send to Sentry/LogRocket here
};
```

## 3. Global Properties

Add properties available to all components (use sparingly).

```typescript
app.config.globalProperties.msg = "hello";
```

_Note: In Composition API, it's often better to use Provide/Inject instead of global properties._

## Key Takeaways

- `createApp` is the start of every Vue 3 app.
- No more `new Vue()` (Vue 2 style).
