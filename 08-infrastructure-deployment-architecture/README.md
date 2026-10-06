# 08 — Infrastructure / Deployment Architecture

> Where does everything run, and how does it get there?

Roughly TOGAF Technology Architecture.

## Example
```text
Internet → Cloudflare → Load Balancer → Kubernetes
→ App Containers → PostgreSQL + Redis
```

## Topics
- Cloud, servers, Docker, Kubernetes, service mesh (Istio/Linkerd)
- Load balancing, autoscaling, networking, CDN, storage
- Regions, AZs, disaster recovery
- Blue-green, canary, feature flags, IaC

## Checklist
- [ ] Regions / AZs + DR story
- [ ] Scaling triggers defined (CPU/latency/queue)
- [ ] Deploy strategy per risk (canary for critical)
- [ ] Secrets / config separated from image

## Artifacts
- Deployment diagram, runbook links
