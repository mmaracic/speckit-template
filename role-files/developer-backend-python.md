# Backend Developer Python Guidelines & Constraints

First section of this document captures Python backend developer constitution principles to be used when generating the project constitution via `speckit.constitution`. Second section defines the gate checklists. Here there is only one checklist and it applies to the task and implementation speckit steps.
Every mandatory bullet with an `BE-` or `BE-CHECK-` ID must be copied into the generated constitution and downstream plan checks with one-to-one traceability. Mandatory bullets must not be skipped, merged away, or summarized.
---

## 0. Copy and Traceability Principles

- **MANDATORY:** [BE-COPY-001] Every mandatory source item in this file must be copied into generated constitution artifacts.
- **MANDATORY:** [BE-COPY-002] Mandatory source items must not be skipped, merged, or summarized into generic wording.
- **MANDATORY:** [BE-COPY-003] If a generated constitution needs more sections or principles to preserve fidelity, add them instead of collapsing items.
- **MANDATORY:** [BE-COPY-004] Generated constitution artifacts must include a traceability matrix that maps each `BE-` and `BE-CHECK-` ID.
- **MANDATORY:** [BE-COPY-005] Each source ID must map to exactly one explicit constitution clause or one explicit plan-check entry, unless a justified exception is documented.

---

## 1. Mandatory Constitution principles
### 1. Python Setup & Tooling

- **MANDATORY:** [BE-001] **Python version**: Use Python version 3.12
- **MANDATORY:** [BE-002] **Package / dependency manager**: Mandatory use of uv tool for project management
- **MANDATORY:** [BE-003] **Dependency versions**: Use exact pinned versions for all dependencies in `pyproject.toml` or `requirements.txt` files, with a shared lockfile committed to the repo.
- **MANDATORY:** [BE-004] **Virtual environment**: `.venv` in project root in case of monorepo, otherwise per-service virtual environments and `pyproject.toml` files
- **MANDATORY:** [BE-005] **Linter**: Use `ruff` for linting with a shared config committed to the repo. Use `black` for code formatting with a shared config committed to the repo.
- **MANDATORY:** [BE-006] **Type checker**: Use `mypy` with strict mode enabled.

---

### 2. Application Configuration

- **MANDATORY:** [BE-007] **Configuration management**: Use Pydantic `BaseSettings` for application configuration, with environment variable support, yaml support and `.env` file loading via `python-dotenv`.
- **MANDATORY:** [BE-008] **Secrets management**: Store secrets in environment variables, and ensure that they are not hardcoded in the codebase or committed to version control. Use a secrets manager for production deployments if necessary.
- **MANDATORY:** [BE-009] **Yaml config files**: Use YAML configuration files for application settings.
- **MANDATORY:** [BE-010] **Configuration precedence**: Environment variables have higher precedence over YAML configuration files.
- **MANDATORY:** [BE-011] **YAML validation**: YAML files must be loaded and validated via Pydantic models, and documented in the project setup instructions.
- **MANDATORY:** [BE-012] **No default values**: Do not use default values in the Pydantic models for YAML config to ensure that missing fields are caught as errors.
- **MANDATORY:** [BE-013] **YAML validation errors**: Both missing and extra fields in the YAML config should raise validation errors to prevent misconfiguration.
- **MANDATORY:** [BE-014] **YAML parsing**: Use `pyyaml` for YAML parsing.
- **MANDATORY:** [BE-015] **YAML config file**: File should be located in the folder `src/` and named `config.yaml` for each application, with a corresponding `Config` Pydantic model defined in `src/config.py`.

---

### 3. Web Framework

- **MANDATORY:** [BE-016] **Framework**: FastAPI is a mandatory choice for the web framework, given its modern features, async support, and strong ecosystem.
- **MANDATORY:** [BE-017] **ASGI vs WSGI**: Use uvicorn as the ASGI server for FastAPI applications.
- **MANDATORY:** [BE-018] **Resource routing**: Use FastAPI's APIRouter for modular route organization, with clear separation of concerns between routers, services, and data models.
- **MANDATORY:** [BE-019] **Dependency setup**: Use lifespan for application startup and shutdown events. Set up all dependencies, configurations and resources (e.g. database connections, HTTP clients) in the lifespan context to ensure proper initialization and cleanup.
- **MANDATORY:** [BE-020] **OpenAPI docs**: Auto-generate at `/docs` (Swagger UI) and `/redoc`.

