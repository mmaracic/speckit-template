# Architect Guidelines & Constraints

This document captures architect input to be used when generating the project constitution via `speckit.constitution`.
Answer or refine each section before running the constitution agent.

---

## 1. System Architecture Style

- **Repository structure**: Monorepo with frontend and backend in the same repo, or separate repos for each?
- **Overall style**: Monolith, modular monolith, microservices, or serverless?
- **API style**: REST, GraphQL, gRPC, or hybrid?
- **Frontend/backend coupling**: Fully decoupled (separate deployments) or server-rendered (SSR/hybrid)?
- **Communication pattern**: Synchronous HTTP only, or event-driven (message queue / pub-sub) as well?
- **Event technology**: If event-driven, which technology for the event bus (RabbitMQ, Kafka, Redis Streams)?
- **Shared libraries**: Is there a shared code library between frontend and backend (e.g. OpenAPI-generated types)?
- **Third-party integrations**: Any critical external systems or APIs that must be integrated from the start?
- **Real-time features**: Are WebSockets, Server-Sent Events, Long Polling, or similar technologies required for real-time updates?
- **Background processing**: Are there any tasks that require scheduled or asynchronous background processing (e.g. Celery, ARQ, RQ)?


---

## 2. Layering & Module Boundaries

- What are the top-level modules/packages for the backend (e.g. `api`, `domain`, `infrastructure`)?
- What are the top-level feature areas for the frontend (e.g. `auth`, `dashboard`, `shared`)?
- Is there a shared domain library between frontend and backend (e.g. OpenAPI-generated types)?
- Are cross-cutting concerns (logging, auth, error handling) centralised or per-module?
- **Dependency direction rules**: Which layers are allowed to depend on which? (e.g. domain must not import infrastructure)
- **Architecture as code**: Will architecture-as-code tooling be used to enforce and document layer and module constraints (e.g., ArchUnit, Dependency Cruiser)?
- **Interface / contract format**: Is the API contract defined as an OpenAPI spec, GraphQL schema, or derived from code?
- **Error handling strategy**: Are errors mapped to domain types at the boundary, or propagated as-is from infrastructure?

---

## 3. Data Layer

- **Primary datastore**: Which database engine and version?
- **SQL vs NoSQL**: Relational, document, key-value, graph, or multi-model?
- **ORM / query builder**: SQLAlchemy, Tortoise, raw SQL, or other?
- **Migrations**: Alembic, or other tooling?
- **Caching layer**: Redis, in-memory, or none?
- **Search**: Full-text search required? (Elasticsearch, pg_trgm, etc.)
- **File/blob storage**: Local filesystem, S3-compatible, or CDN-backed?
- **Read/write separation**: Are read replicas or CQRS required, or a single read/write connection?
- **Multi-tenancy isolation**: Row-level security, separate schemas per tenant, or single shared tables?
- **Soft delete vs. hard delete**: Is logical (soft) delete the standard for all entities?
- **Audit trail**: Are change history or audit logs required for any entity type?

---

## 4. Authentication & Authorisation

- **Auth mechanism**: JWT, session cookies, OAuth2 + OIDC, or API keys?
- **Identity provider**: Self-hosted, Auth0, Keycloak, Entra ID, or other?
- **Authorisation model**: RBAC, ABAC, simple user/admin split or custom authorisation?
- **Token storage on the frontend**: httpOnly cookie (recommended) or localStorage?
- **Token refresh strategy**: Sliding refresh tokens, silent renewal, or short-lived tokens with re-login?
- **Session expiry policy**: Absolute timeout, idle timeout, or both?
- **Multi-factor authentication (MFA)**: Required from launch, optional, or post-MVP?
- **Inter-service authentication**: How do internal backend services authenticate to each other (mTLS, shared secret, service tokens)?

---

## 5. API Contract & Versioning

