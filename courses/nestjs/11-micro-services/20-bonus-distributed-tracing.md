# 20. Bonus: Distributed tracing

## 🎯 Learning Goal

Debug a request that hops through 5 services.

## 🧠 Concept

If User 500s, where did it fail? Gateway? Orders? Auth?
Logs are scattered across 5 containers.

**Tracing**:

1.  Gateway generates a `TraceID` (uuid).
2.  Passes it to Orders (via NATS headers).
3.  Orders passes it to Payments.
4.  All services send "Spans" (Timing info) to a Collector (Jaeger/Zipkin).

## 💻 Implementation

Use **OpenTelemetry**.
It auto-instruments NestJS, HTTP, Redis, PG, etc.

```bash
docker run -d --name jaeger -p 16686:16686 jaegertracing/all-in-one:latest
```

## 🧩 Activity / Challenge

1.  This requires setting up the `otel-sdk`.
2.  The goal is to see a Waterfall chart in Jaeger UI showing exactly how long `createOrder` took in each microservice.

## 🔑 Key Takeaways

- **Observability** (Logs, Metrics, Traces) is non-negotiable for Microservices.
