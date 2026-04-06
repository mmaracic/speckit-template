# Backend Developer Guidelines & Constraints

This document captures backend developer input to be used when generating the project constitution via `speckit.constitution`.
Answer or refine each section before running the constitution agent.

---

## 1. Python Setup & Tooling

- **Python version**: Minimum Python version (3.11, 3.12, 3.13)?
- **Package / dependency manager**: Mandatory use of uv tool for project management?
- **Dependency versions**: Are all dependency versions exactly pinned in `pyproject.toml` or `requirements.txt`, with a shared lockfile committed to the repo?
- **Project layout**: `src/` layout or flat layout?
- **Virtual environment**: `.venv` in project root, or per-service virtual environments for polyrepo?
- **pyproject.toml**: Used as the single source of project metadata?

---

## 2. Application Configuration

- **Configuration management**: Is Pydantic `BaseSettings` used for application configuration, with environment variable support, YAML support, and `.env` file loading?
- **Secrets management**: Are secrets stored only in environment variables and never hardcoded or committed to version control? Is a secrets manager used in production?
- **YAML config files**: Are YAML files used for application settings (as opposed to environment variables only)?
- **Configuration precedence**: Do environment variables take precedence over YAML configuration files?
- **YAML validation**: Are YAML files loaded and validated via Pydantic models? Are they documented in project setup instructions?
- **No default values in YAML models**: Are Pydantic models for YAML config free of default values so that missing fields always raise errors?
- **YAML validation errors**: Do both missing and extra fields in the YAML config raise validation errors to prevent misconfiguration?
- **YAML parsing**: Which library is used for YAML parsing (`pyyaml`, `ruamel.yaml`, or other)?
- **YAML config file location**: Where is the config file located (e.g., `src/config.yaml`) and where is the corresponding Pydantic model defined (e.g., `src/config.py`)?

---

## 3. Web Framework

- **Framework**: FastAPI or Django?
- **ASGI vs WSGI**: Async (ASGI / uvicorn) or sync (WSGI / gunicorn)?
- **Resource routing**: Are FastAPI `APIRouter` instances used for modular route organisation, with clear separation between routers, services, and data models?
- **Dependency setup / lifespan**: Is FastAPI `lifespan` used for application startup and shutdown events? Are all dependencies, configurations, and resources (e.g. DB connections, HTTP clients) initialised in the lifespan context?
- **OpenAPI docs**: Auto-generated at `/docs` (Swagger UI) and `/redoc`? Access restricted in production?
- **Middleware stack**: Which middleware is standard (CORS, request ID, compression, timing)?

---

## 4. Project Structure & Layering

- **Application structure**: Is `src/` the main source code folder and `tests/` used for all tests?
- **Cross-cutting concerns**: Are cross-cutting concerns (e.g. database, auth, logging) placed in separate modules under `src/`?
- **Functional code organisation**: Is functional code organised by layer first and then by feature (e.g. `src/api/users`, `src/services/users`, `src/models/users`)?
- **Application entry point**: Is there a single `src/main.py` that creates the app, includes routers, and sets up the lifespan context?
- **Architecture pattern**: Clean Architecture, Hexagonal, or simple layered (routes → services → repositories)?
- **Dependency injection**: Framework DI (FastAPI `Depends`), a DI container (lagom, injector), or manual?
- **Domain models vs ORM models**: Are Pydantic domain models kept separate from ORM table models?

---

## 5. Database & ORM

- **No in-memory database**: Is a real database (e.g. PostgreSQL) used even in development, with a shared docker-compose setup for local development and testing?
- **Test data isolation**: Do tests use transaction rollbacks or explicit cleanup to avoid leaving residual data in the database?
- **Table and column name constants**: Are all table names and column names defined as constants in a dedicated module (e.g. `src/db/constants.py`) to avoid magic strings?
- **Audit columns**: Do all tables include `created_at`, `updated_at`, `created_by`, and `updated_by` columns that are automatically populated on create and update?
- **ORM / query builder**: SQLAlchemy, Tortoise, raw SQL, or other?
- **Migrations**: Alembic or other tooling?

---

## 6. Validation & Serialisation

- **Schema library**: Mandatory use of Pydantic v2
- **Input validation location**: At the API boundary only, or also in the service layer?
- **Error response format**: RFC 7807 (Problem Details), custom JSON, or framework default?
- **Strict mode**: Is Pydantic `model_config = ConfigDict(strict=True)` standard?

---

## 7. Authentication & Security

- **Identity management**: Is a dedicated identity provider (e.g. Keycloak, Auth0, Azure AD, AWS Cognito) used for user management and authentication flows, rather than custom logic in the application?
- **Authentication method**: What is the authentication mechanism (e.g. JWT with access and refresh tokens, session cookies, API keys)?
- **Token format**: JWT (PyJWT / python-jose), opaque tokens, or session cookies?
- **Password hashing**: bcrypt (passlib), argon2, or other?
- **Secrets**: Environment variables via `python-dotenv`, Pydantic `BaseSettings`, or secrets manager?
- **CORS origins**: Explicitly configured; no wildcard `*` in production?
- **SQL injection prevention**: ORM parameterised queries only; raw SQL is audited?
- **Rate limiting**: `slowapi`, nginx-level, or API gateway?