- **API versioning strategy**: URL prefix (`/v1/`), header, or none?
- **Schema-first or code-first**: OpenAPI spec generated from code, or spec defined first?
- **Backward compatibility policy**: How many old API versions must be supported simultaneously?
- **Internal vs. public API**: Are some endpoints public (unauthenticated) and others private?
- **Pagination strategy**: Cursor-based, offset/limit, or page-number? Standard query-parameter names?
- **Error response format**: RFC 7807 Problem Details, custom JSON envelope, or framework default?
- **Webhook support**: Are inbound and/or outbound webhooks required? Signature verification strategy?
- **File upload handling**: Multipart form upload, presigned URL (S3), or chunked upload?

---

## 6. Events and Messaging

- **Event-driven architecture**: Is event-driven communication required? Which services or components will communicate asynchronously via events?
- **Message broker**: Which message broker will be used (Kafka, RabbitMQ, Azure Service Bus, Redis Streams)?
- **Event versioning**: How will backward compatibility of events be maintained when schemas change?
- **Event storage**: Should events be persisted in an event store or database for auditing and replay purposes?
- **Idempotent consumers**: Are event handlers designed to be idempotent? What is the retry and deduplication strategy for failed event processing?
- **Scaling event processing**: How will event processing be scaled (consumer groups, partitioning)? What are the expected load levels and known scaling bottlenecks?
- **Eventual consistency**: Which workflows rely on eventual consistency, and are there critical flows that require strong consistency guarantees instead?
- **Event structure**: What standard structure will events follow (metadata fields, payload format)? Where will event schemas be stored (e.g., `events.md`, schema registry)?

---

## 7. Infrastructure & Deployment

- **Containerisation**: Docker mandatory? Docker Compose for local dev? Serverless or PaaS options acceptable?
- **Cloud provider**: AWS, Azure, GCP, or cloud-agnostic?
- **Deployment strategy**: CI/CD pipelines with GitHub Actions, GitLab CI, or local scripts?
- **Orchestration**: Kubernetes, ECS, App Service, or PaaS (Render, Railway, Fly.io)?
- **Environment topology**: How many environments (local, dev, staging, prod)?
- **Secrets management**: `.env` files, Vault, cloud-native secrets manager?
- **CDN**: Is a CDN required in front of the frontend or static assets?
- **Network**: Any VPC, private subnet, or firewall requirements?
- **Infrastructure as Code**: Terraform, Bicep, Pulumi, or manual provisioning?
- **Deployment strategy**: Blue-green, canary, rolling update, or recreate?
- **Health check endpoints**: Standard `/health` (liveness) and `/ready` (readiness) endpoints required?
- **Auto-scaling policy**: CPU/memory thresholds, request-rate-based, or manual scaling?

---

## 8. Observability

- **Logging**: Structured JSON logs? Log aggregation platform (Datadog, Loki, CloudWatch)?
- **Metrics**: Prometheus + Grafana, or cloud-native monitoring?
- **Tracing**: OpenTelemetry required? Distributed tracing across services?
- **Error tracking**: Sentry, Rollbar, or equivalent?
- **Alerting**: Who gets alerted, on what conditions, through which channel?
- **SLI / SLO definitions**: Are Service Level Indicators and Objectives formally defined?
- **Log retention policy**: How long must logs be retained? Any compliance-driven minimum?
- **Synthetic monitoring**: Uptime / smoke tests against production endpoints required?

---

## 9. AI

- **LLM / model provider**: Which AI model or provider will be used (e.g., OpenAI, Azure OpenAI, Anthropic)? Is provider independence required?
- **AI abstraction layer**: Will an abstraction library (e.g., LiteLLM, LangChain, Semantic Kernel) be used to avoid vendor lock-in?
- **AI tracing and monitoring**: How will AI interactions (prompts, responses, latency, costs) be logged, traced, and monitored?
- **Use-case justification**: Which tasks are delegated to AI, and is there a clear justification that they cannot be solved with simpler approaches?
- **Evaluation and testing**: How will AI outputs be evaluated (unit tests for deterministic outputs, benchmarks, human review for non-deterministic outputs)?
- **Human in the loop**: Is there a mechanism for human review and override of AI outputs in production workflows?
- **Data privacy for AI**: How is sensitive or PII data handled before being sent to AI providers? Is anonymisation or redaction required?

---

## 10. Security Architecture

