# 6. Implementing e2e Test Logic

## 🎯 Learning Goal

Write an actual E2E test that hits an HTTP endpoint and verifies the response using Supertest.

## 🧠 Concept

**Supertest** wraps the underlying HTTP server. It allows us to say:
"Make a GET request to `/cats` and expect a 200 OK."

Nothing is mocked here (usually). The request goes through Middleware -> Guard -> Interceptor -> Pipe -> Controller -> Service -> Database -> and back!

## 💻 Implementation

### Writing the Request

Inside the `describe` block from the previous lesson:

```typescript
it("/ (GET)", () => {
  return request(app.getHttpServer()) // 1. Target the app
    .get("/") // 2. METHOD and ROUTE
    .expect(200) // 3. Assert Status Code
    .expect("Hello World!"); // 4. Assert Body
});
```

### Testing JSON

```typescript
it("/cats (GET)", () => {
  return request(app.getHttpServer())
    .get("/cats")
    .expect(200)
    .expect((res) => {
      // Custom assertions on the body
      expect(res.body).toBeInstanceOf(Array);
      expect(res.body.length).toBeGreaterThan(0);
    });
});
```

## 🧩 Activity / Challenge

1.  Run the defaults `npm run test:e2e`.
2.  Add a new test case for a route that _doesn't_ exist (e.g., `/dogs`).
3.  Expect a 404 status. `request(...).get('/dogs').expect(404)`.
4.  Run the test again.

## 🔑 Key Takeaways

- `request(server)`: Initiates Supertest.
- `.get()`, `.post()`, `.put()`: HTTP methods.
- `.send(body)`: Attaches a JSON body to the request.
- `.expect(code)`: Asserts the HTTP Status Code.
