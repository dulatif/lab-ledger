💡 CI02.1.1: The Unwanted Action: What is Cross-Site Request Forgery?

**Outline:**

- **The Threat (SEEI):** Understanding how CSRF tricks a trusted user's browser into performing actions they never intended.
- **The Defense (PPP):** Introducing the core principle of verifying user intent with a secret token.
- **Your Mission (Production):** Identify a simple CSRF vulnerability in a GET-based action.

### 📘 Full Lesson Content

### Part 1: Presentation - The Threat (SEEI)

**What is it? (The Vulnerability):**

**Cross-Site Request Forgery (CSRF)** is an attack that forces a logged-in user to execute unwanted actions on a web application in which they're currently authenticated. The attacker's goal is to trick the victim's browser into sending a forged request to another website. The target website has no way of knowing the request was forged because it was sent by a legitimate user's browser, complete with their session cookies.

**How it Works (The Attack Vector):**

The attack exploits a fundamental aspect of how the web works: websites trust browsers, and browsers automatically send authentication cookies with requests. CSRF doesn't involve stealing data directly; it's about tricking the user into _performing a state-changing action_, like changing their email, transferring funds, or deleting an account.

**Analogy:** Imagine your personal assistant (your web browser) has a set of keys to your bank vault (your session cookies). An attacker gives your assistant a forged, pre-signed check telling the bank to transfer money to the attacker. Your assistant, not knowing the check is forged, goes to the bank and presents it. The bank teller sees your valid signature (the cookie) and honors the check. You, the user, may not even know this has happened.

### Part 2: Practice - The Defense (PPP)

**The Golden Rule:**

The fundamental defense against CSRF is to **verify user intent**. We can't just trust that a request is legitimate because it has the right cookie. We need proof that the user themselves initiated the action from our own website. This proof is a secret, unique, and unpredictable value that the attacker cannot guess: an **Anti-CSRF Token**.

**Secure Code Implementation (Conceptual):**

The pattern works like this:

1. When a user visits a page with a form (e.g., a "change password" form), the server generates a secret CSRF token and embeds it in the form.
2. When the user submits the form, this token is sent back to the server.
3. The server checks if the submitted token matches the one it expects for that user's session.

An attacker's forged form on `evil.com` cannot know the secret token, so when their form is submitted, the server will see the missing or incorrect token and reject the request.

### 🧠 Real-World Case Study: "The Dangers of GET Requests"

**The Goal:** An attacker wants to make a user unknowingly follow another user on a social media site.

- **Vulnerable Approach:** The application allows a user to be followed by making a simple GET request: `https://social-media.com/follow?user_id=123`. The check for whether the action is allowed is based only on the session cookie.
    - **Result:** An attacker places a seemingly innocent image on a forum they control: `<img src="<https://social-media.com/follow?user_id=ATTACKER_ID>" width="1" height="1">`. When a logged-in victim loads the forum page, their browser automatically makes the GET request to the `src` URL, sending their session cookie along with it. The server sees a valid request from a logged-in user and processes it. The victim has now followed the attacker without ever knowing it.
- **Secure Approach:** The application requires a POST request for this action and, more importantly, requires a valid CSRF token to be submitted with the request. The attacker's `<img>` tag attack is useless, and any forged form they create will be missing the required token.

### 🤔 Reflective Questions

1. CSRF attacks target "state-changing" requests (actions that change data). Why are read-only requests (like viewing your profile) generally not targets for CSRF?
2. We saw how a GET request can be exploited. How could an attacker force your browser to send a forged POST request? (Hint: think about forms).

### 📚 Further Reading

- **OWASP:** [Cross-Site Request Forgery (CSRF)](https://owasp.org/www-community/attacks/csrf) - A comprehensive overview of the attack.

### 📝 Mini Task (Production)

Your web application has a feature to let users delete their own posts by visiting a URL like `https://myapp.com/posts/delete?id=55`. The only security check is the user's session cookie.

Your task: Explain in one or two sentences how an attacker could trick a user into deleting their own post. What would the attacker need to get the user to do?