💡 CI03-3.4: Trust, But Verify: Confirming Your Code Splitting

Outline:

- **The Metric & The Bottleneck (Presentation):** The danger of "invisible" optimizations; the need to prove that our changes had the intended effect.
- **The Optimization Technique (Practice):** Using the browser's Network tab to observe dynamically loaded chunks in real-time.
- **Your Performance Mission (Production):** Performing a full end-to-end test of a lazy-loaded route and documenting the network evidence.

📘 **Full Lesson Content**

### **Part 1: Presentation - The Metric & The Bottleneck**

- **The Goal:** We've implemented `React.lazy()` and `<Suspense>`. The code looks right, and the loading spinner shows up. But how do we _know_ for sure that code splitting is actually working? Our goal is to become performance detectives and use the browser's DevTools to find the concrete evidence.
- **Identifying the Problem:** The bottleneck is a **lack of verification**. Optimizations that you can't measure are just hopeful guesses. If we don't verify, we might have a misconfiguration, or a library might not be compatible with tree-shaking, or our lazy-loaded component might be so small that it has no real impact. We need to see the proof in the network requests to be confident in our work.

### **Part 2: Practice - The Optimization Technique**

- **The Principle:** The proof is in the **Network tab** of your browser's DevTools. When code splitting is working correctly, you will see a new network request for a JavaScript file at the exact moment you navigate to a lazy-loaded route.
- **The Verification Workflow:**
    1. **Run the production build:** These optimizations only apply to production builds. Run `npm run build` and serve the `dist` folder.
    2. **Open DevTools:** Open your app in the browser and open DevTools to the Network tab.
    3. **Filter by JS:** Click the "JS" filter to hide other requests like images and CSS.
    4. **Disable Cache:** Check the "Disable cache" box to ensure you're not loading files from memory.
    5. **Initial Load:** Reload the page. You should see the initial chunks being loaded (e.g., `index-....js`, `vendor-....js`).
    6. **Clear the Log:** Clear the Network log so it's empty.
    7. **Trigger the Load:** Now, click the link to your lazy-loaded route (e.g., the "/about" page).
    8. **Observe:** A new JavaScript file should appear in the Network log! This is the "chunk" containing the code for your lazy-loaded component. This is your proof.

### 🧠 **Real-World Case Study: "The Mystery Chunk"**

- **The Scenario:** A team implements route-based code splitting for their "Admin" section. They verify with the Network tab and see that `admin-....js` is correctly loaded on demand.
- **The Problem:** While analyzing the network log, they notice _another_ unexpected chunk, `charts-....js`, is also being loaded along with the admin chunk.
- **The Analysis:** They use a bundle analyzer (as learned in Module 2) to inspect the `admin` chunk. The visualization shows that the admin code has a static `import` to a charting library. Because the `charts` library is large, the bundler (Vite/Rollup) made an intelligent decision to split it out into its _own_ chunk to improve caching.
- **The Takeaway:** Verifying in the Network tab gives you a complete picture of what the user's browser is actually downloading. It not only confirms your own lazy loading but also reveals how the bundler is further optimizing your code, allowing you to understand the true impact of your changes.

### 🤔 **Reflective Questions**

1. Why is it crucial to run a production build (`npm run build`) before trying to verify code splitting?
2. What would you expect to see in the Network tab if your `React.lazy()` implementation was _not_ working correctly?
3. If you navigate back to a lazy-loaded route you've already visited, will a new network request be made? Why or why not (assuming the cache is enabled)?

### 📚 **Further Reading**

- **Chrome DevTools:** [Network Analysis Reference](https://developer.chrome.com/docs/devtools/network/)
- **React Docs:** [The full Code-Splitting guide](https://react.dev/reference/react/lazy)
- **Auditing Tool:** Your browser's Network tab is the only tool you need for this.

### 📝 **Mini Task (Production)**

- **Your Mission:** Be the detective and gather the evidence.
- **Instructions:**
    1. Take your completed code from the previous lesson's task (the app with lazy routes and a Suspense boundary).
    2. Create a production build of the application (`npm run build`).
    3. Serve the `dist` folder using a simple server like `npx serve dist`.
    4. Follow the 8-step "Verification Workflow" outlined in today's lesson. Navigate to your lazy-loaded `/dashboard` route.
    5. Look at the new file that appears in the Network log. It will have a name like `DashboardPage-....js` or a generic hash.
    6. Write one sentence confirming your success. (e.g., "After clicking the dashboard link, I saw a new JavaScript chunk named `assets/DashboardPage-aB1cD2eF.js` get loaded by the browser.").