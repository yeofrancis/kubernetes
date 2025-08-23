
# Docker Compose v2 (notes)

- Format: `compose.yaml` or `docker-compose.yml`
- Run with `docker compose up -d` (no hyphen in v2)
- Useful for multi-service dev workflows (db + app + cache)

Common fields: `services`, `ports`, `volumes`, `environment`, `depends_on`, `healthcheck`.
