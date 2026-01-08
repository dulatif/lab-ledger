# 11. Security Best Practices

## 🎯 Learning Goal

Hardening the API.

## 🧠 Concept

1.  **Rate Limiting**: Prevent DDoS. (429 Too Many Requests).
2.  **HTTPS**: Mandatory.
3.  **Input Validation**: Sanitize everything to prevent SQL Injection/XSS.
4.  **CSP (Content Security Policy)**: Browser headers.

## 💻 Implementation

Use a Gateway or Middleware (e.g., Helmet.js, Nginx) to handle headers and rate limiting.

## 🧩 Activity / Challenge

1.  How could an attacker abuse a login endpoint without rate limiting? (Brute force passwords).

## 🔑 Key Takeaways

- Security Layers: WAF -> Gateway -> App Validation -> DB Constraints.
