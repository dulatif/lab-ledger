# 3. Adding Unit Tests

## 🎯 Learning Goal

Write actual tests for your Controller logic by **Mocking** the dependencies.

## 🧠 Concept

**Unit Testing** means testing a unit **in isolation**.
If `CatsController` calls `CatsService.findAll()`, and `CatsService` calls the Database... checking the Controller shouldn't require a running Database!

We **Mock** the Service. We tell the test module: "When the Controller asks for `CatsService`, give it this fake object instead."

## 💻 Implementation

### 1. Create the Mock

A mock is just a simple object with the same methods.

```typescript
const mockCatsService = {
  findAll: jest.fn(() => ["Test Cat"]), // 🎭 Fake implementation
};
```

### 2. Provide the Mock

Use `useValue` to swap the real class for the mock.

```typescript
const module: TestingModule = await Test.createTestingModule({
  controllers: [CatsController],
  providers: [
    {
      provide: CatsService, // 🔑 Token
      useValue: mockCatsService, // 📦 Value
    },
  ],
}).compile();
```

### 3. Write the Test

```typescript
it("should return an array of cats", async () => {
  // 1. Act
  const result = await controller.findAll();

  // 2. Assert
  expect(result).toEqual(["Test Cat"]);
  // Verify the service was actually called
  expect(mockCatsService.findAll).toHaveBeenCalled();
});
```

## 🧩 Activity / Challenge

1.  In your test file from Lesson 2.
2.  Create a mock object for your service.
3.  Replace the real service provider with `{ provide: Service, useValue: mock }`.
4.  Write a test checking that the controller returns what the mock returns.

## 🔑 Key Takeaways

- **Isolation**: Unit tests should not touch the DB or external APIs.
- **Mocking**: Replacing heavy dependencies with lightweight fakes.
- `jest.fn()`: Creates a "Spy" function that tracks calls, arguments, and return values.
