# Lab 02: Minikube Setup (WSL + Docker Desktop)

## Goal
Start a local Kubernetes cluster with **minikube** (Docker driver), verify it works, and expose a test app.

---

## Prerequisites (WSL)
- Windows 10/11 with **WSL2**
- **Docker Desktop** running, with *WSL integration* enabled for your distro
- `minikube` + `kubectl` installed in WSL (or use `minikube kubectl -- ...`)

Quick checks:
```bash
docker version            # should show Server: Docker Engine - Community
minikube version
kubectl version --client  # ok if only client shows
```
Verify:
```
kubectl config current-context      # expect: minikube
kubectl get nodes                   # 1 node Ready
minikube status
```
