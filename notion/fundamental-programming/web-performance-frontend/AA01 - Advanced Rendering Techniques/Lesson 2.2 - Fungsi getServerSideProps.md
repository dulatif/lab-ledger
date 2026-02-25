### **💡 AA01-2.2: getServerSideProps: The Brain of the SSR Page**

**Outline:**

- **The Metric & The Bottleneck (Presentation):** Fetching data before the server can render.
- **The Optimization Technique (Practice):** A hands-on guide to the `getServerSideProps` function.
- **Your Performance Mission (Production):** Time to create your first truly server-rendered component.

### 📘 **Full Lesson Content**

### **Part 1: Presentation - The Metric & The Bottleneck**

- **The Goal:** Our goal is to create data-driven pages that are still SEO-friendly and have a great **First Contentful Paint (FCP)**. To do this, we need to fetch data _on the server_ before we send any HTML to the client.
- **Identifying the Problem:** A page without its data is just a skeleton. In CSR, we ship the skeleton, then the client has to make API calls to get the data, causing a loading spinner and a delayed content paint. The bottleneck is the client-side data fetch. `getServerSideProps` is the specific Next.js function designed to eliminate this bottleneck by running server-side, just before render.

### **Part 2: Practice - The Optimization Technique**

- **The Principle:** `getServerSideProps` is an `async` function you export from a page file. Next.js will execute this function on every request to that page. Whatever object this function returns inside the `props` key will be passed as props to your page component. **This code never runs in the browser.**
    
- **Secure Code Implementation:** Let's create a page that gets its data from the server.
    
    ```tsx
    // file: pages/profile.tsx
    import type { GetServerSideProps, NextPage } from 'next';
    
    // 1. Define the shape of the props our component will receive
    interface ProfileProps {
      username: string;
      lastLogin: string;
    }
    
    // 2. The Page Component
    const ProfilePage: NextPage<ProfileProps> = ({ username, lastLogin }) => {
      return (
        <div>
          <h1>Welcome, {username}!</h1>
          <p>Last login: {lastLogin}</p>
        </div>
      );
    };
    
    // 3. The SSR Function
    export const getServerSideProps: GetServerSideProps = async (context) => {
      // On the server, you can do anything: access databases, use secret keys, etc.
      console.log('This runs on the server, not the browser!');
    
      const data = {
        username: 'UserX',
        lastLogin: new Date().toISOString(),
      };
    
      return {
        props: data, // This object is passed to ProfilePage as props
      };
    };
    
    export default ProfilePage;
    
    ```
    
    When you visit `/profile`, the `getServerSideProps` function runs, the `data` object is created, and your `ProfilePage` component is rendered on the server with `{ username: 'UserX', ... }` as its props. The browser receives the complete HTML.
    

### 🧠 **Real-World Case Study: "The Client-Side Spinner vs. The Server-Rendered Dashboard"**

- **The Scenario:** A user logs in and lands on their dashboard.
- **Before Optimization (CSR):** The app shell loads. A loading spinner is shown. A request is made to `/api/dashboard-data`. When it completes, the spinner is replaced with the user's widgets. Total time to see content: 1.5s.
- **After Optimization (SSR with `getServerSideProps`):** The user logs in. The server immediately runs `getServerSideProps`, which fetches the dashboard data. The server renders the complete dashboard with all its data into HTML. This HTML is sent to the browser. The user sees their content instantly. Total time to see content: 0.6s. The perceived performance is significantly better.

### 🤔 **Reflective Questions**

1. Why is it safe to use secret API keys or make direct database connections inside `getServerSideProps`?
2. What is the purpose of the `context` object that is passed as an argument to `getServerSideProps`? What kind of information can you find there?
3. What happens if the code inside `getServerSideProps` throws an error? How does Next.js handle it?

### 📚 **Further Reading**

- **Web Vitals:** [First Contentful Paint (FCP)](https://web.dev/fcp/)
- **Next.js Docs:** [`getServerSideProps`](https://nextjs.org/docs/basic-features/data-fetching/get-server-side-props)
- **Auditing Tool:** [Google Lighthouse](https://developer.chrome.com/docs/lighthouse/overview/)

### 📝 **Mini Task (Production)**

- **Your Mission:** You are tasked with creating a "Server Status" page.
    1. Create a new page at `pages/status.tsx`.
    2. Implement `getServerSideProps` in this file.
    3. Inside the function, create an object representing the server status, for example: `{ props: { status: 'OK', serverTime: new Date().toLocaleTimeString() } }`.
    4. Your page component should receive these props and display the status and server time.
    5. When you refresh the page, you should see the server time update with each request, proving that the page is being rendered dynamically on the server.