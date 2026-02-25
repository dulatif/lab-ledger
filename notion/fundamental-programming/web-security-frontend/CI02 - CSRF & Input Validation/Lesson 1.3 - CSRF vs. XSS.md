💡 CI02.1.3: Different Enemy, Different Weapon: CSRF vs. XSS

**Outline:**

- **The Threat (SEEI):** Confusing CSRF and XSS, leading to the application of the wrong defensive measures.
- **The Defense (PPP):** Clearly delineating the two attacks to understand why they require separate, specific defenses.
- **Your Mission (Production):** Analyze a scenario and determine if it's vulnerable to XSS, CSRF, or both.

### 📘 Full Lesson Content

### Part 1: Presentation - The Threat (SEEI)

**What is it? (The Vulnerability):**

While both CSRF and XSS are "cross-site" attacks, they are fundamentally different. Confusing them is a critical error because the defense for one does not protect against the other.

- **XSS (Cross-Site Scripting)** exploits the trust a **user has in a website**. The goal is to inject malicious code _into_ the trusted website so it runs in the victim's browser.
- **CSRF (Cross-Site Request Forgery)** exploits the trust a **website has in a user's browser**. The goal is to trick the user's browser into sending a forged, state-changing request _to_ the trusted website.

**How it Works (The Attack Vector):**

**Analogy: The Bank Heist**

- **XSS Attack:** The attacker defaces the bank's public notice board (the website) with a malicious flyer. The flyer contains instructions that trick people (users) who read it into revealing their PINs. The attack happens _at the bank_.
- **CSRF Attack:** The attacker gives a pre-filled-out withdrawal slip to a legitimate bank customer (the user). The customer's trusted courier (their browser) takes the slip to the bank. The bank teller sees the customer's valid signature (the cookie) and hands over the money. The attack was initiated _from outside the bank_.

**Comparison Table:**

|Feature|Cross-Site Scripting (XSS)|Cross-Site Request Forgery (CSRF)|
|---|---|---|
|**Primary Goal**|Execute malicious script in the victim's browser.|Force the victim's browser to send a forged request.|
|**Exploits...**|The user's trust in the website.|The website's trust in the user's browser (and its cookies).|
|**Vector**|Injecting script into a page's output.|Forging a request on a malicious site that targets a trusted site.|
|**Primary Defense**|Output Encoding, Content Security Policy (CSP), Sanitization.|Anti-CSRF Tokens, SameSite Cookies.|

### Part 2: Practice - The Defense (PPP)

**The Golden Rule:**

You must **defend against both XSS and CSRF independently**. They are separate threats requiring separate controls.

- Fixing XSS with output encoding does **nothing** to stop a forged form post from a malicious site.
- Fixing CSRF with tokens does **nothing** to stop a script from being injected into your site's database and rendered to other users.

A truly secure application needs a Content Security Policy and proper output encoding (XSS defense) **AND** synchronizer tokens and SameSite cookies (CSRF defense).

### 🧠 Real-World Case Study: A Social Media Comment Box

**The Goal:** A user can post a comment on an article.

- **XSS Vulnerability:** The application takes the comment text and renders it directly into the page using `dangerouslySetInnerHTML` without sanitization.
    - **Attack:** An attacker posts the comment `<script>stealSessionToken()</script>`.
    - **Result:** Anyone who views the article has the script execute in their browser, potentially stealing their session. This is **Stored XSS**.
- **CSRF Vulnerability:** The comment form is protected from XSS but has no CSRF token.
    - **Attack:** An attacker creates a page on `evil.com` with a hidden, auto-submitting form that posts a spam comment to the article.
    - **Result:** A logged-in user who visits `evil.com` will unknowingly post the spam comment. They performed an action they didn't intend to. This is **CSRF**.

### 🤔 Reflective Questions

1. A successful Stored XSS vulnerability is generally considered more severe than a CSRF vulnerability. Why? (Hint: What can an attacker do with arbitrary script execution vs. forcing a single action?)
2. If an attacker finds an XSS vulnerability, can they bypass CSRF protection? How?

### 📚 Further Reading

- **Acunetix:** [XSS vs CSRF: A Clear-Cut Comparison](https://www.google.com/search?q=https://www.acunetix.com/blog/articles/xss-vs-csrf-a-clear-cut-comparison/) - A good article breaking down the differences.

### 📝 Mini Task (Production)

Your application is vulnerable to an XSS attack in the user's profile bio field. It is also vulnerable to a CSRF attack on the "Add Friend" button. You only have time to fix one vulnerability before the end of the day.

Which one should you fix first, and why? Justify your answer in one or two sentences by describing the potential impact of each vulnerability.