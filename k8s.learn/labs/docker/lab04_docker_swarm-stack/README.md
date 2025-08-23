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
Swarm vs Compose vs K8s:
 - Compose: great for single-host dev (multiple containers).
 - Swarm: clustering & rolling updates, simple UX.
 - Kubernetes: industry-standard orchestrator for prod.
