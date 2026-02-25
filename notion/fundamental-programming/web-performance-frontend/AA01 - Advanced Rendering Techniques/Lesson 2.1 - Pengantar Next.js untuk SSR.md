### **💡 AA01-2.1: Next.js: Your SSR Power Tool**

**Outline:**

- **The Metric & The Bottleneck (Presentation):** The complexity of manual SSR setups.
- **The Optimization Technique (Practice):** A hands-on intro to the Next.js `pages` directory.
- **Your Performance Mission (Production):** Time to build your first multi-page Next.js app.

### 📘 **Full Lesson Content**

### **Part 1: Presentation - The Metric & The Bottleneck**

- **The Goal:** Our goal today isn't a web vital, but a developer vital: **Time to First Meaningful App**. We want to build a server-rendered application without spending weeks configuring servers, bundlers, and routers.
- **Identifying the Problem:** Building SSR from scratch is a performance bottleneck for _teams_. You need to configure a Node.js server (like Express), set up webpack for both server and client bundles, handle routing, implement code-splitting, and manage data fetching logic. This complex boilerplate slows down development and introduces countless points of failure. Next.js abstracts all of this away, letting us focus on building features.

### **Part 2: Practice - The Optimization Technique**

- **The Principle:** Next.js simplifies SSR with two core concepts: a standardized project structure and **file-system based routing**. You don't configure a router; you create files.
    
- **Secure Code Implementation:** The magic happens inside the `pages` directory.
    
    - `pages/index.js` → maps to the `/` route.
    - `pages/about.js` → maps to the `/about` route.
    - `pages/products/index.js` → maps to the `/products` route.
    - `pages/products/[id].js` → maps to a dynamic route like `/products/123`.
    
    Let's create a simple two-page site.
    
    ```tsx
    // file: pages/index.js
    import Link from 'next/link';
    
    export default function HomePage() {
      return (
        <div>
          <h1>Welcome to the Home Page</h1>
          <Link href="/about">
            <a>Go to About Page</a>
          </Link>
        </div>
      );
    }
    
    ```
    
    ```tsx
    // file: pages/about.js
    import Link from 'next/link';
    
    export default function AboutPage() {
      return (
        <div>
          <h1>About Us</h1>
          <Link href="/">
            <a>Back to Home</a>
          </Link>
        </div>
      );
    }
    
    ```
    
    That's it. You now have a server-rendered, multi-page application with optimized, pre-fetched navigation via the `<Link>` component. No router setup needed.
    

### 🧠 **Real-World Case Study: "DIY SSR Setup vs. create-next-app"**

- **The Scenario:** A team needs to build a new SSR application.
- **Before Optimization (DIY SSR):** The team spends a week writing webpack configs, an Express server, and routing logic. They have to manually set up hot-reloading for development. Total files for boilerplate: 10+. Time spent before writing the first component: ~30 hours.
- **After Optimization (Next.js):** The team runs `npx create-next-app@latest`. Within 2 minutes, they have a fully functional, production-optimized SSR application running with a perfect Lighthouse score out of the box. Time spent before writing the first component: ~5 minutes.

### 🤔 **Reflective Questions**

1. What is the main advantage of file-system based routing over a library-based approach like `react-router-dom` in the context of SSR?
2. How does the Next.js `<Link>` component improve performance during navigation? (Hint: what does it do when it enters the viewport?).
3. Besides SSR, what other rendering strategies does Next.js support out of the box?

### 📚 **Further Reading**

- **Web Vitals:** [Understanding Core Web Vitals](https://web.dev/vitals/)
- **Next.js Docs:** [Routing: Introduction](https://nextjs.org/docs/routing/introduction)
- **Auditing Tool:** [Google Lighthouse](https://developer.chrome.com/docs/lighthouse/overview/)

### 📝 **Mini Task (Production)**

- **Your Mission:** Your task is to initialize a new Next.js project and create a simple three-page website.
    1. Run `npx create-next-app@latest --ts` to create a new TypeScript project.
    2. Create three pages: a homepage (`/`), a contact page (`/contact`), and a products page (`/products`).
    3. Add navigation links on every page so that a user can easily move between all three pages.
    4. Verify that navigating between pages feels instant and doesn't cause a full page reload.