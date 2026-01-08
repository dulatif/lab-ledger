# 21. Understanding Sagas

## 🎯 Learning Goal

Manage long-running distributed processes using **Sagas**.

## 🧠 Concept

A **Saga** is a piece of code that listens to events and dispatches commands. It acts as a "Process Manager".

Scenario: **User Signup Saga**

1.  Listen to `UserCreatedEvent`.
2.  Dispatch `CreateWalletCommand`.
3.  Listen to `WalletCreatedEvent`.
4.  Dispatch `SendWelcomeEmailCommand`.
5.  If `WalletCreationFailedEvent`? Dispatch `DeleteUserCommand` (Compensating Transaction).

## 💻 Implementation

In NestJS, Sagas are just Observables using RxJS.

```typescript
@Injectable()
export class UserSagas {
  @Saga()
  userCreated = (events$: Observable<any>): Observable<ICommand> => {
    return events$.pipe(
      ofType(UserCreatedEvent),
      map((event) => new CreateWalletCommand(event.userId))
    );
  };
}
```

## 🧩 Activity / Challenge

1.  Write a Saga that listens for `OrderPlacedEvent` and triggers `ProcessPaymentCommand`.
2.  Write a Saga that listens for `high_temp_alert` and triggers `ShutdownMachineCommand`.

## 🔑 Key Takeaways

- **Sagas** coordinate complex workflows across boundaries.
- They handle **Compensation** (Rollback) logic in distributed systems where ACID transactions aren't possible.