---

### 4. Project Structure & Layering

- **MANDATORY:** [BE-021] **Application structure**: Every application has to have `src` as the main source code folder, and `tests` for tests.
- **MANDATORY:** [BE-022] **Cross-cutting module structure**: Cross-cutting concerns (e.g. database, auth) should be in separate modules under `src`.
- **MANDATORY:** [BE-023] **Functional code organisation**: Functional code should be organised by layers first and then by feature. For example, `src/api/users`, `src/services/users`, `src/models/users`.
- **MANDATORY:** [BE-024] **Application entry point**: The application entry point should be a single `src/main.py` file that creates the FastAPI app, includes the routers, and sets up the lifespan context.

---

### 5. Database & ORM

- **MANDATORY:** [BE-025] **No in-memory database**: Use a real database (e.g. PostgreSQL) even in development, with a shared docker-compose setup for local development and testing.
- **MANDATORY:** [BE-026] **Tests should not taint the database**: Use test transactions with rollbacks, or delete test data after each test, to ensure that tests do not leave residual data.
- **MANDATORY:** [BE-027] **Table names and columns in constants**: Define all table names and column names as constants in a dedicated module (e.g. `src/db/constants.py`) to avoid magic strings and ensure consistency across the codebase.
- **MANDATORY:** [BE-028] **Auditing**: Always include `created_at`, `updated_at`, `created_by`, and `updated_by` columns in all tables for auditing purposes, and ensure that they are automatically set on record creation and update.

---

### 6. Validation & Serialisation

- **MANDATORY:** [BE-029] **Schema library**: Mandatory use of Pydantic v2 for all data validation and serialization needs, including request bodies, response models, and internal data structures.
- **MANDATORY:** [BE-030] **Error response format**: RFC 7807 (Problem Details) standard for error responses, with a consistent structure for all errors.

---

### 7. Authentication & Security

- **MANDATORY:** [BE-031] **Identity management**: Use a dedicated identity provider related to the target cloud platform or open-source solution (e.g. Keycloak, Auth0, AWS Cognito) for user management and authentication flows, rather than implementing custom user management logic in the application.
- **MANDATORY:** [BE-032] **Authentication method**: JWT-based authentication with access and refresh tokens.
- **MANDATORY:** [BE-033] **Token format**: JWT.
- **MANDATORY:** [BE-034] **Secrets**: All secrets (e.g. JWT signing keys) must be stored securely in environment variables and not hardcoded in the codebase or committed to version control.
- **MANDATORY:** [BE-035] **CORS origins**: Explicitly configured list of allowed origins for CORS, with no wildcard (`*`) allowed in production.
- **MANDATORY:** [BE-036] **SQL injection prevention**: ORM parameterised queries only; raw SQL is audited and parameterised.

---

### 8. Testing

- **MANDATORY:** [BE-037] **Test runner**: Use pytest.
- **MANDATORY:** [BE-038] **Fixtures and mocks**: Use `pytest` fixtures with `unittest.mock`.
- **MANDATORY:** [BE-039] **Async test support**: Use `pytest-asyncio`.
- **MANDATORY:** [BE-040] **HTTP client for tests**: Use `requests`.
- **MANDATORY:** [BE-041] **Minimum coverage**: Target 90% code coverage, with no critical paths (e.g. business logic, data access) below 80%.
- **MANDATORY:** [BE-042] **Test categories**: Use Unit and integration tests, with clear guidelines on what belongs in each category.
- **MANDATORY:** [BE-043] **Edge cases**: Unit tests must test individual functions and methods in isolation using mocks for external dependencies. Tests must be fast and must cover edge cases and error handling.

---

### 9. Code Quality & Conventions

