# 5. HTTP Methods Deep Dive

## 🎯 Learning Goal

The verbs of the web.

## 🧠 Concept

- **GET**: Retrieve data. Safe + Idempotent.
- **POST**: Create new data. Not Safe.
- **PUT**: Replace data entirely. Idempotent.
- **PATCH**: Partial update. Not necessarily idempotent.
- **DELETE**: Remove data. Idempotent.

## 💻 Implementation

```bash
# GET
curl https://api.example.com/users/1

# POST
curl -X POST -d '{"name":"Alice"}' https://api.example.com/users
```

## 🧩 Activity / Challenge

1.  Why shouldn't you use GET to delete a user? (Links can be clicked by crawlers!).
2.  Why PUT for full replacement vs PATCH for partial?

## 🔑 Key Takeaways

- Semantics matter. Using GET to delete something is a security risk.
