# Lesson 1: Services & Dependency Injection

## Learning Goals

- Create a Service.
- Understand "Singleton".
- Inject a service into a component.

## 1. What is a Service?

A class designed to share data or logic.
It is separated from the View (Component).

```bash
ng g service services/auth
```

```typescript
@Injectable({
  providedIn: "root", // Available everywhere
})
export class AuthService {
  private user = "Guest";

  login(name: string) {
    this.user = name;
  }
}
```

## 2. Dependency Injection (DI)

Angular's DI system automatically creates instances of your classes.
You don't do `new AuthService()`. You ask for it in the constructor.

```typescript
@Component({...})
export class LoginComponent {
  // Angular injects the singleton instance here
  constructor(private authService: AuthService) {}

  // Modern Alternative (v14+)
  // private authService = inject(AuthService);
}
```

## 3. Singleton Nature

Because we use `providedIn: 'root'`, there is only **one** instance of `AuthService` in the entire app. If `LoginComponent` sets the user, `ProfileComponent` sees the same user.

## Key Takeaways

- Put logic in **Services**, not Components.
- Components should only focus on Presentation.