- **MANDATORY:** [BE-044] **Docstrings**: Required on all functions, classes and modules.
- **MANDATORY:** [BE-045] **Logging**: Use standard `logging` with JSON formatter and structured log messages, with appropriate log levels (DEBUG, INFO, WARNING, ERROR).
- **MANDATORY:** [BE-046] **Sensitive data**: Use Pydantic `SecretStr` for any sensitive data that might be logged to ensure it is redacted.
- **MANDATORY:** [BE-047] **Constants & enums**: No magic strings; enums preferred for fixed sets of values, and constants for other repeated values.
- **MANDATORY:** [BE-048] **No `**kwargs` / `*args`**: Explicit signatures always, no `**kwargs` or `*args` to ensure clarity and type safety.
- **MANDATORY:** [BE-049] **No default args**: Avoid default argument values in functions to prevent unintended behavior and ensure that all required parameters are explicitly provided.
- **MANDATORY:** [BE-050] **No global state**: Avoid global variables or mutable state outside of function scope to ensure thread safety and predictability.
- **MANDATORY:** [BE-051] **No dictionaries**: Use Pydantic models for all structured data, and avoid using raw dictionaries or JSON objects in the codebase to ensure type safety and validation.
- **MANDATORY:** [BE-052] **Typing**: Use Python type hints everywhere, with strict mypy checks enabled. Avoid `Any` type unless absolutely necessary, and always use explicit types for function signatures, variables, and data structures.
- **MANDATORY:** [BE-053] **Classes in separate files**: Each class should be defined in its own file, with the filename matching the class name, to ensure modularity and clarity in the codebase.
- **MANDATORY:** [BE-054] **No Optional**: Use union types (e.g. `str | None`) instead of `Optional` for nullable fields to improve readability and consistency with modern Python syntax.
- **MANDATORY:** [BE-055] **Underscore for private attributes**: Use a leading underscore for private attributes and methods to indicate that they are intended for internal use only.
- **MANDATORY:** [BE-056] **Lazy formatting**: Use lazy formatting for log messages (e.g. `logger.debug("User %s logged in", user_id)`) to avoid unnecessary string interpolation when the log level is not enabled.
- **MANDATORY:** [BE-057] **No print statements**: Use logging instead of print statements for all output to ensure proper log management and avoid cluttering standard output.
- **MANDATORY:** [BE-058] **No hard deletes**: Use soft deletes (e.g. `is_deleted` flag) instead of hard deletes to prevent data loss and allow for recovery if needed.
- **MANDATORY:** [BE-059] **Trace IDs in logs**: Include trace IDs in all log messages for better observability and debugging in distributed systems.
- **MANDATORY:** [BE-060] **Interfaces**: Use interfaces (e.g. abstract base classes) to define clear contracts between layers and modules and decouple implementations.
- **MANDATORY:** [BE-061] **Composition over inheritance**: Prefer composition over inheritance to promote flexibility and maintainability.
- **MANDATORY:** [BE-062] **No circular dependencies**: Avoid circular dependencies between modules and layers to ensure a clean architecture and prevent import issues.
- **MANDATORY:** [BE-063] **Design patterns**: Use appropriate design patterns to solve common problems and improve code maintainability (factory, singleton, strategy, observer etc.).

---

### 10. Background Tasks & Async Patterns

- **MANDATORY:** [BE-064] **Background jobs**: Use FastAPI `BackgroundTasks`.
- **MANDATORY:** [BE-065] **Scheduled tasks**: Use APScheduler.
- **MANDATORY:** [BE-066] **Long-running operations**: Use async endpoints + webhooks for callbacks, or a message broker (e.g. RabbitMQ, Kafka) for event-driven processing.
- **MANDATORY:** [BE-067] **Event publishing**: Use Kafka.

---

### 11. API Design Conventions

- **MANDATORY:** [BE-068] **API style**: RESTful JSON APIs with consistent conventions across all endpoints.
- **MANDATORY:** [BE-069] **API versioning**: Include version in the URL path (e.g. `/api/v1/users`).
- **MANDATORY:** [BE-070] **URL style**: kebab-case paths (`/user-profiles`).
- **MANDATORY:** [BE-071] **HTTP verbs**: Standard REST semantics (GET read, POST create, PUT replace, PATCH update, DELETE remove).
- **MANDATORY:** [BE-072] **Pagination**: Offset-based pagination with `?page=1&size=20` query parameters, and total count in response metadata.
- **MANDATORY:** [BE-073] **Filtering & sorting**: Query-parameter conventions (e.g. `?sort=-created_at&filter[status]=active`).
- **MANDATORY:** [BE-074] **Envelope vs. bare responses**: `{ "data": ..., "meta": ... }` wrapper.
- **MANDATORY:** [BE-075] **Date/time format**: ISO 8601 UTC strings everywhere.