---

## 8. Testing

- **Test runner**: pytest (confirmed per project instructions)?
- **Fixtures and mocks**: `pytest` fixtures; `unittest.mock` or `pytest-mock`?
- **Async test support**: `pytest-asyncio` with `asyncio_mode = "auto"`?
- **HTTP client for tests**: `httpx` (ASGI transport) or `requests`?
- **Factory library**: `factory_boy`, `polyfactory`, or manual fixture builders?
- **Minimum coverage**: Coverage threshold enforced in CI?
- **Test categories**: Unit, integration, and end-to-end clearly separated?
- **Unit test quality**: Do unit tests cover individual functions and methods in isolation using mocks, run fast, and include edge cases and error handling?

---

## 9. Code Quality & Conventions

- **Linter**: `ruff` (lint + format), or `flake8` + `black` + `isort`?
- **Type checker**: `mypy` or `pyright`? Strict mode enabled?
- **Docstrings**: Required on all public functions and classes?
- **Logging**: `structlog` for structured JSON logs, or standard `logging` with JSON formatter?
- **Sensitive data in logs**: Is Pydantic `SecretStr` used for sensitive fields to ensure they are redacted when logged?
- **Trace IDs in logs**: Are trace IDs included in all log messages for observability and debugging?
- **No print statements**: Is `logging` used instead of `print` for all output?
- **Lazy log formatting**: Are log messages formatted lazily (e.g. `logger.debug("User %s logged in", user_id)`) to avoid unnecessary string interpolation?
- **Constants & enums**: No magic strings; enums for fixed value sets, constants for other repeated values?
- **No `**kwargs` / `*args`**: Explicit function signatures always?
- **No default argument values**: Are default argument values in functions avoided to prevent unintended behaviour?
- **No global state**: Are global variables and mutable state outside function scope avoided?
- **No raw dictionaries**: Are Pydantic models used instead of raw `dict` for all structured data?
- **Typing**: Are type hints applied everywhere, with strict mypy checks and `Any` avoided unless justified?
- **Classes in separate files**: Is each class defined in its own file with the filename matching the class name?
- **No `Optional`**: Are union types (e.g. `str | None`) used instead of `Optional` for nullable fields?
- **Private attribute prefix**: Are private attributes and methods prefixed with a leading underscore?
- **No hard deletes**: Are soft deletes (e.g. `is_deleted` flag) used instead of hard deletes?
- **Interfaces / ABCs**: Are abstract base classes used to define contracts between layers and modules?
- **Composition over inheritance**: Is composition preferred over inheritance?
- **No circular dependencies**: Are circular imports between modules and layers avoided?
- **Design patterns**: Are appropriate design patterns (factory, singleton, strategy, observer, etc.) applied where relevant?

---

## 10. Background Tasks & Async Patterns

- **Background jobs**: Celery + Redis/RabbitMQ, ARQ, FastAPI `BackgroundTasks`, or none for MVP?
- **Scheduled tasks**: Celery Beat, APScheduler, cron, or none?
- **Long-running operations**: Async endpoints + webhooks, or polling?
- **Event publishing**: Direct DB writes, outbox pattern, or message broker?

---

## 11. API Design Conventions

- **API style**: RESTful JSON APIs, GraphQL, gRPC, or hybrid?
- **API versioning**: Is the version included in the URL path (e.g. `/api/v1/users`)?
- **URL style**: kebab-case paths (`/user-profiles`) or snake_case?
- **HTTP verbs**: Standard REST semantics (GET read, POST create, PUT replace, PATCH update, DELETE remove)?
- **Pagination**: Cursor-based, offset/limit, or page-number?
- **Filtering & sorting**: Query-parameter conventions (e.g. `?sort=-created_at&filter[status]=active`)?
- **Envelope vs. bare responses**: `{ "data": ..., "meta": ... }` wrapper, or bare objects?
- **Date/time format**: ISO 8601 UTC strings everywhere?

---

## 12. Open Questions

Questions that must be answered before the constitution can be finalised:

- [ ] Is WebSocket or Server-Sent Events support required?
- [ ] Are there multi-tenancy requirements affecting the data model?
- [ ] Is soft-delete (logical delete) the standard for all entities, or hard-delete?
- [ ] Are audit trails / change history required for any entities?
- [ ] Is the API versioned from day one, or added when the first breaking change occurs?
- [ ] Are there any third-party webhook integrations requiring signature verification?
- [ ] Is OpenTelemetry instrumentation (traces + metrics) required in the application layer?