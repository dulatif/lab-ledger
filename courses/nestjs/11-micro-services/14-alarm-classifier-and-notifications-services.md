# 14. Alarm Classifier and Notifications Services

## 🎯 Learning Goal

Add intelligence to the mesh.

## 🧠 Concept

Raw sensor data isn't an alarm.
We need a **Classifier Service**.

1.  Ingest raw data.
2.  Run logic (IF temp > 50 AND smoke = true).
3.  Emit `AlarmConfirmed`.

Then **Notifications Service**:

1.  Listen to `AlarmConfirmed`.
2.  Send Email/SMS.

## 💻 Implementation

This is a standard Pipeline pattern.
`Sensor -> [NATS] -> Classifier -> [NATS] -> Notification`.

## 🧩 Activity / Challenge

1.  Create `classifier` app.
2.  Create `notifications` app.
3.  Wire them up using NATS subjects.

## 🔑 Key Takeaways

- **Pipes and Filters**: Microservices allow you to chain processing steps.
- Each service does ONE thing well.
