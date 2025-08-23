
# Lab 01: Run Containers with docker run

## Goal
Run NGINX locally and access it via browser.

## Steps
```bash
docker run -d --name web -p 8080:80 nginx
docker ps
# verify (wsl - Linux)
curl -I http://localhost:8080 | head -n 1    # expect: HTTP/1.1 200 OK
```
#### Open from Windows (if you want a browser):
visit http://localhost:8080

## Clean up
```
docker rm -f web
```

