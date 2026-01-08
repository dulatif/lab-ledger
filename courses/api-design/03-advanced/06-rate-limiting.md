# 6. Rate Limiting and Throttling Algorithms

## 🎯 Learning Goal

Protecting resources.

## 🧠 Concept

- **Fixed Window**: 100 reqs / hour. (Spike at minute 59 and 00).
- **Sliding Window**: Smoother.
- **Token Bucket**: Allows bursts but maintains average.

## 💻 Implementation

Headers:
`X-RateLimit-Limit: 1000`
`X-RateLimit-Remaining: 999`
`Retry-After: 60`

## 🧩 Activity / Challenge

1.  Implement a "Leaky Bucket" algorithm logic mentally.

## 🔑 Key Takeaways

- Protect your database. The API layer should reject excess load cheap and fast.
