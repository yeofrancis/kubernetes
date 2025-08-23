# Lab 04: NGINX via Docker Compose

## Goal
Use **Compose v2** to run NGINX with a bind-mounted site folder.

## Files
- `compose.yaml`
- `html/index.html`

**compose.yaml**
```yaml
services:
  web:
    image: nginx:alpine
    ports:
      - "8082:80"
    volumes:
      - ./html:/usr/share/nginx/html:ro
```
**html/index.html**
```
<!doctype html>
<html><body><h1>Compose says hello 👋</h1></body></html>
```
**Steps**
```
docker compose up -d
docker compose ps
curl -I http://localhost:8082 | head -n 1
```
**Clean up**
```
docker compose down
```
