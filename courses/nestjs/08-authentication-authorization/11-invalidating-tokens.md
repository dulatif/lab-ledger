# 11. Invalidating Tokens?

## 🎯 Learning Goal

Learn strategies for "Logging Out" a user when using stateless JWTs.

## 🧠 Concept

**The Hard Truth**: You cannot "delete" a JWT. Once it's issued, it's valid until it expires. That's the trade-off of statelessness. 🤷‍♂️

So how do we Log Out?

1.  **Client-side**: Delete the token from storage. (Weak security, token still valid if copied).
2.  **Short Expiration**: Rely on the 15min expiry.
3.  **Blocklist (Denylist)**: Store the "Logged Out" token IDs in Redis with a TTL. Check every request against Redis.
4.  **Stateful Refresh**: Store the Refresh Token in the DB. To logout, delete it from the DB. The Access Token will die in <15m, and they can't get a new one.

## 💻 Implementation (Stateful Refresh)

### 1. User Entity

Add `currentRefreshToken` column to User table (hashed!).

### 2. Login

Save `hash(refreshToken)` to DB.

### 3. Logout Endpoint

```typescript
@UseGuards(AuthGuard('jwt'))
@Post('logout')
async logout(@ActiveUser() user) {
  await this.usersService.removeRefreshToken(user.id);
}
```

### 4. Refresh Loop

When refreshing, check:
`if (storedToken !== inputToken) throw Forbidden;`

## 🧩 Activity / Challenge

1.  Implement the Logout endpoint.
2.  Simply setting the stored refresh token to `null` is enough.
3.  Test successful logout -> Try to use the refresh token again -> Should fail.

## 🔑 Key Takeaways

- True stateless logout is impossible.
- Hybrid approach (Stateless Access, Stateful Refresh) is the industry standard sweet spot.
- Keep Access Token lifespan **short**.
