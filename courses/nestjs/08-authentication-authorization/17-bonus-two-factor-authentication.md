# 17. Bonus: Two-factor authentication

## 🎯 Learning Goal

Add an extra layer of security using Time-based One-Time Passwords (TOTP) compatible with Google Authenticator or Authy.

## 🧠 Concept

**2FA** (Something you have) complements **Password** (Something you know).
We use the **TOTP** algorithm.

1.  Server generates a secret (e.g., `JBSWY3DPEHPK3PXP`).
2.  Server sends secret to User (via QR Code).
3.  User's app hashes the current time + secret to generate a 6-digit code.
4.  User sends code to Server.
5.  Server validates codes.

## 💻 Implementation

### 1. Dependencies

```bash
npm install otplib qrcode
```

### 2. Generate Secret & QR Code

```typescript
import { authenticator } from 'otplib';
import * as qrcode from 'qrcode';

async generateTwoFactorSecret(user: User) {
  const secret = authenticator.generateSecret();
  const otpauthUrl = authenticator.keyuri(user.email, 'MyNestApp', secret);

  // Save 'secret' to DB for this user (encrypted!)

  return qrcode.toDataURL(otpauthUrl); // Returns base64 image
}
```

### 3. Verify Code

```typescript
isValid(code: string, userSecret: string) {
  return authenticator.verify({
    token: code,
    secret: userSecret,
  });
}
```

## 🧩 Activity / Challenge

1.  Implement `generateTwoFactorSecret` endpoint.
2.  Call it, take the output string, and paste it into a browser address bar (`data:image/png...`).
3.  Scan the QR code with your phone.
4.  Implement a `verify` endpoint that accepts the 6-digit code.

## 🔑 Key Takeaways

- **TOTP**: Standard-compliant 2FA.
- **Recovery Codes**: In a real app, always generate backup codes in case the user loses their phone!
