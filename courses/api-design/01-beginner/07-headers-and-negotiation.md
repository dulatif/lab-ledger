# 7. Headers and Content Negotiation

## 🎯 Learning Goal

Metadata for the request/response.

## 🧠 Concept

**Headers**: Key-Value pairs.

- `Content-Type`: What format is the body? (`application/json`).
- `Accept`: What format do I want?
- `Authorization`: Credentials.

**Content Negotiation**:
Client: `Accept: application/xml`
Server: Returns XML version.

## 💻 Implementation

```http
GET /users/1 HTTP/1.1
Accept: application/json
```

## 🧩 Activity / Challenge

1.  Imagine a multilingual API.
2.  Header: `Accept-Language: es-ES`.
3.  Server responds with Spanish content.

## 🔑 Key Takeaways

- Content negotiation allows one URL to serve multiple formats/languages.
