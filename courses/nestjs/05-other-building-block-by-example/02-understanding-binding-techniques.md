# 2. Understanding Binding Techniques

## 🎯 Learning Goal

Learn the four different ways to apply (bind) guards, interceptors, pipes, and filters to your application.

## 🧠 Concept

You can apply these building blocks at different levels of granularity. The narrower the scope, the more specific the control.

1.  **Global Scoped**: Applies to _every_ request in the app.
2.  **Controller Scoped**: Applies to every endpoint in a specific Controller.
3.  **Method Scoped**: Applies to one specific endpoint (`@Get()`).
4.  **Param Scoped** (Pipes only): Applies to a specific parameter.

## 💻 Implementation

### 1. Global Binding

In `main.ts`:

```typescript
app.useGlobalPipes(new ValidationPipe());
```

_Pros_: Uniformity. _Cons_: Hard to dependency inject services into them (because `main.ts` is outside the Module context).

### 2. Controller Binding

Using decorators.

```typescript
@Controller("cats")
@UseGuards(RolesGuard) // 🛡️ Protects all routes in this controller
export class CatsController {}
```

### 3. Method Binding

```typescript
@Get()
@UseInterceptors(LoggingInterceptor) // 🕵️ Logs only this route
findAll() {}
```

### 4. Param Binding (Pipes only)

```typescript
@Get(':id')
findOne(@Param('id', ParseIntPipe) id: number) {} // 🚰 Validates only this param
```

## 🧩 Activity / Challenge

1.  Create a dummy Guard (just a class implementing `CanActivate` returning `true`).
2.  Apply it to just ONE method in your controller using `@UseGuards()`.
3.  Try applying it to the whole Controller.
4.  Try applying it Globally in `main.ts`.

## 🔑 Key Takeaways

- **Global**: `app.useGlobal...`
- **Class/Method**: `@Use...` decorators.
- **Dependency Injection in Globals**: To use DI in global guards/pipes, register them in `AppModule` using a special provider token `{ provide: APP_GUARD, useClass: ... }`.
