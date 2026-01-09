# Lesson 6: Directives (Control Flow)

## Learning Goals

- Render content conditionally.
- Render lists.
- Style classes dynamically.

## 1. New Control Flow (Angular 17+)

Angular recently replaced `*ngIf` and `*ngFor` with a cleaner syntax.

### @if

```html
@if (isLoggedIn) {
<button (click)="logout()">Logout</button>
} @else {
<button (click)="login()">Login</button>
}
```

### @for

Replaces `*ngFor`.
**Must** provide a unique `track` key (like React's `key`).

```html
<ul>
  @for (user of users; track user.id) {
  <li>{{ user.name }}</li>
  } @empty {
  <li>No users found.</li>
  }
</ul>
```

## 2. Attribute Directives

Change the appearance or behavior of an element.

### [ngClass]

Conditionally add CSS classes.

```html
<div [ngClass]="{ 'active': isActive, 'error': hasError }">Status</div>
```

### [ngStyle]

Dynamic inline styles.

```html
<div [ngStyle]="{ 'color': isCritical ? 'red' : 'green' }">Level</div>
```

## Key Takeaways

- **@track** is mandatory for `@for`. It improves performance significantly.
- **Directives** can be recognized by the `[]` or `@` syntax usually.
