# Lesson 3: Slots (Content Distribution)

## Learning Goals

- Passing Template fragments.
- Named Slots.

## 1. Default Slot

Pass HTML content **into** a component.

```html
<!-- Button.vue -->
<button class="btn">
  <slot></slot>
  <!-- Outlet -->
</button>
```

**Usage**:

```html
<button>
  <strong>Click Me</strong>
</button>
```

## 2. Named Slots

For complex layouts (Layout, Modal).

```html
<!-- BaseLayout.vue -->
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
    <!-- Default -->
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```

**Usage**:

```html
<BaseLayout>
  <template #header>
    <!-- # is shorthand for v-slot: -->
    <h1>My Title</h1>
  </template>

  <p>Main content.</p>

  <template #footer>
    <p>Contact us</p>
  </template>
</BaseLayout>
```

## Key Takeaways

- Slots allow you to compose components efficiently.
- **Scoped Slots** allow the child to pass data back to the slot content (Advanced).
