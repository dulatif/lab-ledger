# Lesson 2: Global State (NgRx)

## Learning Goals

- Understand the Redux pattern in Angular.
- Core Concepts: Store, Actions, Reducers, Selectors.

## 1. The Pattern

- **Store**: Creating a single source of truth.
- **Actions**: "Commands" dispatched by components (`[Auth] Login`).
- **Reducers**: Pure functions that handle actions and update state.
- **Selectors**: Slices of state used by components.

## 2. SignalStore (NgRx Signals)

The modern, lighter alternative to the classic Flux/Redux boilerplate.

```typescript
export const UserStore = signalStore(
  { providedIn: "root" },
  withState({ users: [], loading: false }),
  withMethods((store, usersService = inject(UsersService)) => ({
    async loadUsers() {
      patchState(store, { loading: true });
      const users = await usersService.getAll();
      patchState(store, { users, loading: false });
    },
  }))
);
```

**Usage**:

```typescript
@Component({...})
export class UserListComponent {
  readonly store = inject(UserStore);
  // {{ store.users() }}
}
```

## Key Takeaways

- **Classic NgRx**: Good for massive enterprise apps with complex side effects.
- **SignalStore**: Best for 90% of apps today. Functional and lightweight.
