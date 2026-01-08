# 6. Advanced Standards: RFC 7807 and HATEOAS

## 🎯 Learning Goal

Standardized formats.

## 🧠 Concept

**RFC 7807 (Problem Details)**:
Standard error format.

```json
{
  "type": "balance-insufficient",
  "title": "Your balance is too low",
  "status": 403
}
```

**HATEOAS** (Hypermedia as the Engine of Application State):
Links in response to guide the client.

```json
{
  "id": 1,
  "links": [
    { "rel": "self", "href": "/orders/1" },
    { "rel": "cancel", "href": "/orders/1/cancel" }
  ]
}
```

## 🧩 Activity / Challenge

1.  HATEOAS implementation is rare in industry because it's complex for clients to consume.
2.  RFC 7807 is growing in popularity.

## 🔑 Key Takeaways

- Use Problem Details (RFC 7807) for errors. Skip HATEOAS unless required.
