# 15. Introduction to API Keys

## 🎯 Learning Goal

Understand Machine-to-Machine (M2M) authentication and when to use API Keys vs JWTs.

## 🧠 Concept

- **JWT / User Auth**: For humans. Short-lived. Represents a person.
- **API Key**: For software/integrations. Long-lived. Represents a "Project" or "System".

Example: Stripe gives you a Secret Key (`sk_live_...`). You don't "login" to Stripe every time your server makes a payment request. You just send the key.

## 💻 Implementation

API Keys are usually sent in a specific header:
`X-API-KEY: abc_12345`

### Security Considerations

- **High Entropy**: Keys must be long, random strings.
- **Hashed**: Ideally, store the hash of the key in DB, not the plain key (just like passwords!).
- **Revocable**: You must be able to delete/disable a key instantly.

## 🧩 Activity / Challenge

1.  Design a database schema for `ApiKeys`.
    - `key`: string (hashed)
    - `scope`: string[] (permissions)
    - `isActive`: boolean
2.  Think about how you would generate a key (e.g., `crypto.randomBytes`).

## 🔑 Key Takeaways

- **M2M**: Use API Keys when "Server A" talks to "Server B".
- **Simplicity**: Easier than OAuth flow for internal services.
- **Danger**: Since they are long-lived, keeping them secret is critical.
