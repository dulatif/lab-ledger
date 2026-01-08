# 2. Sync vs Async APIs (Webhooks vs Polling)

## 🎯 Learning Goal

Handling long-running tasks.

## 🧠 Concept

**Sync**: Request -> Wait -> Response. Timeouts if task takes > 30s.
**Async**: Request -> 202 Accepted.

- **Polling**: Client asks "Is it done?" every 5s. Wasteful.
- **Webhook**: Server calls Client "It's done!" (Reverse API).

## 💻 Implementation

Header for Async Pattern: `Location: /tasks/123/status`.

## 🧩 Activity / Challenge

1.  Video Transcoding API.
2.  Upload -> 202 Accepted.
3.  Webhook to `callback_url` when mp4 is ready.

## 🔑 Key Takeaways

- Use Async for anything > 500ms if possible, definitely for > 10s.
