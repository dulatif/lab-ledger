# Lesson 3: Modules vs Standalone

## Learning Goals

- The Mental Model of NgModules.
- The Modern "Standalone" Approach.

## 1. The Old Way: NgModules

Historically, every component had to be declared in an `NgModule`.

```typescript
// app.module.ts
@NgModule({
  declarations: [AppComponent, NavbarComponent],
  imports: [BrowserModule],
  bootstrap: [AppComponent],
})
export class AppModule {}
```

This added boilerplate and learning curve.

## 2. The New Way: Standalone Components

Since Angular 14+, components can be "Standalone". They manage their own dependencies.
**This is the default in modern Angular (v17+).**

```typescript
@Component({
  selector: "app-root",
  standalone: true, // Key Flag
  imports: [CommonModule, RouterOutlet], // Import what YOU need
  template: `...`,
})
export class AppComponent {}
```

## 3. Feature Organization

Even without NgModules, we group code by **Feature Directories**.

- `src/app/user-profile/`
- `src/app/cart/`

## Key Takeaways

- We will use **Standalone Components** in this course (Modern Best Practice).
- You only import what you use, making the app lighter (Tree Shaking).
