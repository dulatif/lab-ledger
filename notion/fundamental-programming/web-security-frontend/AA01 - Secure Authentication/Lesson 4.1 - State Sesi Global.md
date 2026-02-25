💡 AA01-4.1: Global Session State

**Outline:**

- **The Threat (SEEI):** Understanding the chaos of "prop drilling" and having a disconnected, inconsistent UI.
- **The Defense (PPP):** Introducing a single source of truth for authentication state using a lightweight state manager like Zustand.
- **Your Mission (Production):** Time to create a global auth store.

### 📘 Full Lesson Content

### **Part 1: Presentation - The Threat (SEEI)**

- **What is it? (The "Vulnerability"):** **Inconsistent State & Prop Drilling.** This isn't a traditional security vulnerability, but an architectural one that _leads_ to security-related bugs. When your authentication state (e.g., the user object) isn't managed globally, you end up passing it down through component props (`<App user={user}><Navbar user={user}><UserMenu user={user} />`). This is "prop drilling." It's inefficient, but the real danger is when different components fetch or manage their own piece of the auth state. You can end up with a Navbar that thinks the user is logged in, while the main content area thinks they are logged out, leading to a confusing and broken UI that might expose data incorrectly.
    
- **How it Works (The Bad Pattern):**
    
    ```ts
    // BAD PATTERN: PROP DRILLING AND DISCONNECTED STATE
    
    function App() {
      const [user, setUser] = useState(null);
      // Imagine fetching the user here...
      return <Navbar user={user} />;
    }
    
    function Navbar({ user }) {
      return (
        <nav>
          {/* ... branding ... */}
          <UserMenu user={user} />
        </nav>
      );
    }
    
    function UserMenu({ user }) {
      // This is 3 levels deep. What if a component 10 levels deep needs the user?
      return user ? <div>Welcome, {user.name}</div> : <a href="/login">Login</a>;
    }
    
    // Now, imagine another component on the page fetches its own state.
    // What if its request fails but the main App's request succeeded? Inconsistent UI!
    
    ```
    
    This becomes impossible to manage in a large application. A change in one place doesn't reliably update the others.
    

### **Part 2: Practice - The Defense (PPP)**

- **The Golden Rule:** **Establish a single source of truth.** All components in your application should read authentication state from one, and only one, global store. When that store updates, every component that uses it will re-render automatically and consistently. Libraries like Zustand, Redux Toolkit, or React's Context API solve this problem. We'll use Zustand for its simplicity.
    
- **Secure Code Implementation:**
    
    1. **Install Zustand:** `npm install zustand`
        
    2. **Create the Auth Store:**
        
        ```ts
        // store/authStore.ts
        import { create } from 'zustand';
        import { persist } from 'zustand/middleware'; // For localStorage persistence
        
        interface AuthState {
          user: { name: string; email: string } | null;
          // The login function will receive user data from the API
          login: (user: { name: string; email: string }) => void;
          // The logout function will clear the state
          logout: () => void;
        }
        
        // Create the store
        export const useAuthStore = create<AuthState>()(
          // Use the persist middleware to automatically save to localStorage
          persist(
            (set) => ({
              user: null,
              login: (userData) => set({ user: userData }),
              logout: () => set({ user: null }),
            }),
            {
              name: 'auth-storage', // The key to use in localStorage
            }
          )
        );
        
        ```
        
    3. **Use the Store in any component:**
        
        ```ts
        // UserMenu.tsx (No more props!)
        import { useAuthStore } from '../store/authStore';
        
        function UserMenu() {
          // Select the piece of state you need.
          const user = useAuthStore((state) => state.user);
          const logout = useAuthStore((state) => state.logout);
        
          if (!user) {
            return <a href="/login">Login</a>;
          }
        
          return (
            <div>
              <span>Welcome, {user.name}</span>
              <button onClick={logout}>Logout</button>
            </div>
          );
        }
        
        ```
        
    
    Now, if `logout()` is called from anywhere in the app, the `UserMenu` (and every other component subscribed to the store) will instantly and automatically re-render with the correct state.
    

### 🧠 Real-World Case Study: "The Town Square Billboard vs. Word of Mouth"

- **The Goal:** Ensure every citizen in a town knows the current official news.
- **Vulnerable Approach (Prop Drilling):** The mayor tells two people the news. Those two people each tell two more people. As the message spreads ("word of mouth"), it gets distorted. Some people hear the wrong version, others never hear it at all. The town is in a state of confusion and inconsistent information.
- **Secure Approach (Global State):** The mayor posts the official news on a giant billboard in the town square (`the global store`). Everyone in town knows that to get the real, up-to-date news, they must look at the billboard (`useAuthStore`). There is no confusion or inconsistency. When the mayor updates the billboard, everyone sees the new information at the same time.

### 🤔 Reflective Questions

1. We used Zustand's `persist` middleware. What are the benefits of this for the hybrid storage strategy we discussed in Module 1?
2. React's built-in Context API can also be used for global state. What are some of the performance trade-offs between using Context vs. a dedicated library like Zustand or Redux?
3. How does having a global state management store simplify the logic inside our API interceptors?

### 📚 Further Reading

- **Zustand:** [Official Documentation](https://docs.pmnd.rs/zustand/getting-started/introduction)
- **Redux Toolkit:** [Official Documentation](https://redux-toolkit.js.org/)
- **React Docs:** [Passing Data Deeply with Context](https://react.dev/learn/passing-data-deeply-with-context)

### 📝 Mini-Task (Production)

You are tasked with creating the initial authentication store for your application using Zustand.

Your task: Create a file `authStore.ts`. In this file, define and export a Zustand store called `useAuthStore`. The store should manage the following state and actions:

- `user`: Can be an object `{ id: string; email: string; }` or `null`. It should start as `null`.
- `token`: Can be a `string` or `null`. It should start as `null`. (Even though we store tokens in cookies, we might want the access token's _payload_ in memory).
- `setAuthState`: An action that takes both a `user` and a `token` and sets them in the state.
- `clearAuthState`: An action that resets both `user` and `token` back to `null`.