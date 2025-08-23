# Lab 03: Build an NGINX image from a Dockerfile

## Goal
Package a simple static site into an image and run it.

## Files
Create these in this folder:
- `Dockerfile`
- `index.html`

**Dockerfile**
```Dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
```

**index.html**
```
<!doctype html>
<html><body><h1>Hello from my custom NGINX image</h1></body></html>
```

**Steps**
```
# build (tag with your name)
docker build -t mynginx:1.0 .

# run
docker run -d --name web -p 8081:80 mynginx:1.0

# verify 
curl -I http://localhost:8081 | head -n 1   # expect HTTP/1.1 200 OK
```

**Clean up**
```
docker rm -f web
docker rmi mynginx:1.0
```
