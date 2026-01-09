# Lesson 9: ViewChild & Content Projection

## Learning Goals

- Access child elements directly (`@ViewChild`).
- Create flexible containers (`<ng-content>`).

## 1. ViewChild

Sometimes you need to call a method on a child component directly, or access a raw DOM element.

```typescript
@Component({...})
export class ParentComponent {
  // Find the first UserCardComponent in the template
  @ViewChild(UserCardComponent) card!: UserCardComponent;

  ngAfterViewInit() {
    // Access it here
    console.log(this.card.name);
  }
}
```

## 2. Content Projection (ng-content)

Pass HTML _into_ a component (like `children` in React).

**Card Component**:

```html
<div class="card">
  <div class="header">
    <ng-content select="[header]"></ng-content>
  </div>
  <div class="body">
    <ng-content></ng-content>
    <!-- Default slot -->
  </div>
</div>
```

**Usage**:

```html
<app-card>
  <h3 header>My Title</h3>
  <p>This goes in the body.</p>
</app-card>
```

## Key Takeaways

- **ng-content** allows creating "Slot" based reusable UI components (Modals, Cards, Layouts).
- **ViewChild** breaks the simple Data-Down/Event-Up flow. Use it sparingly.
