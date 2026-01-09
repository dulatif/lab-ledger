# Lesson 4: Components & Templates

## Learning Goals

- Generate a Component.
- Understand Selector, Template, Style.

## 1. Generating a Component

```bash
ng generate component components/hello-world
# Shortcut
ng g c components/hello-world
```

This creates 4 files: `.ts` (logic), `.html` (view), `.css` (style), `.spec.ts` (test).

## 2. Component Logic (.ts)

```typescript
import { Component } from "@angular/core";

@Component({
  selector: "app-hello-world", // HTML tag name
  standalone: true,
  imports: [],
  templateUrl: "./hello-world.component.html",
  styleUrl: "./hello-world.component.css",
})
export class HelloWorldComponent {
  // Properties (Data)
  title = "Hello Angular";

  // Methods (actions)
  sayHi() {
    alert(this.title);
  }
}
```

## 3. Using it

In `app.component.html`:

```html
<main>
  <h1>Welcome</h1>
  <app-hello-world></app-hello-world>
</main>
```

**Important**: You must import `HelloWorldComponent` in the `imports` array of `AppComponent`!

## Key Takeaways

- **Selector**: Defines how you use the component in HTML (`<app-hello-world>`).
- **Encapsulation**: Styles in `hello-world.component.css` _only_ apply to this component. They do not leak out.
