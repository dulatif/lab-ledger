# 💡 CD02-2.2: Registering Your Service Worker

**Outline:**

- **The Modern Web App (Presentation):** The first step in giving your app superpowers: registration.
- **The Engine Room (Practice):** How to add the registration code to your React application.
- **Your First Registration (Production):** Time to get the service worker recognized by the browser.

## 📘 **Full Lesson Content**

### Part 1: Presentation - The First Step

Before a service worker can do anything, your application has to tell the browser about it. This process is called **registration**.

Here's what happens when you call the registration function:

1. **Check for Support:** Your code first checks if the user's browser even supports the Service Worker API.
2. **Download & Parse:** The browser downloads the service worker JavaScript file you specify.
3. **Execute & Install:** If the file is downloaded and parsed successfully (no syntax errors), the browser runs the script and fires the `install` event inside the service worker.
4. **Activation:** After successful installation, the service worker will then activate.

Registration is something you only need to do once in your application's code. The browser then handles the rest. If it finds that the service worker file on your server is even one byte different from the one it has registered, it will automatically start the update process in the background.

## Part 2: Practice - The Engine Room

In a modern React project using a tool like Vite, this process is made incredibly simple by plugins. We'll use `vite-plugin-pwa`, which provides a handy utility.

You'll add this code to your main entry file, typically `main.tsx` or `index.tsx`.

```
// In your main React file (e.g., main.tsx)
import { registerSW } from 'virtual:pwa-register';

// The { immediate: true } option tells the service worker to update
// and take control as soon as a new version is available,
// which is great for development.
registerSW({ immediate: true });

```

For context, here's what the vanilla JavaScript registration code looks like under the hood. The plugin generates something similar to this for you.

```markdown
if ('serviceWorker' in navigator) {

// We wait for the window to load to ensure that registering the SW

// doesn't compete for bandwidth with critical resources like images or CSS.

window.addEventListener('load', () => {

navigator.serviceWorker.register('/sw.js')

.then(registration => {

console.log('Service Worker registered with scope: ', registration.scope);

})

.catch(error => {

console.error('Service Worker registration failed: ', error);

});

});

}
```

## 🧠 Real-World Case Study: "The Scope of Power”

An important concept in registration is **scope**. By default, a service worker's scope is determined by where the script is located.

- If you register `/sw.js`, the service worker can control all pages under your domain (e.g., `yoursite.com/`, `yoursite.com/about`, `yoursite.com/products/123`). This is what you want 99% of the time.
- If you registered `/dashboard/sw.js`, it could only control pages within the `/dashboard/` path (e.g., `yoursite.com/dashboard/settings`), but not the homepage.

A mistake in scope can lead to parts of your application not having offline capabilities. The tools we use, like `vite-plugin-pwa`, handle this correctly by placing the service worker at the root, ensuring it controls your entire application.

## 🤔 **Reflection Questions**

1. The vanilla JS example uses `window.addEventListener('load', ...)`. Why is it a good practice to delay service worker registration until after the page has loaded?
2. What do you think happens if your `sw.js` file contains a syntax error? How does the browser handle that during registration?
3. What are the security implications if a service worker could be registered from a different domain (e.g., `yoursite.com` registering a service worker from `evil-site.com/sw.js`)?

## 📚 **Further Reading**

- **Vite PWA Plugin:**

RegisteringtheServiceWorker

([https://vite-pwa-org.netlify.app/guide/register-service-worker](https://vite-pwa-org.netlify.app/guide/register-service-worker))

- **MDN:**

\`ServiceWorkerContainer.register()\\`

([https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerContainer/register](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerContainer/register))

## 📝 **Mini-Task (Production)**

Your task is to register a (currently empty) service worker in a sample project.

1. In a fresh Vite + React + TypeScript project, install `vite-plugin-pwa`.
2. Create an empty file named `sw.ts` in your public directory.
3. Add the `registerSW()` code to your `main.tsx` file.
4. Run your development server.
5. Open Chrome DevTools, go to the **Application** tab, and click on **Service Workers**.
6. You should see your service worker listed, though it might show an error like "The script has an unsupported MIME type" or "Failed to fetch". This is OK! The important part is that the browser _tried_ to register it. You have successfully taken the first step.