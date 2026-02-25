💡 CI01.1.2: Know Your Battlefield

**Outline:**

- **The Threat (SEEI):** Understanding the "attack surface" and the danger of unknown entry points.
- **The Defense (PPP):** Proactively identifying and mapping every source of external data.
- **Your Mission (Production):** Analyze a component and map its attack surface.

### 📘 Full Lesson Content

### Part 1: Presentation - The Threat (SEEI)

**What is it? (The Vulnerability):**

An application's **attack surface** is the sum of all points where an attacker can try to inject data or interact with the system. The vulnerability isn't just one specific flaw; it's the **ignorance of the full extent of your attack surface**. Many developers secure the obvious entry points—like login forms and comment fields—but forget about the less obvious ones. An attacker only needs to find one unguarded door.

**How it Works (The Attack Vector):**

Attackers actively look for overlooked data entry points. Consider a component that displays a different UI mode based on a URL hash. This is a common pattern for single-page applications.

```
// VULNERABLE CODE
import React, { useEffect, useState } from 'react';

const SpecialFeature = () => {
  const [message, setMessage] = useState('');

  useEffect(() => {
    const handleHashChange = () => {
      // The developer assumes the hash is just a simple string like '#promo'
      const hash = window.location.hash.substring(1); // Remove '#'
      if (hash) {
        // DANGER: The hash value is directly injected into the DOM.
        setMessage(`Special offer unlocked: ${hash}`);
      }
    };

    window.addEventListener('hashchange', handleHashChange);
    handleHashChange(); // Initial load

    return () => window.removeEventListener('hashchange', handleHashChange);
  }, []);

  // What if the URL is yoursite.com#<img src=x onerror=alert('XSS')> ?
  return <div dangerouslySetInnerHTML={{ __html: message }} />;
};

```

The developer forgot that the URL hash is user-controlled input. An attacker can craft a link that injects a malicious payload directly through the URL, completely bypassing any form validation.

### Part 2: Practice - The Defense (PPP)

**The Golden Rule:**

The principle of defense is **"Map Your Battlefield."** Before you write a single line of defense code, you must know everything you're defending. Systematically identify and document every single channel through which external data can enter your frontend application. Assume nothing is safe.

**Secure Code Implementation:**

The fix is to treat the URL hash with the same suspicion as a form input. Render it as text, never as HTML.

```
// SECURE CODE
import React, { useEffect, useState } from 'react';

const SpecialFeature = () => {
  const [mode, setMode] = useState('');

  useEffect(() => {
    const handleHashChange = () => {
      const hash = window.location.hash.substring(1);
      if (hash) {
        // The value is stored in state, but will be rendered safely.
        setMode(hash);
      }
    };

    window.addEventListener('hashchange', handleHashChange);
    handleHashChange();

    return () => window.removeEventListener('hashchange', handleHashChange);
  }, []);

  // SAFE: Even if the hash contains HTML, React's JSX will escape it.
  // The user will see the literal string, not the executed code.
  return (
    <div>
      {mode && <p>Special offer unlocked: {mode}</p>}
    </div>
  );
};

```

### 🧠 Real-World Case Study: "Vulnerable vs. Secured Code"

**The Goal:** A web analytics feature that gets campaign info from URL query parameters and displays it to an admin.

- **Vulnerable Approach:** A developer reads `utm_campaign` from the URL (`?utm_campaign=...`) and displays it in an admin dashboard using `innerHTML` to show a quick report.
    - **Result:** An attacker sends a phishing email to an admin with a link to the dashboard: `yoursite.com/admin?utm_campaign=<script>stealAdminToken()</script>`. When the admin clicks the link, the script executes in their browser, potentially giving the attacker full administrative access. The developer secured the login form but forgot about the URL parameters on the dashboard page.
- **Secure Approach:** The developer treats the query parameter as untrusted data. The report is generated using React components: `<span>Campaign: {campaignName}</span>`.
    - **Result:** When the admin clicks the malicious link, the dashboard correctly displays the text "Campaign: `<script>stealAdminToken()</script>`". The attack is visible but completely harmless.

### 🤔 Reflective Questions

1. Your application uses a third-party API (e.g., a weather API). Is the data received from this API part of your attack surface? Why or why not?
2. How does using a client-side routing library like `react-router` change your application's attack surface compared to a traditional multi-page application where every page navigation is a full request to the server?

### 📚 Further Reading

- **OWASP:** [Attack Surface Analysis Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Attack_Surface_Analysis_Cheat_Sheet.html) - A more formal guide to identifying potential weaknesses.
- **MDN:** [`window.postMessage`](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage) - A powerful but often overlooked part of the attack surface when dealing with iframes or popups.

### 📝 Mini Task (Production)

You are building a "User Profile" component in a React application. This component does the following:

1. Fetches user data from an API endpoint: `/api/users/{userId}`. The `userId` comes from the URL path.
2. The API returns a JSON object: `{ "username": "...", "profileImageUrl": "...", "bio": "..." }`.
3. Displays the `username`, `profileImageUrl` in an `<img>` tag, and the `bio`.

Your task: **List all the distinct entry points of external data** for this single component that make up its attack surface. For each point, state the potential risk.