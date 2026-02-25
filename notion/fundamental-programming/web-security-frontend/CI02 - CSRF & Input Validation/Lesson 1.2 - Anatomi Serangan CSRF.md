💡 CI02.1.2: The Silent Command: Anatomy of a CSRF Attack

**Outline:**

- **The Threat (SEEI):** A step-by-step breakdown of how a malicious website can force a user's browser to submit a forged form.
- **The Defense (PPP):** Understanding how a single, unguessable token in the form breaks the entire attack chain.
- **Your Mission (Production):** Identify what's missing from a vulnerable HTML form.

### 📘 Full Lesson Content

### Part 1: Presentation - The Threat (SEEI)

**What is it? (The Vulnerability):**

A CSRF attack is a sequence of events that exploits the trust a web application has in a user's browser. The attack's anatomy is a predictable, step-by-step process where the attacker acts as a puppeteer, and the victim's browser is the puppet.

**How it Works (The Attack Vector):**

Let's break down a classic CSRF attack that targets a `POST` request.

1. **Prerequisite:** The victim, Alice, logs into her trusted web application, `https://your-bank.com`. Her browser stores a session cookie for this domain.
2. **The Lure:** An attacker, Mallory, emails Alice a link to a "fun cat video" website, which is actually Mallory's malicious site, `evil-cats.com`.
3. **The Trap:** Alice clicks the link and visits `evil-cats.com`. Hidden on this page is an HTML form that perfectly mimics the "Transfer Funds" form from `your-bank.com`.
4. **The Trigger:** As soon as the page loads, a small piece of JavaScript on `evil-cats.com` automatically submits this hidden form.
5. **The Forgery:** Alice's browser, seeing a form submission targeted at `https://your-bank.com`, helpfully attaches her session cookie for that domain and sends the `POST` request.
6. **The Impact:** `your-bank.com` receives a request to transfer funds. The request contains a valid session cookie for Alice. Since the server has no way of knowing this request originated from `evil-cats.com`, it processes the transfer.

**Vulnerable Code on `evil-cats.com`:**

```html
<!-- This is the HTML on the attacker's website -->
<html>
  <head>
    <title>Cute Cat Videos!</title>
  </head>
  <body onload="document.forms[0].submit()">
    <h1>Loading your video...</h1>
    <!-- This form is invisible to the user -->
    <form action="<https://your-bank.com/api/transfer>" method="POST" style="display: none;">
      <input type="hidden" name="recipient" value="MALLORYS_ACCOUNT">
      <input type="hidden" name="amount" value="5000">
    </form>
  </body>
</html>

```

### Part 2: Practice - The Defense (PPP)

**The Golden Rule:**

The defense is to break the attacker's ability to perfectly forge the request. The attacker knows what parameters the form needs (`recipient`, `amount`), but we must add a parameter they **cannot know**: the CSRF token. This is called the **Synchronizer Token Pattern**.

**Secure Code Implementation:**

The secure version of the bank's form would require a secret token provided by the server.

```html
<!-- This is the LEGITIMATE form on <https://your-bank.com> -->
<form action="<https://your-bank.com/api/transfer>" method="POST">
  <!-- This token is unique to Alice's session and this form -->
  <input type="hidden" name="csrf_token" value="aBcDeFgHiJkLmNoPqRsTuVwXyZ123456">

  <label for="recipient">Recipient:</label>
  <input type="text" name="recipient">

  <label for="amount">Amount:</label>
  <input type="text" name="amount">

  <button type="submit">Transfer</button>
</form>

```

The attacker's form on `evil-cats.com` cannot guess the correct `csrf_token` value. When the forged request arrives at the server without the token (or with the wrong one), the server immediately rejects it as invalid. The attack is stopped.

### 🤔 Reflective Questions

1. Why can't the attacker's JavaScript on `evil-cats.com` simply use `fetch` to load the bank's form page, scrape the CSRF token, and then submit their forged form? (Hint: Think about browser security boundaries).
2. This example used a hidden form. What are some other ways an attacker could get a user's browser to send a request to another site?

### 📚 Further Reading

- **PortSwigger:** [What is CSRF (Cross-Site Request Forgery)?](https://portswigger.net/web-security/csrf) - An excellent, in-depth explanation with interactive labs.

### 📝 Mini Task (Production)

You are reviewing the code for a "Change Password" form on your site.

```html
<form action="/settings/change-password" method="POST">
  <input type="password" name="new_password">
  <input type="password" name="confirm_password">
  <button type="submit">Save New Password</button>
</form>

```

What single, critical `input` element is missing from this form to protect it from CSRF attacks? Write out the full HTML for the missing element (you can use `SECRET_TOKEN` as the value).