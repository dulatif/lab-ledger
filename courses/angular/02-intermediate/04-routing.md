# Lesson 4: Routing Basics

## Learning Goals

- Configure Routes.
- Navigate via HTML and TS.

## 1. Route Configuration

In `app.routes.ts`:

```typescript
export const routes: Routes = [
  { path: "", component: HomeComponent },
  { path: "about", component: AboutComponent },
  { path: "products/:id", component: ProductDetailComponent },
  { path: "**", component: NotFoundComponent }, // Wildcard
];
```

## 2. Navigation

**HTML**:

```html
<nav>
  <a routerLink="/">Home</a>
  <a routerLink="/about" routerLinkActive="active-link">About</a>
</nav>
```

**TypeScript**:

```typescript
constructor(private router: Router) {}

goHome() {
  this.router.navigate(['/']);
}
```

## 3. Reading Parameters

In `ProductDetailComponent`:

```typescript
// With Component Input Binding (Angular 16+)
// In app.config.ts: withComponentInputBinding()
@Input() id!: string;

ngOnInit() {
    // "id" is automatically populated from the URL :id
    this.fetchProduct(this.id);
}
```

## Key Takeaways

- **RouterOutlet**: The placeholder where the routed component is inserted.
- **Wildcard (`**`)\*\*: Order matters. Put this last.
- **Input Binding**: The modern way to get path params without subscribing to `ActivatedRoute`.
