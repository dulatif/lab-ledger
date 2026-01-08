# 12. Creating an Alarms Service

## 🎯 Learning Goal

Build a realistic IoT scenario.

## 🧠 Concept

We are building a Building Management System.
Sensors send data (Temperature, Motion).
We need an **Alarms Service** to determine if we should alert the police.

## 💻 Implementation

1.  `nest g app alarms`.
2.  Setup Transporter (NATS).
3.  Create an Event: `MessagePattern('sensor_reading')`.

## 🧩 Activity / Challenge

1.  Create the service.
2.  Your `orders` service is boring. Let's pivot to this IoT example for the rest of the chapter.
3.  Simulate a sensor sending generic data.

## 🔑 Key Takeaways

- Real world microservices often deal with high-volume data streams.
