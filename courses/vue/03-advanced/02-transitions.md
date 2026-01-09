# Lesson 2: Transitions & Animations

## Learning Goals

- `<Transition>` component.
- CSS Classes.

## 1. The Transition Component

Wraps an element entering/leaving the DOM (`v-if`, `v-show`).

```html
<Transition name="fade">
  <p v-if="show">Hello</p>
</Transition>
```

## 2. CSS Classes

Vue automatically toggles these classes:

- `.fade-enter-from` -> `.fade-enter-active` -> `.fade-enter-to`
- `.fade-leave-from` -> `.fade-leave-active` -> `.fade-leave-to`

```css
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
```

## 3. List Transitions

Use `<TransitionGroup tag="ul">`.
You must provide a unique `:key` for each item.
Vue can animate the reshuffling of items using the `FLIP` technique (handled automatically via the `.v-move` class).

## Key Takeaways

- Transitions add "polish" to your app.
- Vue handles the DOM timing for you.
