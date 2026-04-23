# vue-social-feed - Architecture

## Layer Diagram

```

              Client  (Browser / Mobile)

                         HTTPS

          Reverse Proxy  (nginx / Traefik)

               FastAPI Application

Middleware     Routes      Lifespan
(audit,      /api/v1   (db+cache
rate lim)                init)

              Service Layer
    Auth  Item  Notif  Analytics  Search

    PostgreSQL               Redis
    (SQLAlchemy           (cache +
     async)               email queue)

```

## Technology Choices

| Layer | Technology | Reason |
|-------|-----------|--------|
| API framework | FastAPI 0.115 | Async, auto OpenAPI, dependency injection |
| ORM | SQLAlchemy 2 (async) | Type-safe, async sessions, Alembic migrations |
| DB | PostgreSQL 16 | Reliability, JSONB, full-text search |
| Cache | Redis 7 | Low-latency key/value, pub/sub |
| Auth | python-jose + passlib | Industry-standard JWT + bcrypt |
| Validation | Pydantic v2 | Fast, Rust-backed validation |
| Testing | pytest-asyncio + httpx | Async-native test client |

## Database Schema (key tables)

```
users           notifications      items

id PK           id PK               id PK
email UNIQUE    user_id FK->users    title
hashed_password title               description
full_name       body                owner_id FK->users
role            type                is_active
is_active       read                view_count
created_at      created_at          created_at

audit_logs      user_settings       tags / item_tags

id PK           id PK               tags.id PK
user_id FK      user_id FK UNIQUE   tags.name UNIQUE
action          theme               tags.slug
resource_type   language            item_tags.item_id FK
resource_id     timezone            item_tags.tag_id FK
ip_address      notifications_ena
created_at      updated_at
```

## Deployment

**Development:**
```bash
docker compose up -d   # starts postgres + redis
make dev               # uvicorn hot-reload on :8000
```

**Production:**
- Build Docker image: `docker compose build`
- Set `DEBUG=false`, strong `SECRET_KEY`, valid `DATABASE_URL`
- Run migrations: `alembic upgrade head`
- Start: `gunicorn src.main:app -k uvicorn.workers.UvicornWorker -w 4`
- Serve static files via nginx, terminate TLS at proxy

## Background Workers

- `src/workers/cleanup.py` - cron daily, purges audit logs >90 days, orphaned uploads
- `src/workers/email_worker.py` - long-running, pops from `email:queue` Redis list
