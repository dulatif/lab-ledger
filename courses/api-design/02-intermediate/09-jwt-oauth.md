# 9. Modern Auth: JWT and OAuth 2.0

## 🎯 Learning Goal

Stateless and Delegated Auth.

## 🧠 Concept

**JWT (JSON Web Token)**:
Signed payload. The server doesn't need to check DB to know who you are. The signature proves it.
`header.payload.signature`.

**OAuth 2.0**:
Delegation protocol. "Let Google Log you into Spotify".
Spotify doesn't see your Google password. It gets a Token.

## 💻 Implementation

Access Token (Short lived, JWT) + Refresh Token (Long lived, Opaque).

## 🧩 Activity / Challenge

1.  Go to jwt.io and decode a token.
2.  See that data is readable (Base64) but not modifiable (Signature).

## 🔑 Key Takeaways

- Don't put secrets in JWT payload!
