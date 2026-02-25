### **💡 AA01-5.3: Mix and Match: Per-Page Rendering in Next.js**

**Outline:**

- **The Metric & The Bottleneck (Presentation):** The inefficiency of a one-size-fits-all rendering strategy for an entire application.
- **The Optimization Technique (Practice):** A hands-on guide to how Next.js enables per-page rendering decisions.
- **Your Performance Mission (Production):** Time to apply the correct data-fetching function to various pages.

### 📘 **Full Lesson Content**

### **Part 1: Presentation - The Metric & The Bottleneck**

- **The Goal:** Our goal is to leverage the key architectural advantage of frameworks like Next.js: the ability to choose the most appropriate rendering strategy **on a page-by-page basis**. This allows us to build highly optimized, hybrid applications that are not locked into a single rendering mode.
- **Identifying the Problem:** The bottleneck is applying a single strategy to an entire app. For example, if you build a full SSR application, your static "About Us" page is still needlessly hitting a server on every request. This increases **TTFB** for that page and adds unnecessary load on your infrastructure. Conversely, if you build a static (SSG) site, how do you handle a dynamic, server-rendered search page? A monolithic approach forces you to compromise performance on some part of your application.

### **Part 2: Practice - The Optimization Technique**

- **The Principle:** In Next.js (using the `pages` router), the rendering strategy for a page is determined by which data-fetching function you `export` from its file. This is a simple but incredibly powerful convention.
    
- **Code Implementation Examples:**
    
    1. **Static Site Generation (SSG):** Export `getStaticProps`.
        
        ```tsx
        // file: pages/about.js
        // This page will be pre-rendered into HTML at build time.
        export async function getStaticProps() {
          // You can fetch content from a CMS here
          const companyInfo = { name: 'Performance Inc.' };
          return { props: { companyInfo } };
        }
        
        function AboutPage({ companyInfo }) {
          return <h1>About {companyInfo.name}</h1>;
        }
        export default AboutPage;
        
        ```
        
    2. **Server-Side Rendering (SSR):** Export `getServerSideProps`.
        
        ```tsx
        // file: pages/feed.js
        // This page will be rendered on the server for every single request.
        export async function getServerSideProps(context) {
          const res = await fetch('<https://api.example.com/feed>');
          const feedItems = await res.json();
          return { props: { feedItems } };
        }
        
        function FeedPage({ feedItems }) { /* ... render items ... */ }
        export default FeedPage;
        
        ```
        
    3. **Client-Side Rendering (CSR):** Export _no_ data-fetching function.
        
        ```tsx
        // file: pages/dashboard.js
        // This page is a static shell. Data fetching happens on the client.
        import { useState, useEffect } from 'react';
        
        function DashboardPage() {
          const [user, setUser] = useState(null);
          useEffect(() => {
            fetch('/api/user/me').then(res => res.json()).then(setUser);
          }, []);
        
          if (!user) return <p>Loading...</p>;
          return <h1>Welcome, {user.name}</h1>;
        }
        export default DashboardPage;
        
        ```
        
    
    By simply choosing the right function to export, you control the rendering.
    

### 🧠 **Real-World Case Study: "The Hybrid E-Commerce Site"**

- **The Scenario:** A team is building a modern e-commerce site with Next.js. They want maximum performance for every part of the user journey.
- **The Architecture:**
    - `pages/index.js` (Homepage): Uses **SSG** (`getStaticProps`) to create a super-fast, SEO-friendly landing page.
    - `pages/products/[id].js` (Product Page): Uses **ISR** (`getStaticProps` with `revalidate`) to serve pages from a CDN but keep price/stock info fresh.
    - `pages/search.js` (Search Results): Uses **SSR** (`getServerSideProps`) because the content is unique for every query and must be generated dynamically.
    - `pages/account/settings.js` (User Settings): Uses **CSR** (no data-fetching export) because it's behind a login and displays private user data fetched on the client.
- **The Result:** The application is a perfect hybrid. Public-facing pages are incredibly fast and SEO-friendly. Dynamic pages are fresh and responsive. Private pages are secure and client-rendered. They have used the best tool for each specific job, all within one cohesive application.

### 🤔 **Reflective Questions**

1. What happens if you try to `export` both `getStaticProps` and `getServerSideProps` from the same page file?
2. How does Next.js know to create a static `.html` file versus a serverless function for a page?
3. In the CSR example, the page itself is still "pre-rendered" into a static shell by Next.js at build time. What does this mean for its initial load performance compared to a traditional Create React App?

### 📚 **Further Reading**

- **Next.js Docs:** [Data Fetching: Overview](https://nextjs.org/docs/basic-features/data-fetching/overview)
- **Vercel:** [Next.js: The Hybrid Framework](https://vercel.com/docs/frameworks/nextjs)
- **Performance:** [Rendering on the Web (web.dev)](https://web.dev/rendering-on-the-web/)

### 📝 **Mini Task (Production)**

- **Your Mission:** You are given three page files in a Next.js project. Read the description for each and add the correct data-fetching function export (`getStaticProps` or `getServerSideProps`) or state that none is needed.
    1. `pages/docs/[slug].js`: A documentation page. The content comes from local markdown files and only changes when the code is redeployed. It needs to be fast and SEO-friendly.
    2. `pages/status.js`: A page that shows the live status of an external API. The data must be fetched fresh on _every page load_.
    3. `pages/profile/edit.js`: A form for a logged-in user to edit their profile information.