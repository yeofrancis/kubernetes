# Writing Dockerfiles (notes)

Key instructions: `FROM`, `RUN`, `COPY`, `WORKDIR`, `EXPOSE`, `CMD`, `ENTRYPOINT`.
Best practices:
- Start from small bases (e.g., `alpine`)
- One process per container
- Pin versions; avoid `latest` in prod
- Use multi-stage builds to reduce size

Quick example:
```Dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```
