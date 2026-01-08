# 6. Status Codes and What They Mean

## 🎯 Learning Goal

The server's way of saying "Yes", "No", or "Maybe".

## 🧠 Concept

- **2xx (Success)**: 200 OK, 201 Created, 204 No Content.
- **3xx (Redirection)**: 301 Moved Permanently, 304 Not Modified.
- **4xx (Client Error)**: 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found.
- **5xx (Server Error)**: 500 Internal Server Error, 502 Bad Gateway.

## 💻 Implementation

Never return `200 OK` with an error message in the body. That breaks monitoring tools.

## 🧩 Activity / Challenge

1.  What code would you return if:
    - User tries to access admin route? -> 403.
    - User sends invalid JSON? -> 400.
    - Database is down? -> 500.

## 🔑 Key Takeaways

- Status codes are the first thing a client checks.
