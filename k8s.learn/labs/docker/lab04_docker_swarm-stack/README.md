# Lab 05: Swarm stack with replicas

## Goal
Enable Swarm, deploy a stack with multiple replicas, scale, and observe.

## Files
- `docker-stack.yml`

**docker-stack.yml**
```yaml
version: "3.9"
services:
  web:
    image: nginx:alpine
    ports:
      - "8083:80"
    deploy:
      replicas: 3
      restart_policy:
        condition: on-failure
```
**Steps**
```
# enable swarm (single-node manager in WSL)
docker swarm init

# deploy stack
docker stack deploy -c docker-stack.yml webstack

# inspect
docker stack services webstack
docker service ls
docker service ps webstack_web

# hit it
curl -I http://localhost:8083 | head -n 1
```
**Scale**
```
docker service scale webstack_web=5
docker service ps webstack_web
```
**Clean up**
```
docker stack rm webstack
docker swarm leave --force
```
### Swarm vs Compose vs Kubernetes (mental model)

- **Docker Compose** — single **host**, multi-container dev workflows.
  - Run: `docker compose up -d`
  - Good for: local development, simple multi-service setups.

- **Docker Swarm** — **cluster** orchestrator (multi-node).
  - Concepts: node, service, task, stack; rolling updates, built-in LB, secrets.
  - Run: `docker swarm init` → `docker stack deploy -c stack.yml app`
  - Good for: small teams that want simple clustering with Docker UX.

- **Kubernetes** — de-facto **cluster** orchestrator (multi-node) with a huge ecosystem.
  - Concepts: pod, deployment, service, ingress, HPA, etc.
  - Run: `kubectl apply -f k8s.yaml`
  - Good for: production, scalability, extensibility (CRDs/operators), cloud-native tooling.

