# 18. Bonus: Google authentication

## 🎯 Learning Goal

Allow users to "Login with Google" using OAuth2.0.

## 🧠 Concept

Instead of managing passwords, we delegate Authentication to Google.

1.  User clicks "Login".
2.  Redirected to Google.
3.  User approves.
4.  Google redirects back to our server with an `accessToken`.
5.  We use that token to get the user's Profile (Email, Name).
6.  We create/find the user in our DB and issue our own JWT.

## 💻 Implementation

### 1. Google Console

Create a project at [console.cloud.google.com](https://console.cloud.google.com) and get `CLIENT_ID` and `CLIENT_SECRET`.

### 2. Passport Strategy

```bash
npm install passport-google-oauth20
```

```typescript
// google.strategy.ts
@Injectable()
export class GoogleStrategy extends PassportStrategy(Strategy, "google") {
  constructor() {
    super({
      clientID: process.env.GOOGLE_CLIENT_ID,
      clientSecret: process.env.GOOGLE_CLIENT_SECRET,
      callbackURL: "http://localhost:3000/auth/google/callback",
      scope: ["email", "profile"],
    });
  }

  async validate(accessToken, refreshToken, profile, done) {
    // Check if user exists in DB, or create them
    done(null, user);
  }
}
```

### 3. Setup Routes

```typescript
@Get('google')
@UseGuards(AuthGuard('google'))
googleAuth() {} // Initiates redirect

@Get('google/callback')
@UseGuards(AuthGuard('google'))
googleAuthRedirect(@Req() req) {
  return this.authService.login(req.user); // Issue app JWT
}
```

## 🧩 Activity / Challenge

1.  Register an app on Google Cloud Console.
2.  Implement the Strategy using your credentials.
3.  Note that `validate()` is where you integrate with _your_ database logic.

## 🔑 Key Takeaways

- **OAuth2**: The standard for restricted access delegation.
- **Hybrid**: You use Google for _AuthN_, but you still treat them as a User in your system for _AuthZ_.
