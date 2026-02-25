💡 AA01-5.1: Protecting Client-Side Routes

**Outline:**

- **The Threat (SEEI):** Understanding how an unprotected client-side route can flash protected content and leak application structure.
- **The Defense (PPP):** Learning the principle of checking authorization _before_ rendering a component.
- **Your Mission (Production):** Time to analyze a routing setup and identify its vulnerabilities.

### 📘 Full Lesson Content

### **Part 1: Presentation - The Threat (SEEI)**

- **What is it? (The Vulnerability):** **UI Flashing & Information Disclosure.** This is a subtle but significant flaw in single-page applications. If your routing logic allows a protected component (like a user dashboard) to render _before_ you've confirmed the user is authenticated, two things can happen:
    
    1. **UI Flashing:** For a fraction of a second, the user might see the skeleton of the protected page (titles, menus, layout) before being redirected. This is a jarring user experience.
    2. **Information Disclosure:** This flash tells an unauthenticated user that a route like `/dashboard` exists and gives them clues about the application's structure and features. For an attacker, this is valuable reconnaissance.
- **How it Works (The Attack Vector):** An unauthenticated user manually types `https://yourapp.com/dashboard` into their browser's address bar.
    
    ```tsx
    // BAD PATTERN: AUTH CHECK INSIDE THE COMPONENT
    
    // DashboardPage.tsx
    const DashboardPage = () => {
      const { user } = useAuthStore();
      const navigate = useNavigate();
    
      useEffect(() => {
        // PROBLEM: This check only runs AFTER the component has already started to render.
        if (!user) {
          navigate('/login');
        }
      }, [user, navigate]);
    
      // For a moment, this UI skeleton might be visible to an unauthenticated user.
      return (
        <div>
          <h1>User Dashboard</h1>
          <nav>...</nav>
          <p>Loading your data...</p>
        </div>
      );
    };
    
    // AppRouter.tsx
    <Routes>
      <Route path="/login" element={<LoginPage />} />
      {/* This route renders the component immediately, without any prior checks. */}
      <Route path="/dashboard" element={<DashboardPage />} />
    </Routes>
    
    ```
    
    The router immediately mounts `DashboardPage`. The component renders its initial UI, _then_ the `useEffect` hook runs, discovers the user is not logged in, and finally triggers the redirect. The damage is already done.
    

### **Part 2: Practice - The Defense (PPP)**

- **The Golden Rule:** **Gatekeep the route, not the component.** Authorization checks must happen at the routing layer, _before_ the protected component is ever allowed to mount and render. The component itself should be "dumb" and assume that if it's being rendered, it has permission to be there.
    
- **Secure Code Implementation (Conceptual):** We can implement the check directly in the router definition. This is a basic approach we will improve upon in the next lesson.
    
    ```tsx
    // BETTER PATTERN: INLINE CHECK AT THE ROUTING LAYER
    
    // AppRouter.tsx
    import { Routes, Route, Navigate } from 'react-router-dom';
    import { useAuthStore } from './store/authStore';
    
    const AppRouter = () => {
      // Get the user state from our single source of truth.
      const user = useAuthStore((state) => state.user);
    
      return (
        <Routes>
          <Route path="/login" element={<LoginPage />} />
          <Route
            path="/dashboard"
            element={
              // This logic runs BEFORE <DashboardPage> is chosen to be rendered.
              user ? <DashboardPage /> : <Navigate to="/login" replace />
            }
          />
        </Routes>
      );
    };
    
    ```
    
    In this pattern, when a user tries to access `/dashboard`, React Router evaluates the `element` prop. It checks the `user` variable. If the user is null, it renders the `<Navigate>` component, which immediately redirects without ever touching `DashboardPage`.
    

### 🧠 Real-World Case Study: "Club Bouncer vs. Hallway Guard"

- **The Goal:** Prevent unauthorized people from entering a VIP lounge.
- **Vulnerable Approach (Auth check in component):** The club has no bouncer at the main entrance to the VIP hallway. Anyone can walk down the hall and see the fancy door to the lounge, the velvet ropes, and hear the music from inside (`UI flash` & `info disclosure`). When they try to open the door, a guard _inside_ the lounge stops them and sends them back out (`useEffect` redirect).
- **Secure Approach (Auth check in router):** The club has a bouncer (`router logic`) at the very entrance to the VIP hallway. When you approach, the bouncer checks your ID (`auth state`). If you're not on the list, you are immediately turned away (`<Navigate>`). You never even get to see the lounge door, so you don't know what it looks like or what's behind it.

### 🤔 Reflective Questions

1. What does the `replace` prop on the `<Navigate>` component do? Why is it good practice to use it for authentication redirects?
2. The inline check pattern is better, but what happens if you have 20 protected routes? What are the code maintenance drawbacks of this approach?
3. How does server-side rendering (SSR) change the dynamics of protecting routes compared to a purely client-side rendered (CSR) React app?

### 📚 Further Reading

- **React Router Docs:** [Authentication](https://www.google.com/search?q=https://reactrouter.com/en/main/start/auth)
- **React Router Docs:** [`<Navigate>` component](https://reactrouter.com/en/main/components/navigate)
- **OWASP:** [Top 10 A01:2021 – Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)

### 📝 Mini-Task (Production)

You are reviewing the following React Router setup for a new application.

```tsx
<Routes>
  {/* Public Routes */}
  <Route path="/" element={<HomePage />} />
  <Route path="/pricing" element={<PricingPage />} />
  <Route path="/login" element={<LoginPage />} />

  {/* Other Routes */}
  <Route path="/settings" element={<SettingsPage />} />
  <Route path="/admin/users" element={<UserManagementPage />} />
  <Route path="/profile/:userId" element={<UserProfilePage />} />
</Routes>

```

Your task:

1. Identify which of the "Other Routes" should almost certainly be protected and require a user to be logged in.
2. Which route should likely require an additional level of authorization (e.g., an "admin" role)?
3. Rewrite the route definition for `/settings` using the inline check pattern (`user ? <Component /> : <Navigate />`) to protect it. Assume you have access to a `user` variable from your auth store.