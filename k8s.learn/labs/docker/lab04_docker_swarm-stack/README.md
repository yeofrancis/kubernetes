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

```
Compose (single host)              Swarm (cluster)                            Kubernetes (cluster)
+-------------------+              +--------------------+   +----------------+  +--------------------+   +----------------+
| Docker Engine     |              | manager (Raft)     |---| worker         |  | control plane      |---| worker node    |
| compose services  |              |  services/stacks   |   |  services      |  |  API, etcd, ctrlr, |   |  kubelet, pods |
| containers only   |              |  overlay network   |   |  overlay net   |  |  scheduler         |   |  kube-proxy    |
+-------------------+              +--------------------+   +----------------+  +--------------------+   +----------------+
(single host)                       (multi-node orchestrator)                   (multi-node orchestrator, rich ecosystem)
```
| Topic            | Docker Compose (single host)          | Docker Swarm (cluster)                        | Kubernetes (cluster)                                  |
|------------------|---------------------------------------|-----------------------------------------------|-------------------------------------------------------|
| Scope            | One machine, multi-container dev      | Multi-node orchestrator (services, stacks)    | Multi-node orchestrator (pods, deployments, etc.)     |
| Scaling          | `--scale` flag                        | `docker service scale`                        | `kubectl scale` (HPA/auto available)                  |
| LB / Service Disc| Basic container links                 | Built-in VIP + DNS, overlay networks          | Services/Endpoints, kube-proxy, CNI                   |
| Config format    | `compose.yaml`                        | `docker-stack.yml` (Compose v3)               | YAML manifests (`Deployment`, `Service`, etc.)        |
| CLI              | `docker compose up/down`              | `docker swarm init`, `docker stack deploy`    | `kubectl apply/get/describe/rollout`                  |
| Typical use      | Local dev, simple multi-service apps  | Simple clustering with Docker UX              | Production, rich ecosystem & extensibility            |

## Compose
docker compose up -d
docker compose ps
docker compose down

## Swarm
docker swarm init
docker stack deploy -c docker-stack.yml app
docker service scale app_web=3
docker stack rm app
docker swarm leave --force

## Kubernetes
kubectl create deployment web --image=nginx:alpine
kubectl expose deployment web --port=80 --type=NodePort
kubectl scale deployment web --replicas=3
kubectl set image deployment/web nginx=nginx:1.27-alpine
kubectl rollout status deployment/web
kubectl delete svc/web deploy/web

