# Vue.js Intermediate Course Syllabus

## Module 5: Component Architecture

**Goal**: Build scalable and reusable components.

1.  **Composition API vs Options API**
    - Concepts: Comparing the two styles. Focus on `<script setup>`.
    - Goal: Understand modern Vue architecture.
2.  **Props & Events**
    - Concepts: `defineProps`, `defineEmits`.
    - Goal: Pass data down and bubble events up.
3.  **Slots (Content Distribution)**
    - Concepts: Default Slots, Named Slots, Scoped Slots.
    - Goal: Create flexible container components (e.g., Layouts, Modals).
4.  **Provide / Inject**
    - Concepts: Dependency Injection (Prop Drilling solution).
    - Goal: Share data across deep component trees.

## Module 6: Advanced Logic

**Goal**: Reusable logic and side-effects.

5.  **Watchers**
    - Concepts: `watch` vs `watchEffect`.
    - Goal: React to state changes (Side Effects).
6.  **Composables**
    - Concepts: Extracting logic into "Hooks" (VueUse style).
    - Goal: Reuse stateful logic across components.
7.  **Custom Directives**
    - Concepts: `v-focus`, Lifecycle hooks in directives.
    - Goal: Low-level DOM access.

## Module 7: Routing & State

**Goal**: Single Page Application (SPA) features.

8.  **Vue Router**
    - Concepts: Configuration, `RouterView`, `RouterLink`, Dynamic Routes.
    - Goal: Navigate between pages without refresh.
9.  **State Management (Pinia)**
    - Concepts: Stores, State, Getters, Actions.
    - Goal: Manage global application state (replacing Vuex).

## Module 8: Styling & UI

**Goal**: Make it look good.

10. **CSS in Vue**
    - Concepts: Scoped CSS, CSS Modules, `v-bind()` in CSS.
    - Goal: Component-scoped styling.
11. **External Libraries**
    - Concepts: Integrating Tailwind CSS or a UI Kit (PrimeVue/Vuetify).
    - Goal: Build professional UIs quickly.
