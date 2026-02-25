💡 CI01.4.3: Deployment: Implementing CSP in Your App

**Outline:**

- **The Threat (SEEI):** Designing a perfect CSP on paper is useless if it's not deployed correctly, or if it breaks the production site and gets rolled back.
- **The Defense (PPP):** Learning the two methods for deploying a CSP (HTTP Header and Meta Tag) and the "report-only" mode for safe rollout.
- **Your Mission (Production):** Choose the correct implementation method for different scenarios.

### 📘 Full Lesson Content

### Part 1: Presentation - The Threat (SEEI)

**What is it? (The Vulnerability):**

The vulnerability is a **flawed deployment strategy**. You can craft the world's most secure CSP, but if you implement it incorrectly, it's worthless. A more common risk is deploying a new, strict CSP directly to production, only to find it breaks a critical feature. The team then panics, rolls back the CSP entirely, and labels it "too difficult," leaving the site unprotected. A security feature that breaks the app is a failed feature.

**How it Works (The Attack Vector):**

A team decides to implement a strict CSP. They add it directly to their web server configuration and push to production.

```
# Nginx Server Config
add_header Content-Security-Policy "script-src 'self';";

```

Immediately, their customer support line lights up. A critical third-party payment processing script, which was loaded from `https://payments.vendor.com`, is now being blocked by the CSP. The site's checkout is completely broken. Under pressure, the lead developer comments out the CSP header to fix the emergency. They create a ticket to "revisit CSP later," but it never gets prioritized again.

### Part 2: Practice - The Defense (PPP)

**The Golden Rule:**

The principle is: **"Deploy with Confidence: Report First, Enforce Later."** You should never deploy a new CSP directly in enforcement mode. Instead, you use "Report-Only" mode. This allows you to test your policy on real production traffic without actually blocking anything. The browser will report violations to a specified endpoint, allowing you to discover what you need to add to your allowlist before you flip the switch to enforcement.

**Secure Code Implementation (Deployment Methods):**

There are two ways to deliver a CSP, and one safe way to roll it out.

1. **Method 1: HTTP Header (Recommended)**
    
    - This is the most secure and flexible method. You configure your web server (Nginx, Apache), cloud service (Netlify, Vercel), or application server (Express, Django) to add the `Content-Security-Policy` header to every response.
        
    - **Example (Express.js server):**
        
        ```ts
        app.use((req, res, next) => {
          res.setHeader(
            'Content-Security-Policy',
            "script-src 'self' <https://trusted.cdn.com>;"
          );
          next();
        });
        
        ```
        
2. **Method 2: HTML Meta Tag**
    
    - If you don't have access to server headers, you can add the CSP via a `<meta>` tag in your HTML's `<head>`.
        
    - **Example (index.html):**
        
        ```html
        <head>
          <meta http-equiv="Content-Security-Policy" content="script-src 'self' <https://trusted.cdn.com>;">
        </head>
        
        ```
        
    - **Limitations:** The meta tag method is less effective. It doesn't apply to all directives (like `frame-ancestors`) and can be bypassed in some scenarios. The HTTP header is always preferred.
        
3. **Safe Rollout: Report-Only Mode**
    
    - To test your policy, you use the `Content-Security-Policy-Report-Only` header. The browser will process the policy and send JSON reports of any violations to the URL you specify in a `report-uri` or `report-to` directive, but it won't block the resources.
    - **Example:**`Content-Security-Policy-Report-Only: default-src 'self'; report-uri /csp-violations;`
    - You can monitor these reports for a week, adjust your policy to allow legitimate resources, and then switch to the enforcing `Content-Security-Policy` header with confidence.

### 🤔 Reflective Questions

1. Why is the HTTP header method considered more secure and powerful than the HTML meta tag method for delivering a CSP?
2. What are the pros and cons of setting a CSP at the web server/CDN level versus within your application's code (e.g., in an Express middleware)?

### 📚 Further Reading

- **MDN:** [Content-Security-Policy-Report-Only](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy-Report-Only) - Learn how to safely test your policies.
- **Sentry/URIports:** [CSP Reporting Services](https://report-uri.com/) - Examples of services that can collect and analyze CSP violation reports for you.

### 📝 Mini Task (Production)

You are in charge of security for two different web applications.

1. **App A** is a modern Single-Page Application built with React and served by a Node.js/Express backend. You have full control over the server code.
2. **App B** is a simple, static HTML and CSS website hosted on a basic file hosting service where you cannot configure any HTTP headers.

Your task: For each application, state which method you would use to implement its CSP (**HTTP Header** or **Meta Tag**) and write the one line of code or HTML you would add to implement a simple policy of `script-src 'self';`.