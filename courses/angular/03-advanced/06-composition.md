# Lesson 6: Advanced Composition

## Learning Goals

- Multi-slot Content Projection.
- Dynamic Components.

## 1. Content Projection Selectors

Project content into specific places.

```html
<!-- Wrapper Component -->
<div class="wrapper">
  <header>
    <ng-content select="[header]"></ng-content>
  </header>
  <main>
    <ng-content></ng-content>
  </main>
</div>
```

**Usage**:

```html
<app-wrapper>
  <h1 header>I am the header</h1>
  <p>I am the body</p>
</app-wrapper>
```

## 2. Host Binding

Change the properties of the component's _host element_ from within.

```typescript
@Component({
  selector: "app-card",
  template: `...`,
})
export class CardComponent {
  @HostBinding("class.active") isActive = false;
  // Adds class="active" to <app-card> element
}
```

## Key Takeaways

- Build generic layouts (PageLayout, Card, Modal) using projection.
- Use `HostBinding` to control the component's own tag.
