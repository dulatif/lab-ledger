# Lesson 3: Performance Optimization

## Learning Goals

- OnPush Change Detection.
- Optimizing lists.

## 1. OnPush Strategy

By default, Angular checks _everything_ when _anything_ happens (Zone.js).
`OnPush` tells Angular: "Only check me if my **@Input reference changes** or an event originated from me."

```typescript
@Component({
  selector: 'app-list',
  template: `...`,
  changeDetection: ChangeDetectionStrategy.OnPush
})
```

**Impact**: Massive performance boost for presentation components.

## 2. Optimizing Loops

Use `@for` tracking.
If you don't track, Angular destroys and DOM-recreates the whole list on every update.

```html
<!-- BAD -->
@for (item of items)

<!-- GOOD -->
@for (item of items; track item.id)
```

## 3. Deferrable Views

Lazy load chunks of the template.

```html
@defer (on viewport) {
<app-heavy-chart />
} @placeholder {
<p>Loading Chart...</p>
}
```

## Key Takeaways

- Always use `OnPush` if possible.
- Always use `track` in loops.
- Use `@defer` to speed up initial load time (LCP).
