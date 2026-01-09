# Lesson 5: Custom Directives

## Learning Goals

- Create an Attribute Directive.
- Manipulate the DOM safely.

## 1. Creating the Directive

```bash
ng g d directives/highlight
```

```typescript
@Directive({
  selector: "[appHighlight]", // Usage: <p appHighlight>
  standalone: true,
})
export class HighlightDirective {
  constructor(private el: ElementRef) {}

  @HostListener("mouseenter") onMouseEnter() {
    this.highlight("yellow");
  }

  @HostListener("mouseleave") onMouseLeave() {
    this.highlight("");
  }

  private highlight(color: string) {
    this.el.nativeElement.style.backgroundColor = color;
  }
}
```

## 2. Why?

Reuse behavior across components.
Examples: Tooltips, Copy-to-Clipboard, Permissions checks (`*appHasRole="'admin'"`).

## Key Takeaways

- **ElementRef**: Gives direct access to the DOM node. Use carefully.
- **HostListener**: Listens to events on the host element.
