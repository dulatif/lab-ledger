# Lesson 8: Vue Router

## Learning Goals

- Configuring Routes.
- `<RouterLink>` and `<RouterView>`.

## 1. Setup

`router/index.ts`:

```typescript
import { createRouter, createWebHistory } from "vue-router";
import HomeView from "../views/HomeView.vue";

const router = createRouter({
  history: createWebHistory(), // HTML5 Mode
  routes: [
    { path: "/", component: HomeView },
    { path: "/about", component: () => import("../views/AboutView.vue") }, // Lazy Loading
  ],
});

export default router;
```

## 2. Usage

`App.vue`:

```html
<header>
  <RouterLink to="/">Home</RouterLink>
  <RouterLink to="/about">About</RouterLink>
</header>

<RouterView />
<!-- Component renders here -->
```

## 3. Dynamic Routes

```typescript
{ path: '/users/:id', component: UserView }
```

Access in component:

```typescript
import { useRoute, useRouter } from "vue-router";
const route = useRoute();
console.log(route.params.id);
```

## Key Takeaways

- **Lazy Loading**: Always lazy load routes to keep the initial bundle small.
- **useRouter**: for Programmatic navigation (`router.push('/home')`).
- **useRoute**: for reading current route data (`route.params`).
