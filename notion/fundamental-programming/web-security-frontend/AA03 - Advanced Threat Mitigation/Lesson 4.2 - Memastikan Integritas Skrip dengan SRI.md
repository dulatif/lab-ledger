💡 AE01-4.2: Trust, But Verify: Ensuring Script Integrity with SRI

**Outline:**

- **The Threat (SEEI):** A script file you load from a third-party CDN can be maliciously modified without your knowledge, turning a trusted library into a weapon against your users.
- **The Defense (PPP):** Using Subresource Integrity (SRI) to tell the browser the expected cryptographic hash of a script. The browser will only execute the script if its content perfectly matches the hash.
- **Your Mission (Production):** Time to generate an SRI hash for a popular JavaScript library and construct the fully secured `<script>` tag.

### 📘 Full Lesson Content

### **Part 1: Presentation - The Threat (SEEI)**

- **What is it? (The "Vulnerability"):** **Resource Hijacking.** You decide to use a popular JavaScript library, like jQuery or Lodash, and you load it from a public Content Delivery Network (CDN) to improve performance. `<script src="<https://some-cdn.com/lodash.min.js>"></script>` The vulnerability is that you have no guarantee that the file served by `some-cdn.com` today is the same one that will be served tomorrow. If an attacker compromises the CDN, they can replace `lodash.min.js` with a malicious version. Your website, and every other site that links to that resource, will start serving malware to its users. You are vulnerable to any security failure at the CDN.
- **How it Works (The Attack Vector):**
    1. An attacker gains access to a popular CDN's servers.
    2. They find the file for `react-dom.min.js` version `18.2.0`.
    3. They append a malicious keylogger function to the end of the legitimate React code.
    4. The file size changes, but the filename and URL remain identical.
    5. The next time a user visits your React site, their browser requests the script from the CDN.
    6. The browser receives the compromised version and executes it. The keylogger is now active on your page, stealing user credentials. Your server and code are untouched, but your site is compromised.

### **Part 2: Practice - The Defense (PPP)**

- **The Golden Rule:** **Pin the version, then pin the content.** Don't just link to a script; link to a specific version of a script whose content you have verified and "locked in" with a hash.
    
- **Secure Code Implementation (HTML Attributes):** Subresource Integrity is implemented using the `integrity` attribute on `<script>` and `<link>` tags.
    
    1. **Get the file you want to use.** For example, `https://code.jquery.com/jquery-3.6.0.min.js`.
    2. **Generate a cryptographic hash of that file.** The standard is to use SHA-384. You can do this with command-line tools or online generators. `openssl dgst -sha384 -binary jquery-3.6.0.min.js | openssl base64 -A` This will produce a hash like: `sha384-vtXRMe3mGCbOeY7l30aIg8H9p3GdeSe4IFlP6G8JMa7o7lXvnz3GFKzPxzJdPfGK`
    3. **Add the hash to your script tag.** You must also add the `crossorigin="anonymous"` attribute.
    
    ```html
    <!-- VULNERABLE -->
    <script src="<https://code.jquery.com/jquery-3.6.0.min.js>"></script>
    
    <!-- SECURE -->
    <script
      src="<https://code.jquery.com/jquery-3.6.0.min.js>"
      integrity="sha384-vtXRMe3mGCbOeY7l30aIg8H9p3GdeSe4IFlP6G8JMa7o7lXvnz3GFKzPxzJdPfGK"
      crossorigin="anonymous"
    ></script>
    
    ```
    
    Now, when a browser sees this tag, it will:
    
    4. Download the `jquery-3.6.0.min.js` file.
    5. Calculate its own SHA-384 hash of the downloaded file.
    6. Compare its calculated hash to the one in the `integrity` attribute.
    7. If they match, execute the script. If they **do not** match, the browser will refuse to execute the script and will log an error to the console. The attack is stopped dead.

### 🧠 Real-World Case Study: "The Fortified Library"

- **The Goal:** Use a third-party library for a common task, like showing cookie consent popups, without risking a supply chain attack.
- **The System:** A website needs to be GDPR compliant and uses a popular open-source consent management script hosted on a CDN.
- **Vulnerable Approach (No SRI):**
    - The developer includes `<script src="<https://cdn.example.com/cookie-consent.js>"></script>`.
    - One day, the CDN is compromised, and `cookie-consent.js` is replaced with a script that mines cryptocurrency in the user's browser (cryptojacking).
    - The developer's site performance plummets, and users complain their fans are spinning up.
- **Secure Approach (With SRI):**
    - The developer finds the script they need and generates its hash.
    - They implement the script tag with the `integrity` attribute.
    - The CDN is compromised in the same way.
    - When users visit the site, their browsers download the modified script, see that the hash doesn't match the one in the `integrity` attribute, and simply block the script from running.
    - **Result:** The cookie consent popup doesn't appear, which is a minor functional bug, but the site is completely protected from the cryptojacking attack. This is a huge security win.

### 🤔 Reflective Questions

1. Why is the `crossorigin="anonymous"` attribute mandatory for SRI to work?
2. What is the operational challenge of using SRI when you want to always link to the `@latest` version of a script instead of a specific version?
3. SRI protects against a compromised CDN, but how would you protect yourself from a legitimate library that had a malicious contributor merge bad code into an official release?

### 📚 Further Reading

- **MDN Web Docs:** [Subresource Integrity](https://developer.mozilla.org/en-US/docs/Web/Security/Subresource_Integrity)
- **W3C Recommendation:** [The official SRI specification](https://www.w3.org/TR/SRI/)
- **SRI Hash Generator:** [An easy-to-use online tool for generating SRI hashes](https://www.srihash.org/)

### 📝 Mini-Task (Production)

Your mission is to secure a third-party resource.

1. We need to include the Bootstrap 5.1.3 CSS file on a page. The URL for the file is: `https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css`
2. Go to an online SRI hash generator (like `srihash.org`) or use a command-line tool.
3. Paste in the URL and generate the SHA-384 hash for this file.
4. Write the complete, final `<link>` tag for this stylesheet, including the `rel`, `href`, `integrity`, and `crossorigin` attributes, making it fully secure with SRI.