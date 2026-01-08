# 19. Bonus: Sessions with Passport

## 🎯 Learning Goal

Understand the "Stateful" alternative to JWTs using server-side Sessions and Cookies.

## 🧠 Concept

Before JWTs, everyone used **Sessions**.

1.  Server creates a Session ID (`sid`).
2.  Stores `{ sid: 1, userId: 5 }` in RAM/Redis.
3.  Sends `sid` as a **Cookie** to browser.
4.  Browser sends Cookie on every request.

**Pros**: True logout (server deletes session).
**Cons**: Requires server memory or Redis. Harder to scale horizontally (sticky sessions or shared store).

## 💻 Implementation

### 1. Install

```bash
npm install express-session passport-session
```

### 2. Configure Middleware

In `main.ts`:

```typescript
import * as session from "express-session";
app.use(
  session({
    secret: "my-secret",
    resave: false,
    saveUninitialized: false,
    cookie: { maxAge: 3600000 }, // 1 hour
  })
);
app.use(passport.session());
```

### 3. Serialization

You must tell Passport how to pack/unpack the user from the session.

```typescript
@Injectable()
export class SessionSerializer extends PassportSerializer {
  serializeUser(user: any, done: Function) {
    done(null, user.id); // Save only ID to session
  }
  deserializeUser(id: any, done: Function) {
    // Find user by ID
    done(null, user);
  }
}
```

## 🧩 Activity / Challenge

1.  Discuss: Why would you choose Sessions over JWT?
    - _Ans_: Enterprise apps requiring strict security and instant revocation often prefer Sessions.
2.  Discuss: Why prefer JWT?
    - _Ans_: Microservices, Mobile Apps, high scalability requirements.

## 🔑 Key Takeaways

- NestJS supports both paradigms.
- Sessions = Stateful (Server implementation).
- JWT = Stateless (Client implementation).