---

## 2. Gate Checklists
### 1. Task and Implementation Gate Checklist

- [ ] [BE-CHECK-001] Is Python 3.12 used?
- [ ] [BE-CHECK-002] Is `uv` used as the package and dependency manager?
- [ ] [BE-CHECK-003] Are all dependency versions exactly pinned in `pyproject.toml` or `requirements.txt` with a committed lockfile?
- [ ] [BE-CHECK-004] Is the virtual environment located in `.venv` in the project root (monorepo) or per-service otherwise?
- [ ] [BE-CHECK-005] Is `ruff` used for linting and `black` for formatting, each with a shared config committed to the repo?
- [ ] [BE-CHECK-006] Is `mypy` enabled with strict mode?
- [ ] [BE-CHECK-007] Is Pydantic `BaseSettings` used for application configuration with environment variable, YAML, and `.env` file support?
- [ ] [BE-CHECK-008] Are secrets stored in environment variables and absent from the codebase and version control?
- [ ] [BE-CHECK-009] Are YAML files used for application settings?
- [ ] [BE-CHECK-010] Do environment variables take precedence over YAML configuration files?
- [ ] [BE-CHECK-011] Are YAML files loaded and validated via Pydantic models and documented in project setup instructions?
- [ ] [BE-CHECK-012] Are Pydantic models for YAML config free of default values so missing fields raise errors?
- [ ] [BE-CHECK-013] Do both missing and extra fields in YAML config raise validation errors?
- [ ] [BE-CHECK-014] Is `pyyaml` used for YAML parsing?
- [ ] [BE-CHECK-015] Is the YAML config file located at `src/config.yaml` with a corresponding `Config` model in `src/config.py`?
- [ ] [BE-CHECK-016] Is FastAPI used as the web framework?
- [ ] [BE-CHECK-017] Is `uvicorn` used as the ASGI server?
- [ ] [BE-CHECK-018] Are routes organised using FastAPI's `APIRouter` with clear separation between routers, services, and data models?
- [ ] [BE-CHECK-019] Is `lifespan` used for startup and shutdown events, with all dependencies and resources initialised there?
- [ ] [BE-CHECK-020] Are OpenAPI docs auto-generated and served at `/docs` and `/redoc`?
- [ ] [BE-CHECK-021] Is `src/` the main source code folder and `tests/` used for tests?
- [ ] [BE-CHECK-022] Are cross-cutting concerns placed in separate modules under `src/`?
- [ ] [BE-CHECK-023] Is functional code organised by layer first and then by feature (e.g. `src/api/users`, `src/services/users`)?
- [ ] [BE-CHECK-024] Is the application entry point a single `src/main.py` file that creates the app, includes routers, and sets up lifespan?
- [ ] [BE-CHECK-025] Is a real database (e.g. PostgreSQL) used in development with a shared docker-compose setup?
- [ ] [BE-CHECK-026] Do tests use transactions with rollbacks or clean up test data to avoid tainting the database?
- [ ] [BE-CHECK-027] Are table names and column names defined as constants in a dedicated module (e.g. `src/db/constants.py`)?
- [ ] [BE-CHECK-028] Do all tables include `created_at`, `updated_at`, `created_by`, and `updated_by` columns that are automatically populated?
- [ ] [BE-CHECK-029] Is Pydantic v2 used for all data validation and serialisation?
- [ ] [BE-CHECK-030] Are error responses formatted according to RFC 7807 (Problem Details)?
- [ ] [BE-CHECK-031] Is a dedicated identity provider used for user management and authentication, rather than custom logic?
- [ ] [BE-CHECK-032] Is JWT-based authentication with access and refresh tokens used?
- [ ] [BE-CHECK-033] Is JWT used as the token format?
- [ ] [BE-CHECK-034] Are JWT signing keys and other secrets stored in environment variables and not hardcoded?
- [ ] [BE-CHECK-035] Is the CORS allowed-origins list explicitly configured with no wildcard in production?
- [ ] [BE-CHECK-036] Are all database queries parameterised via ORM, and is raw SQL audited and parameterised?
- [ ] [BE-CHECK-037] Is `pytest` used as the test runner?
- [ ] [BE-CHECK-038] Are `pytest` fixtures used with `unittest.mock` for mocking?
- [ ] [BE-CHECK-039] Is `pytest-asyncio` used for async test support?
- [ ] [BE-CHECK-040] Is `requests` used as the HTTP client in tests?
- [ ] [BE-CHECK-041] Is 90% overall code coverage targeted, with no critical paths below 80%?
- [ ] [BE-CHECK-042] Are unit and integration tests used with clear guidelines on what belongs in each category?
- [ ] [BE-CHECK-043] Are docstrings required on all functions, classes, and modules?
- [ ] [BE-CHECK-044] Is standard `logging` used with a JSON formatter and structured messages at appropriate log levels?
- [ ] [BE-CHECK-045] Is Pydantic `SecretStr` used for sensitive data that may appear in logs?
- [ ] [BE-CHECK-046] Are magic strings avoided, with enums used for fixed value sets and constants for other repeated values?
- [ ] [BE-CHECK-047] Are `*args` and `**kwargs` absent from function signatures?
- [ ] [BE-CHECK-048] Are default argument values avoided in functions?
- [ ] [BE-CHECK-049] Are global variables and mutable state outside function scope avoided?
- [ ] [BE-CHECK-050] Are Pydantic models used instead of raw dictionaries for all structured data?
- [ ] [BE-CHECK-051] Are type hints applied everywhere, with strict mypy checks and no use of `Any` unless justified?
- [ ] [BE-CHECK-052] Is each class defined in its own file with the filename matching the class name?
- [ ] [BE-CHECK-053] Are union types (e.g. `str | None`) used instead of `Optional` for nullable fields?
- [ ] [BE-CHECK-054] Are private attributes and methods prefixed with a leading underscore?
- [ ] [BE-CHECK-055] Are log messages formatted lazily (e.g. `logger.debug("User %s logged in", user_id)`)?
- [ ] [BE-CHECK-056] Is `logging` used instead of `print` statements for all output?
- [ ] [BE-CHECK-057] Are soft deletes (e.g. `is_deleted` flag) used instead of hard deletes?
- [ ] [BE-CHECK-058] Are trace IDs included in all log messages?
- [ ] [BE-CHECK-059] Is FastAPI `BackgroundTasks` used for background jobs?
- [ ] [BE-CHECK-060] Is APScheduler used for scheduled tasks?
- [ ] [BE-CHECK-061] Are long-running operations handled via async endpoints with webhooks or a message broker?
- [ ] [BE-CHECK-062] Is Kafka used for event publishing?
- [ ] [BE-CHECK-063] Is a RESTful JSON API style followed with consistent conventions across all endpoints?
- [ ] [BE-CHECK-064] Is the API version included in the URL path (e.g. `/api/v1/users`)?
- [ ] [BE-CHECK-065] Are URL path segments written in kebab-case (e.g. `/user-profiles`)?
- [ ] [BE-CHECK-066] Are HTTP verbs used with standard REST semantics (GET read, POST create, PUT replace, PATCH update, DELETE remove)?
- [ ] [BE-CHECK-067] Is offset-based pagination used with `?page=1&size=20` parameters and total count in response metadata?
- [ ] [BE-CHECK-068] Are filtering and sorting expressed as query parameters (e.g. `?sort=-created_at&filter[status]=active`)?
- [ ] [BE-CHECK-069] Are responses wrapped in a `{ "data": ..., "meta": ... }` envelope?
- [ ] [BE-CHECK-070] Are all date and time values formatted as ISO 8601 UTC strings?
- [ ] [BE-CHECK-071] Are abstract base classes (interfaces) used to define contracts between layers and modules, decoupling implementations?
- [ ] [BE-CHECK-072] Is composition preferred over inheritance to promote flexibility and maintainability?
- [ ] [BE-CHECK-073] Are circular dependencies between modules and layers avoided?
- [ ] [BE-CHECK-074] Are appropriate design patterns (factory, singleton, strategy, observer, etc.) applied to solve common problems and improve maintainability?
- [ ] [BE-CHECK-075] Do unit tests focus on individual functions and methods in isolation, use mocks for external dependencies, run fast, and cover edge cases and error handling?
- [ ] [BE-CHECK-076] Do all the databases have defined provider (e.g., PostgreSQL, MySQL) and type (e.g., relational, NoSQL)?
