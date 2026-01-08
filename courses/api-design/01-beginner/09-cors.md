# 9. Browser Security (CORS)

## 🎯 Learning Goal

Cross-Origin Resource Sharing.

## 🧠 Concept

By default, Browser blocks Request from Domain A to API on Domain B.
**CORS** is the API on Domain B saying "I allow Domain A to talk to me".

## 💻 Implementation

Server sends header:
`Access-Control-Allow-Origin: https://frontend.com`

If not present, Browser throws error "CORS policy".

## 🧩 Activity / Challenge

1.  This is a **Browser** restriction. Use `curl` or Postman, and CORS is ignored.
2.  It shields users, not the server.

## 🔑 Key Takeaways

- CORS creates the most frustration for beginners. It must be configured on the Server.
