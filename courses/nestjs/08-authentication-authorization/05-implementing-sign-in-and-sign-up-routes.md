# 5. Implementing Sign-in and Sign-up Routes

## 🎯 Learning Goal

Create the `AuthService` logic to validate credentials and the `AuthController` to expose login endpoints.

## 🧠 Concept

The login flow:

1.  User sends `email` and `password`.
2.  `AuthService` asks `UsersService` for the user with that email.
3.  `AuthService` uses `HashingService` to compare the input password with the stored hash.
4.  If match -> Return Success (Token).
5.  If mismatch -> Return `UnauthorizedException`.

## 💻 Implementation

### 1. AuthService

```typescript
@Injectable()
export class AuthService {
  constructor(
    private usersService: UsersService,
    private hashingService: HashingService,
    private jwtService: JwtService // We'll configure this next lesson
  ) {}

  async validateUser(email: string, pass: string): Promise<any> {
    const user = await this.usersService.findOne(email);
    if (user && (await this.hashingService.compare(pass, user.password))) {
      const { password, ...result } = user; // ✂️ Strip password
      return result;
    }
    return null;
  }

  async login(user: any) {
    const payload = { email: user.email, sub: user.id };
    return {
      access_token: this.jwtService.sign(payload),
    };
  }
}
```

### 2. AuthController

```typescript
@Controller("auth")
export class AuthController {
  constructor(private authService: AuthService) {}

  @Post("login")
  async login(@Body() signInDto: Record<string, any>) {
    const user = await this.authService.validateUser(
      signInDto.email,
      signInDto.password
    );
    if (!user) {
      throw new UnauthorizedException();
    }
    return this.authService.login(user); // returns token
  }
}
```

## 🧩 Activity / Challenge

1.  Generate `AuthModule`, `AuthService`, `AuthController`.
2.  Import `UsersModule` into `AuthModule`.
3.  Implement the validation logic.
4.  Try to login with wrong password (expect 401) and correct password (expect success).

## 🔑 Key Takeaways

- **ValidateUser**: Purely checks credentials. Returns user object or null.
- **Login**: Generates the session artifact (JWT).
- **Strip Passwords**: Never return the password field in the response object!
