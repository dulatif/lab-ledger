# Lesson 7: GraphQL over HTTP

## Learning Goals

- POST vs GET.
- Status Codes.

## 1. The Standard

GraphQL is usually served over HTTP POST.
Body:

```json
{
  "query": "...",
  "variables": { ... }
}
```

## 2. Status Codes

In REST, 404 means Not Found.
In GraphQL, if the _Server_ is reachable, the status is usually **200 OK**.
Errors are returned in the JSON body:

```json
{
  "data": null,
  "errors": [{ "message": "Not Found" }]
}
```

## Key Takeaways

- Don't rely on HTTP Status Codes for error handling in the client. Check `response.errors`.
