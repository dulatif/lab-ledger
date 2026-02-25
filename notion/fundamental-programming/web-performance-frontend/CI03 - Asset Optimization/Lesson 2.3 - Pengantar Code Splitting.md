💡 CI03-2.3: Don't Ship It All at Once: Introduction to Code Splitting

Outline:

- **The Metric & The Bottleneck (Presentation):** The problem of the single, monolithic bundle becoming too large for fast initial loads.
- **The Optimization Technique (Practice):** Understanding the strategy of splitting your code into smaller, on-demand "chunks."
- **Your Performance Mission (Production):** Identifying the ideal split points in a sample application architecture.

📘 **Full Lesson Content**

### **Part 1: Presentation - The Metric & The Bottleneck**

- **The Goal:** We've learned how bundlers create a single, optimized bundle. But what happens when your app grows and that "optimized" bundle is 5 MB? The goal today is to understand **code splitting**, the single most impactful strategy for reducing the initial JavaScript payload of large applications.
- **Identifying the Problem:** The bottleneck is the **monolithic bundle**. As an application gets more features, the main JavaScript bundle grows larger and larger. A user visiting your site for the first time has to download, parse, and execute the code for _every single feature_ in your app, even features they may never use. This dramatically slows down the initial page load and harms key metrics like Time to Interactive (TTI) and First Contentful Paint (FCP).

### **Part 2: Practice - The Optimization Technique**

- **The Principle:** Code splitting is the process of breaking up that large, monolithic bundle into smaller files, often called "chunks." The bundler creates a small "main" chunk that contains the core, essential code needed for the initial page load (e.g., React, your main shell, and router). Then, it creates separate chunks for different features or pages of your application. These feature-specific chunks are then **loaded on-demand**, or "lazily," only when the user navigates to that part of the app.
    
- **Conceptual Flow:**
    
    1. **User visits `yourapp.com`:**
        - Browser downloads `main.js` and `vendor.js`.
        - The app renders the initial page (e.g., the Homepage). This is very fast.
    2. **User clicks "Dashboard" link:**
        - The application sees the user is going to a new route.
        - It then makes a network request to download `dashboard.js`.
        - Once downloaded, the dashboard page is rendered.
    
    The code for the dashboard was never part of the initial payload, making the first load incredibly fast.
    

### 🧠 **Real-World Case Study: "The Bloated Admin Panel"**

- **The Scenario:** A large company has a single-page application for their internal admin tools. It includes user management, complex data reporting (with heavy charting libraries), content management, and application settings.
- **The Problem:** The initial JavaScript bundle was 3.5 MB (gzipped). It took over 10 seconds for the application to become interactive on a decent connection. Employees were complaining that the "app was always frozen."
- **The Fix:** The frontend team implemented route-based code splitting. They identified each major section (Users, Reports, Content, Settings) as a split point.
- **The Takeaway:** The initial bundle size dropped to under 400 KB. The login page now loaded in less than two seconds. When an admin clicked on the "Reports" tab, they would see a loading spinner for a brief moment while the `reports.js` chunk (containing the large charting libraries) was downloaded. The perceived performance of the entire application improved tenfold, even though the total amount of code was the same. They just changed _when_ it was loaded.

### 🤔 **Reflective Questions**

1. What is the primary trade-off you make when implementing code splitting? (Hint: faster initial load vs. a small delay on navigation).
2. Besides application routes, what are some other good candidates for code splitting? (e.g., a component that is only shown after a button click).
3. How does code splitting benefit users on slow mobile networks the most?

### 📚 **Further Reading**

- **React Docs:** [The official guide to Code-Splitting](https://react.dev/reference/react/lazy)
- **Vite Docs:** [Code Splitting is a default feature](https://www.google.com/search?q=https://vitejs.dev/guide/features.html%23code-splitting)
- **Web.dev:** [A guide to reducing JavaScript payloads with code splitting](https://web.dev/articles/reduce-javascript-payloads-with-code-splitting)

### 📝 **Mini Task (Production)**

- **Your Mission:** You are the architect for a new e-commerce application.
- **The Feature List:**
    - Homepage (shows featured products)
    - Product Search Results Page
    - Product Detail Page
    - Shopping Cart (a pop-up modal)
    - Checkout Flow (a multi-step page)
    - User Profile / Order History Page
    - Admin Dashboard (a separate, complex area for store owners)
- **Your Deliverable:** List at least four logical "chunks" you would create using code splitting to ensure the initial load for a new customer is as fast as possible.