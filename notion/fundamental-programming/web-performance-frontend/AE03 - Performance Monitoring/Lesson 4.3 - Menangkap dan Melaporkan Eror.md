### **💡 AE01-4.3: Mastering Error Capture: Automatic vs. Manual Reporting**

**Outline:**

- **The Metric & The Bottleneck (Presentation):** The problem of "silent failures" where errors are handled but not reported.
- **The Optimization Technique (Practice):** Using `Sentry.captureException` to manually report handled errors.
- **Your Performance Mission (Production):** Time to implement both automatic and manual error capture.

### 📘 **Full Lesson Content**

### **Part 1: Presentation - The Metric & The Bottleneck**

- **The Goal:** Our goal is to learn how to capture and report _all_ meaningful errors, not just the ones that crash our application. This includes errors that we anticipate and handle gracefully in our code.
- **Identifying the Problem:** Sentry automatically catches _unhandled_ exceptions, which is fantastic. But what happens when you write a `try...catch` block? You've "handled" the error, so your app doesn't crash, and Sentry's automatic instrumentation won't report it. The user might see a message like "Something went wrong," but you, the developer, have no record that this failure occurred. This is a **silent failure**, and it can hide significant underlying problems with your APIs or business logic.

### **Part 2: Practice - The Optimization Technique**

- **The Principle:** A robust error monitoring strategy combines automatic capture of unhandled exceptions with deliberate, manual capture of handled exceptions that are still important to know about.
    
- **Automatic Capture (The Safety Net):** This is what you get for free after `Sentry.init()`. It catches things you didn't anticipate.
    
    ```ts
    // This will be caught automatically by Sentry
    function causeUnhandledError() {
      const user = null;
      console.log(user.name); // TypeError: Cannot read properties of null
    }
    
    ```
    
- **Manual Capture (The Signal):** For errors you anticipate, you use `Sentry.captureException()` inside your `catch` block. This tells Sentry, "I expected this might fail, and I've handled it, but I still want you to record this event."
    
    ```ts
    import * as Sentry from "@sentry/react";
    
    async function fetchUserData(userId) {
      try {
        const response = await fetch(`/api/users/${userId}`);
        if (!response.ok) {
          // This is an expected failure condition
          throw new Error(`API responded with status ${response.status}`);
        }
        return await response.json();
      } catch (error) {
        // We handle the error gracefully for the user
        showErrorMessage("Could not load user data.");
    
        // But we still report it to Sentry for analysis
        Sentry.captureException(error);
    
        return null;
      }
    }
    
    ```
    

### 🧠 **Real-World Case Study: "The API's Bad Habits"**

- **The Scenario:** A fintech app uses a third-party service to get stock market data. The developers know the service can sometimes be flaky, so they responsibly wrap all API calls in `try...catch` blocks. If a call fails, they show the user cached data.
- **The Problem:** The app feels stable to users, but the developers have no idea how often the third-party API is actually failing. Is it once a day? Or 50% of the time?
- **The Solution:** They add `Sentry.captureException(error)` to every `catch` block that handles the API calls. The next day, their Sentry dashboard is flooded. They discover the API has a 30% failure rate during peak market hours. Armed with this concrete data, they are able to go to the API provider and prove there's a problem, leading to a fix. Manual reporting turned their "hidden" problem into an actionable insight.

### 🤔 **Reflective Questions**

1. Is it a good idea to call `Sentry.captureException()` for _every single_ `catch` block in your application? Why or why not? What kind of "noise" might this create?
2. Sentry also provides `Sentry.captureMessage()`. How is this different from `captureException()`? When might you use it? (Hint: think about logging events that aren't technically errors).
3. How can you add more context to a manually captured error? For example, how would you tell Sentry which `userId` was affected? (Hint: look up `Sentry.withScope`).

### 📚 **Further Reading**

- **Sentry Docs:** [Capture Errors Manually](https://www.google.com/search?q=https://docs.sentry.io/platforms/javascript/guides/react/usage/capturing-errors/)
- **Sentry Docs:** [Adding Context](https://docs.sentry.io/platforms/javascript/guides/react/enriching-events/context/)
- **MDN:** [`try...catch` statement](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch)

### 📝 **Mini Task (Production)**

- **Your Mission:** Create a simple React component that demonstrates both automatic and manual error capture.
    1. Ensure you have Sentry initialized in your project.
    2. Create a component with two buttons: "Cause Unhandled Error" and "Cause Handled Error".
    3. The first button's `onClick` handler should call a function that tries to access a property on `null` to cause an unhandled `TypeError`.
    4. The second button's `onClick` handler should wrap a call that throws an error (`throw new Error("This was a handled error!")`) inside a `try...catch` block.
    5. Inside the `catch` block, call `Sentry.captureException()`.
    6. Click both buttons and verify that two distinct issues appear in your Sentry dashboard.