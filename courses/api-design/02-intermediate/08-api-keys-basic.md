# 8. API Keys and Basic Auth

## 🎯 Learning Goal

Simple authentication schemes.

## 🧠 Concept

**Basic Auth**:
Header `Authorization: Basic base64(user:pass)`.
Simple but requires sending credentials every time. TLS (HTTPS) is mandatory.

**API Keys**:
`X-API-Key: abcdef...`.
Identifies the _Project/Machine_, usually not the _User_.

## 💻 Implementation

API Keys often have no expiration and full access. Dangerous if leaked.

## 🧩 Activity / Challenge

1.  Generate a UUID. That allows it to be an API Key.
2.  Store hash in DB, check match on request.

## 🔑 Key Takeaways

- Use API Keys for Machine-to-Machine. Use Tokens for Users.
