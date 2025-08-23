# Docker Fundamentals (notes)

- Images vs Containers
- `docker run`, `ps`, `logs`, `exec`
- Port mapping `-p`, volumes `-v`
- Tagging/pushing images

Quick refs:
```bash
docker run -d -p 8080:80 nginx
docker ps -a
docker logs <id>
docker exec -it <id> sh

