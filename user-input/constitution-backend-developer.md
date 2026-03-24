# Backend Developer Guidelines & Constraints

This document captures backend developer input to be used when generating the project constitution via `speckit.constitution`.
Answer or refine each section before running the constitution agent.

---

## 1. Python Setup & Tooling

- **Python version**: Minimum Python version (3.11, 3.12, 3.13)?
- **Package / dependency manager**: Mandatory use of uv tool for project management
- **Project layout**: `src/` layout or flat layout?
- **Virtual environment**: `.venv` in project root
- **pyproject.toml**: Used as the single source of project metadata?

---

## 2. Web Framework

- **Framework**: FastAPI
- **ASGI vs WSGI**: Async (ASGI / uvicorn) or sync (WSGI / gunicorn)?
- **OpenAPI docs**: Auto-generated at `/docs` (Swagger UI) and `/redoc`? Access restricted in production?
- **Middleware stack**: Which middleware is standard (CORS, request ID, compression, timing)?

---

## 3. Project Structure & Layering

- **Architecture pattern**: Clean Architecture, Hexagonal, or simple layered (routes → services → repositories)?
- **Module organisation**: Feature-based (`auth/`, `users/`, `posts/`) or layer-based (`routers/`, `services/`, `models/`)?
- **Dependency injection**: Framework DI (FastAPI `Depends`), a DI container (lagom, injector), or manual?
- **Domain models vs ORM models**: Are Pydantic domain models kept separate from ORM table models?

---

## 4. Database & ORM

- **Test database**: Dedicated test DB, SQLite in-memory, or transaction rollback per test?

---

## 5. Validation & Serialisation

- **Schema library**: Mandatory use of Pydantic v2
- **Input validation location**: At the API boundary only, or also in the service layer?
- **Error response format**: RFC 7807 (Problem Details), custom JSON, or framework default?
- **Strict mode**: Is Pydantic `model_config = ConfigDict(strict=True)` standard?

---

## 6. Authentication & Security

- **Token format**: JWT (PyJWT / python-jose), opaque tokens, or session cookies?
- **Password hashing**: bcrypt (passlib), argon2, or other?
- **Secrets**: Environment variables via `python-dotenv`, Pydantic `BaseSettings`, or secrets manager?
- **CORS origins**: Explicitly configured; no wildcard `*` in production?
- **SQL injection prevention**: ORM parameterised queries only; raw SQL is audited?
- **Rate limiting**: `slowapi`, nginx-level, or API gateway?

---

## 7. Testing

- **Test runner**: pytest (confirmed per project instructions)?
- **Fixtures and mocks**: `pytest` fixtures; `unittest.mock` or `pytest-mock`?
- **Async test support**: `pytest-asyncio` with `asyncio_mode = "auto"`?
- **HTTP client for tests**: `httpx` (ASGI transport) or `requests`?
- **Factory library**: `factory_boy`, `polyfactory`, or manual fixture builders?
- **Minimum coverage**: Coverage threshold enforced in CI?
- **Test categories**: Unit, integration, and end-to-end clearly separated?

---

## 8. Code Quality & Conventions

- **Linter**: `ruff` (lint + format), or `flake8` + `black` + `isort`?
- **Type checker**: `mypy` or `pyright`? Strict mode enabled?
- **Docstrings**: Required on all public functions and classes (confirmed per project instructions)?
- **Logging**: `structlog` for structured JSON logs, or standard `logging` with JSON formatter?
- **Constants & enums**: No magic strings; enums preferred (confirmed per project instructions)?
- **No `**kwargs` / `*args`**: Explicit signatures always (confirmed per project instructions)?

---

## 9. Background Tasks & Async Patterns

- **Background jobs**: Celery + Redis/RabbitMQ, ARQ, FastAPI `BackgroundTasks`, or none for MVP?
- **Scheduled tasks**: Celery Beat, APScheduler, cron, or none?
- **Long-running operations**: Async endpoints + webhooks, or polling?
- **Event publishing**: Direct DB writes, outbox pattern, or message broker?

---

## 10. API Design Conventions

- **URL style**: kebab-case paths (`/user-profiles`) or snake_case?
- **HTTP verbs**: Standard REST semantics (GET read, POST create, PUT replace, PATCH update, DELETE remove)?
- **Pagination**: Cursor-based, offset/limit, or page-number?
- **Filtering & sorting**: Query-parameter conventions (e.g. `?sort=-created_at&filter[status]=active`)?
- **Envelope vs. bare responses**: `{ "data": ..., "meta": ... }` wrapper, or bare objects?
- **Date/time format**: ISO 8601 UTC strings everywhere?

---

## 11. Open Questions

Questions that must be answered before the constitution can be finalised:

- [ ] Is WebSocket or Server-Sent Events support required?
- [ ] Are there multi-tenancy requirements affecting the data model?
- [ ] Is soft-delete (logical delete) the standard for all entities, or hard-delete?
- [ ] Are audit trails / change history required for any entities?
- [ ] Is the API versioned from day one, or added when the first breaking change occurs?
- [ ] Are there any third-party webhook integrations requiring signature verification?
- [ ] Is OpenTelemetry instrumentation (traces + metrics) required in the application layer?
