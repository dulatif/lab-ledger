# 14. DI sub-trees: A practical example

## 🎯 Learning Goal

Manually create a DI sub-tree to isolate dynamic dependencies without infecting the main app.

## 🧠 Concept

Scenario: We want to load a "Plugin" dynamically based on a user ID, but we want the Plugin to have its own isolated container logic.

We can use `ContextIdFactory`.

## 💻 Implementation

```typescript
@Get()
async handler(@Req() req) {
  // 1. Create a unique ID for this "Context"
  const contextId = ContextIdFactory.create();

  // 2. Register data (PAYLOAD) into this context
  this.moduleRef.registerRequestByContextId(req, contextId);

  // 3. Resolve a provider within this ISOLATED context
  const service = await this.moduleRef.resolve(MyService, contextId);

  return service.doSomething();
}
```

## 🧩 Activity / Challenge

1.  This is super advanced usage.
2.  Just understand that NestJS allows you to spin up mini-containers per request manually if the automatic behavior isn't what you want.

## 🔑 Key Takeaways

- **ContextId**: The key that identifies a specific sub-tree instantiation.
