# 6. Deployment Strategies

## 🎯 Learning Goal

Updating apps without downtime.

## 🧠 Concept

**Recreate Strategy** (Downtime):

1.  Stop V1.
2.  Start V2.
3.  Pro: Simple. Con: Downtime between 1 and 2.

**Rolling Update** (Default in K8s/Swarm):

1.  Have 3 replicas of V1.
2.  Start 1 replica of V2. Wait for health.
3.  Kill 1 replica of V1.
4.  Repeat.
5.  Pro: Zero Downtime. Con: Mixed versions running simultaneously.

**Blue/Green**:

1.  Have Env Blue (V1) taking 100% traffic.
2.  Spin up Env Green (V2). Test it privately.
3.  Switch Load Balancer from Blue to Green instantly.
4.  Pro: Instant rollback. Con: Double resource usage.

## 🧩 Activity / Challenge

1.  Which strategy handles DB migrations best? (Hard question. Usually require backward compatible DB changes).

## 🔑 Key Takeaways

- Strategies require an Orchestrator (K8s) and Load Balancer.
- Docker Compose can simulate Recreate (Restart), but not complex Rolling updates easily.