- **Transport**: TLS everywhere? Certificate management (Let's Encrypt, ACM)?
- **Input validation**: Where is validation enforced — API boundary, domain layer, or both?
- **Query parameter sanitisation**: Are ORM parameterised queries enforced to prevent SQL injection? Are raw SQL queries audited?
- **Rate limiting / throttling**: Required at API gateway or application level?
- **CORS policy**: Which origins are allowed?
- **Dependency scanning**: Automated CVE scanning in CI (Dependabot, Snyk, Trivy)?
- **OWASP Top 10**: Which items require explicit architectural mitigations?
- **Encryption at rest**: Is database and/or blob storage encryption at rest required?
- **Audit logging**: Must all user actions (create, update, delete) be logged with actor and timestamp?
- **Penetration testing**: Is a security review or pen-test required before go-live?

---

## 11. Scalability & Resilience

- **Horizontal scaling**: Are stateless services assumed? Session affinity needed?
- **Database connection pooling**: PgBouncer, built-in pool, or ORM-managed?
- **Retry & circuit-breaker patterns**: Required for external integrations?
- **Graceful degradation**: What happens when a downstream service is unavailable?
- **Disaster recovery**: RTO and RPO targets? Backup strategy?
- **Load testing**: Is load / stress testing required before launch? Target RPS / TPS?
- **Cold start mitigation**: If serverless is used, are cold start latencies acceptable or must be mitigated?

---

## 12. Data Privacy & Compliance

- **GDPR / data-privacy law**: Which jurisdictions apply? Is a Data Processing Agreement (DPA) required?
- **PII inventory**: Which fields are classified as personally identifiable information?
- **Data residency**: Must data remain in a specific geographic region?
- **Right to erasure**: Is automated account/data deletion required?
- **Cookie consent**: Is a consent banner and preference centre required?
- **Data breach notification**: Is there an incident-response plan and notification SLA?

---

## 13. Developer Experience

- **Local dev setup**: Single command to start all services (Docker Compose, Makefile, `just`)?
- **Port conventions**: Agreed localhost ports for API, frontend, database, cache?
- **Hot reload**: Required for both frontend and backend in local dev mode?
- **Seed / fixture data**: Is a one-command database seed required for a usable dev environment?
- **Pre-commit hooks**: Linting, formatting, and type-check hooks enforced before commit?

---

## 14. Architecture Decision Records (ADRs)

- Will ADRs be maintained in the repository? If so, which folder?
- How are architectural decisions reviewed and approved?
- Which decisions are already made and non-negotiable?

---

## 15. Architecture Diagrams

- **Required diagram types**: Which diagrams are required (e.g., high-level system architecture, deployment architecture, sequence diagrams, data flow diagrams)?
- **Diagram format**: What format will be used for diagrams (Mermaid, PlantUML, draw.io, C4 model)?
- **Storage location**: Where will diagrams be stored in the repository (e.g., `diagrams/` folder)?
- **Diagram update process**: When and how will diagrams be updated (e.g., as part of the ADR process, during architecture reviews, after implementation changes)?
- **Deployment diagram scope**: Should the deployment diagram include cloud provider specifics (regions, managed services, VPCs, networking)?

---

## 16. Open Questions

Questions that must be answered before the constitution can be finalised:

- [ ] Is a dedicated API gateway (Kong, Nginx, Traefik) in scope?
- [ ] Are there multi-region or high-availability requirements?
- [ ] Is the frontend a SPA, or will there be SSR/SSG pages (Next.js-style)?
- [ ] Are background jobs / task queues required (Celery, ARQ, etc.)?
- [ ] Is a WebSocket / real-time connection layer needed?
- [ ] What is the branching strategy (trunk-based, Gitflow)?
- [ ] Is Infrastructure as Code mandatory from day one, or added when the first cloud environment is provisioned?
- [ ] Are there any compliance certifications required (SOC 2, ISO 27001, PCI DSS)?
- [ ] Is a service mesh (Istio, Linkerd) in scope if microservices are adopted?
- [ ] How are breaking API changes communicated to consumers (deprecation headers, changelogs, notifications)?