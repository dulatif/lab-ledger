# 13. Orchestration vs Choreography

## 🎯 Learning Goal

Decide who controls the business process.

## 🧠 Concept

Scenario: **Fire Alarm Triggered**.
We need to: (1) Call Fire Dept, (2) Open Doors, (3) Email Tenants.

**Orchestration (Conductor)**:

- A central `AlarmFlowService`.
- It calls `FireDeptService`, then `DoorService`, then `EmailService`.
- It handles errors. "If FireDept fails, retry".
- Pros: clear logic. Cons: Single point of failure/logic bloat.

**Choreography (Dancers)**:

- Sensor publishes `FireDetectedEvent`.
- `FireDeptService` listens and acts.
- `DoorService` listens and acts.
- Pros: Decoupled. Cons: Hard to visualize the whole flow ("Who opens the doors??").

## 💻 Implementation

We tend to use **Choreography** inside Microservices ecosystems for scalability.

## 🧩 Activity / Challenge

1.  Implement Choreography.
2.  The `AlarmsService` emits `AlarmTriggered`.
3.  Create a Listener in `NotificationsService`.

## 🔑 Key Takeaways

- Use **Orchestration** for complex business transactions (Sagas) where order matters.
- Use **Choreography** for broadcasting state changes.
