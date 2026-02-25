💡 AA01-5.3: The Custom `useAuth()` Hook

**Outline:**

- **The Threat (SEEI):** Understanding the architectural problems of tight coupling between components and a specific state management library.
- **The Defense (PPP):** Decoupling components from the implementation details of the auth store by creating a custom hook as a facade.
- **Your Mission (Production):** Time to create and implement your own `useAuth` hook.

### 📘 Full Lesson Content

### **Part 1: Presentation - The Threat (SEEI)**

- **What is it? (The "Vulnerability"):** **Tight Coupling & Implementation Leakage.** When your components directly import `useAuthStore` from Zustand, they become _tightly coupled_ to Zustand. This means your components "know" that you are using Zustand. If you ever decide to migrate to a different state manager (like Redux, Jotai, or React Context), you would have to go into _every single component_ that uses the store and refactor it. This is a massive, time-consuming, and risky undertaking. The implementation details of your state management have "leaked" into your entire application.
    
- **How it Works (The Bad Pattern):**
    
    ```tsx
    // BAD PATTERN: DIRECTLY USING THE ZUSTAND STORE IN A COMPONENT
    
    import { useAuthStore } from '../store/authStore'; // <-- Tight coupling to Zustand
    
    function UserProfile() {
      // Verbose: We have to select each piece of state individually.
      const user = useAuthStore((state) => state.user);
      const login = useAuthStore((state) => state.login);
      const logout = useAuthStore((state) => state.logout);
    
      // This component is now dependent on Zustand's specific API.
      // Changing the state library would require changing this file.
    
      return (
        // ... component JSX
      );
    }
    
    ```
    
    Multiply this by 50 components, and you have a significant refactoring challenge if your architecture needs to evolve.
    

### **Part 2: Practice - The Defense (PPP)**

- **The Golden Rule:** **Depend on abstractions, not on concretions.** Your components shouldn't care _how_ the auth state is managed, only that they can _get_ it. A custom hook (`useAuth`) acts as this abstraction layer, or a "facade." It provides a simple, consistent interface to the rest of your app, while hiding the complex or specific implementation details inside itself.
    
- **Secure Code Implementation:**
    
    1. **Create the custom hook:**
        
        ```tsx
        // hooks/useAuth.ts
        import { useMemo } from 'react';
        import { useAuthStore } from '../store/authStore'; // Zustand is now ONLY imported here.
        
        export const useAuth = () => {
          // Get the raw state and actions from the store.
          const { user, login, logout } = useAuthStore();
        
          // We can derive new, useful state.
          const isAuthenticated = !!user;
        
          // useMemo ensures that the object we return is stable and doesn't
          // cause unnecessary re-renders in consumer components.
          return useMemo(() => ({
            user,
            login,
            logout,
            isAuthenticated,
          }), [user, login, logout]);
        };
        
        ```
        
    2. **Use the hook in your components:**
        
        ```tsx
        // UserProfile.tsx (Now clean and decoupled)
        import { useAuth } from '../hooks/useAuth'; // <-- We depend on our own abstraction.
        
        function UserProfile() {
          // The component gets everything it needs from one simple call.
          const { user, logout, isAuthenticated } = useAuth();
        
          if (!isAuthenticated) {
            return <p>Please log in.</p>;
          }
        
          return (
            <div>
              <p>Welcome, {user.name}!</p>
              <button onClick={logout}>Log Out</button>
            </div>
          );
        }
        
        ```
        
    
    Now, if you want to switch from Zustand to Redux, you only have to rewrite the _inside_ of the `useAuth` hook. None of your components need to change.
    

### 🧠 Real-World Case Study: "The Universal Remote Control"

- **The Goal:** Control your home entertainment system.
- **Vulnerable Approach (Tight Coupling):** You have three separate remote controls: one for the TV, one for the soundbar, and one for the Blu-ray player. Each has a different layout and works in a specific way (`direct store usage`). If you buy a new soundbar, you have to throw away its old remote and learn a completely new one (`refactoring components`).
- **Secure Approach (Custom Hook):** You buy a universal remote control like a Logitech Harmony (`useAuth` hook). You program it once, telling it how to talk to your specific TV, soundbar, and Blu-ray player (`hook implementation`). Now, you only need one simple remote. If you replace your Sony soundbar with a Bose soundbar, you don't change how you use the remote; you just re-program the remote's internal settings (`update the hook`). The user experience remains simple and consistent.

### 🤔 Reflective Questions

1. What is the purpose of `useMemo` in our `useAuth` hook? What could happen in child components if we didn't wrap the returned object in `useMemo`?
2. Our custom hook derives the `isAuthenticated` state. What other useful, derived state could you add to the `useAuth` hook (e.g., `isAdmin`, `userInitials`)?
3. This pattern is an example of the "Facade" design pattern. How does this pattern help in managing complexity in large software projects?

### 📚 Further Reading

- **React Docs:** [Building Your Own Hooks](https://react.dev/learn/reusing-logic-with-custom-hooks)
- **Refactoring Guru:** [Facade Design Pattern](https://refactoring.guru/design-patterns/facade)
- **Blog Post:** [Stop using Zustand/Redux directly in your components](https://www.google.com/search?q=https://dev.to/glegoux/stop-using-zustand-redux-directly-in-your-components-4k8o)

### 📝 Mini-Task (Production)

You are tasked with creating a `useAuth` hook for your application and refactoring a component to use it.

1. Create a file `hooks/useAuth.ts`.
2. Inside this file, define and export a function `useAuth`.
3. This hook should get the `user` and `logout` function from your existing `useAuthStore`.
4. It should return an object containing:
    - The `user` object.
    - The `logout` function.
    - A boolean value `isAuthenticated`, which is `true` if `user` is not null.
5. Refactor the `UserMenu` component from Lesson 4.1 to use your new `useAuth` hook instead of calling `useAuthStore` directly.