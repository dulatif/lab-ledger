# 10. Implementing Refresh tokens

## 🎯 Learning Goal

Implement a robust session management system using short-lived Access Tokens and long-lived Refresh Tokens.

## 🧠 Concept

**The Problem**: If a JWT is valid for forever, and it gets stolen, the hacker has access forever.
**The Solution**:

1.  **Access Token**: Valid for 15 minutes. Used for API calls.
2.  **Refresh Token**: Valid for 7 days. Stored securely (HttpOnly Cookie preferably, or secure storage). Used ONLY to get a new Access Token.

## 💻 Implementation

### 1. Update Login Response

Return both tokens.

```typescript
// auth.service.ts
const validParams = { ... }
return {
  accessToken: this.jwtService.sign(validParams, { expiresIn: '15m' }),
  refreshToken: this.jwtService.sign(validParams, { expiresIn: '7d' }),
}
```

### 2. Refresh Strategy

We need a _separate_ Passport strategy to verify the refresh token. It should pass if the token is valid.

### 3. Refresh Endpoint

```typescript
@UseGuards(AuthGuard('jwt-refresh')) // Verify the refresh token signature
@Post('refresh')
refresh(@Req() req) {
  // Check if this refresh token is revoked (in DB/Redis)
  // If valid, issue NEW Access Token + NEW Refresh Token (Rotation)
}
```

## 🧩 Activity / Challenge

1.  Create a second strategy `JwtRefreshStrategy` (different secret key ideally!).
2.  Create a `/refresh` endpoint.
3.  Logic: Validate the incoming Refresh Token, issue a fresh pair of tokens.

## 🔑 Key Takeaways

- **Security**: Short access tokens minimize the window of opportunity for attackers.
- **UX**: Refresh tokens allow users to stay logged in for weeks without re-typing passwords.
- **Rotation**: Ideally, replace the Refresh Token every time it is used to detect theft.
