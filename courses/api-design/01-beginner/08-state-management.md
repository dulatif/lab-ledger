# 8. State Management (Cookies & Caching)

## 🎯 Learning Goal

HTTP is stateless. How do we remember things?

## 🧠 Concept

**Cookies**: Small text files stored by browser. Sent automatically with every request to that domain.
**Caching (`Cache-Control`)**: Telling the client "Don't ask me again for 1 hour".

## 💻 Implementation

`Set-Cookie: session_id=123; HttpOnly; Secure`

`Cache-Control: max-age=3600`

## 🧩 Activity / Challenge

1.  Why use `HttpOnly`? (Prevent XSS scripts from reading cookies).
2.  Why cache static assets? (Save bandwidth).

## 🔑 Key Takeaways

- Cookies are for Auth state. Caching is for Performance state.
