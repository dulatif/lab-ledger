# 8. Cloud PaaS Options

## 🎯 Learning Goal

Running containers without managing servers (Serverless Containers).

## 🧠 Concept

Running your own K8s cluster (EC2/VMs) is "Hard Mode".
**PaaS (Platform as a Service)** abstracts the orchestration.

### Examples

- **AWS Fargate (ECS)**: "Run this task". AWS provisions the compute. You pay per vCPU/Second.
- **Google Cloud Run**: "Run this container, listen on port 80". Autoscales to Zero. HTTP triggered.
- **Azure Container Apps**: Similar to Cloud Run.

## 💻 Deep Dive: The Shift

No longer managing "Nodes". Just managing "Services".
Log collection, monitoring, and scaling are handled by the Cloud Provider.

## 🧩 Activity / Challenge

1.  Docker Deployment usually ends up here for modern apps.
2.  Build Image -> Push to ECR/GCR -> Deploy to Fargate/Cloud Run.

## 🔑 Key Takeaways

- Start here. Move to K8s only if you need custom orchestration complex control.
