💡 AA01-5.2: Implementing a `<ProtectedRoute>` Component

**Outline:**

- **The Threat (SEEI):** Understanding the maintenance nightmare of duplicated, inconsistent, and error-prone authorization logic.
- **The Defense (PPP):** Abstracting route protection logic into a single, reusable, and authoritative component.
- **Your Mission (Production):** Time to build and implement your own `<ProtectedRoute>` component.

### 📘 Full Lesson Content

### **Part 1: Presentation - The Threat (SEEI)**

- **What is it? (The "Vulnerability"):** **Duplicated Logic & Code Rot.** The inline protection pattern from the last lesson works, but it violates the DRY (Don't Repeat Yourself) principle. If you have 20 protected routes, you will have 20 copies of the same `user ? <Component /> : <Navigate />` logic. What happens when your requirements change? For example, you now need to check if the user's email is verified. You would have to find and update all 20 instances. It's almost certain a developer will miss one, leading to an inconsistent and insecure application.
    
- **How it Works (The Bad Pattern):**
    
    ```tsx
    // BAD PATTERN: DUPLICATED LOGIC EVERYWHERE
    
    const AppRouter = () => {
      const user = useAuthStore((state) => state.user);
    
      return (
        <Routes>
          <Route path="/login" element={<LoginPage />} />
    
          {/* Logic is copied... */}
          <Route
            path="/dashboard"
            element={user ? <DashboardPage /> : <Navigate to="/login" />}
          />
          {/* ...and pasted... */}
          <Route
            path="/settings"
            element={user ? <SettingsPage /> : <Navigate to="/login" />}
          />
          {/* ...and pasted again. This is not maintainable. */}
          <Route
            path="/profile"
            element={user ? <ProfilePage /> : <Navigate to="/login" />}
          />
        </Routes>
      );
    };
    
    ```
    
    This code is verbose, hard to read, and extremely brittle. A single change in auth requirements forces a widespread, error-prone refactor.
    

### **Part 2: Practice - The Defense (PPP)**

- **The Golden Rule:** **Encapsulate and centralize your security logic.** Create a single, reusable component (`<ProtectedRoute>`) that contains the authorization logic. This component becomes the single source of truth for route protection. If the rules change, you only have to update this one file.
    
- **Secure Code Implementation:** Let's build the component. It will act as a wrapper.
    
    ```tsx
    // 1. Create the ProtectedRoute component
    
    // components/ProtectedRoute.tsx
    import { Navigate, useLocation } from 'react-router-dom';
    import { useAuthStore } from '../store/authStore';
    
    // The component takes `children` which will be the page we want to render.
    const ProtectedRoute = ({ children }) => {
      const user = useAuthStore((state) => state.user);
      const location = useLocation();
    
      // If there is no user, we redirect to the login page.
      if (!user) {
        // We pass the current location in the state.
        // This allows us to redirect the user back to the page they
        // were trying to access after they log in.
        return <Navigate to="/login" state={{ from: location }} replace />;
      }
    
      // If the user is authenticated, we render the child component.
      return children;
    };
    
    export default ProtectedRoute;
    
    // 2. Use it in your router
    
    // AppRouter.tsx
    <Routes>
      <Route path="/login" element={<LoginPage />} />
    
      <Route
        path="/dashboard"
        element={
          <ProtectedRoute>
            <DashboardPage />
          </ProtectedRoute>
        }
      />
      <Route
        path="/settings"
        element={
          <ProtectedRoute>
            <SettingsPage />
          </ProtectedRoute>
        }
      />
    </Routes>
    
    ```
    
    This approach is clean, declarative, and maintainable. The router's job is just to define paths and components; the `<ProtectedRoute>` handles all the security logic.
    

### 🧠 Real-World Case Study: "The Master Keycard Scanner"

- **The Goal:** Secure multiple labs in a research facility.
- **Vulnerable Approach (Duplicated Logic):** Every lab door has its own separate, custom-built keycard reader. They all have slightly different wiring and software. To update the security protocol (e.g., to accept a new type of ID card), a technician has to go to every single door and reprogram it individually.
- **Secure Approach (Reusable Component):** The facility installs a single model of a master keycard scanner (`<ProtectedRoute>`). All doors use this exact same scanner. The scanners are all connected to a central security server. To update the security protocol, the admin just updates the central server's software _once_, and every scanner in the building is immediately updated with the new rules. It's efficient, consistent, and secure.

### 🤔 Reflective Questions

1. In our `<ProtectedRoute>`, we pass `state={{ from: location }}` to the `<Navigate>` component. How would the `LoginPage` component access this state to redirect the user back after a successful login?
2. React Router v6 introduced "Layout Routes" using the `<Outlet />` component. How could you refactor the router to make the syntax even cleaner, so you don't have to wrap every single route?
3. How could you modify the `<ProtectedRoute>` component to accept an `allowedRoles` prop (e.g., `<ProtectedRoute allowedRoles={['admin']}>`) to handle role-based authorization?

### 📚 Further Reading

- **React Router Docs:** [Example: Auth](https://www.google.com/search?q=https://reactrouter.com/en/main/start/auth) (This official example shows a similar pattern).
- **Blog Post:** [Private Routes in React Router 6](https://www.robinwieruch.de/react-router-private-routes/) by Robin Wieruch.
- **React Docs:** [Composition vs Inheritance](https://www.google.com/search?q=https://react.dev/learn/composition-vs-inheritance) (The principle behind why wrapper components like `<ProtectedRoute>` are powerful).

### 📝 Mini-Task (Production)

You are tasked with refactoring an existing router that uses the inline check pattern to use a new `<ProtectedRoute>` component.

1. Create the `<ProtectedRoute>` component as shown in the lesson. It should check for a `user` from your `useAuthStore` and redirect to `/login` if the user is not present.
    
2. Take the following router code:
    
    ```tsx
    const AppRouter = () => {
      const user = useAuthStore((state) => state.user);
    
      return (
        <Routes>
          <Route path="/" element={<HomePage />} />
          <Route path="/login" element={<LoginPage />} />
          <Route
            path="/app/dashboard"
            element={user ? <DashboardPage /> : <Navigate to="/login" />}
          />
          <Route
            path="/app/reports"
            element={user ? <ReportsPage /> : <Navigate to="/login" />}
          />
        </Routes>
      );
    };
    
    ```
    
3. Rewrite `AppRouter.tsx` to use your new `<ProtectedRoute>` component for the `/app/dashboard` and `/app/reports` routes. The resulting code should be cleaner and have no duplicated logic.