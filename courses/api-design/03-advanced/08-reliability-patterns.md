# 8. Reliability Patterns (Circuit Breaker, Retries)

## 🎯 Learning Goal

Failing gracefully.

## 🧠 Concept

**Retry**: Try again.
**Exponential Backoff**: Wait 1s, 2s, 4s, 8s. (Don't hammer a struggling server).
**Circuit Breaker**: If Payment Service fails 5 times, stop calling it for 1 min. Return error immediately.

## 💻 Implementation

Libraries like `Polly` (.NET) or `Resilience4j` (Java) or `cockatiel` (Node).

## 🧩 Activity / Challenge

1.  Why is "Retry Storm" dangerous? (Everyone retrying at once kills the recovering server).
2.  Use Jitter (randomness) in backoff.

## 🔑 Key Takeaways

- Protect your dependencies from your traffic.
