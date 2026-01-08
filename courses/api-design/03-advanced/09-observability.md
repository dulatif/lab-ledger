# 9. Observability: Profiling and Metrics

## 🎯 Learning Goal

Knowing what's happening.

## 🧠 Concept

- **Logs**: "User X failed to login". (Events).
- **Metrics**: "99% of requests < 200ms". (Aggregates).
- **Tracing**: "Req-ID 123 spent 50ms in Gateway, 100ms in DB". (Distributed Context).

## 💻 Implementation

OpenTelemetry is the standard for tracing.
Prometheus/Grafana for metrics.

## 🧩 Activity / Challenge

1.  Without Distributed Tracing, debugging microservices is impossible.

## 🔑 Key Takeaways

- Logging is expensive. Metrics are cheap. Tracing is essential for distributed systems.
