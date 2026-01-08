# 8. Adding Health Checks

## 🎯 Learning Goal

Allow orchestrators (Kubernetes) to know when to kill/restart your pods.

## 🧠 Concept

Liveness Probe: "Are you alive?" (If no, restart).
Readiness Probe: "Are you ready to accept traffic?" (If no, don't send traffic, but don't restart yet - maybe DB is connecting).

## 💻 Implementation

Use `@nestjs/terminus`.

```typescript
@Controller("health")
export class HealthController {
  constructor(
    private health: HealthCheckService,
    private db: TypeOrmHealthIndicator
  ) {}

  @Get()
  @HealthCheck()
  check() {
    return this.health.check([() => this.db.pingCheck("database")]);
  }
}
```

## 🧩 Activity / Challenge

1.  Install Terminus.
2.  Add `/health` endpoint to Gateway and Orders service.
3.  Kill the database container.
4.  See the health check return 503.

## 🔑 Key Takeaways

- Microservices without health checks are zombies. You won't know they are broken until users complain.
