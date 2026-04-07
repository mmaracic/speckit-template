# Architect Guidelines & Constraints

First section of this document captures architect constitution principles to be used when generating the project constitution via `speckit.constitution`. Second section captures project-specific overrides and additions. Third section defines the gate checklists. Here there is only one checklist and it applies to the plan speckit step.
Every mandatory bullet with an `ARCH-` or `ARCH-CHECK-` ID must be copied into the generated constitution and downstream plan checks with one-to-one traceability. Mandatory bullets must not be skipped, merged away, or summarized.
---

## 0. Copy and Traceability Principles

- **MANDATORY:** [ARCH-COPY-001] Every mandatory source item in this file must be copied into generated constitution artifacts.
- **MANDATORY:** [ARCH-COPY-002] Mandatory source items must not be skipped, merged, or summarized into generic wording.
- **MANDATORY:** [ARCH-COPY-003] If a generated constitution needs more sections or principles to preserve fidelity, add them instead of collapsing items.
- **MANDATORY:** [ARCH-COPY-004] Generated constitution artifacts must include a traceability matrix that maps each `ARCH-` and `ARCH-CHECK-` ID.
- **MANDATORY:** [ARCH-COPY-005] Each source ID must map to exactly one explicit constitution clause or one explicit plan-check entry, unless a justified exception is documented.

---

## 1. Mandatory Constitution principles
### 1. System Architecture Style

- **MANDATORY:** [ARCH-001] **Repository structure**: Use Monorepo when a high degree of coupling between components and a single team works on them. Use Polyrepo when components are expected to be independently deployable or reusable across projects or multiple teams work on them.
- **MANDATORY:** [ARCH-002] **Overall style**: Always use modular style. Use Modular Monolith for simplicity and ease of development when the system is not expected to grow beyond a certain complexity. Use Microservices for large, complex systems with clear bounded contexts and the need for independent scalability or deployment. Use service based architecture as a middle ground when some modularity, scalability and independent deployment is desired but speed is a priority too.
- **MANDATORY:** [ARCH-003] **API style**: Use REST when simple and widely supported approach is needed. GraphQL is beneficial when clients need flexible queries and to minimize over-fetching. gRPC is ideal for high-performance, low-latency communication between internal services. Use hybrid approaches when different API styles are needed for different use cases (e.g., REST for public API, gRPC for internal services).
- **MANDATORY:** [ARCH-004] **Frontend/backend coupling**: Use fully decoupled architectures (separate deployments) for larger applications with distinct frontend and backend teams, or when the frontend must be reusable across multiple platforms (web, mobile). Use server-rendered (SSR/hybrid) architectures for smaller applications or when SEO is a concern, but account for the tighter coupling between frontend and backend this introduces.
- **MANDATORY:** [ARCH-005] **Background processing**: Use background processing for long-running or resource-intensive tasks that do not need to be completed within the request-response cycle. Choose a task queue (e.g., Celery, ARQ) based on language and ecosystem compatibility, ease of use, and scalability requirements.
- **MANDATORY:** [ARCH-006] **Mobile applications**: Define if there is a mobile application requirement and if so, whether it will be native (iOS/Android), cross-platform (Flutter, React Native) or a mobile web app, and ensure the architecture supports the chosen approach.
---

### 2. Module Boundaries
- **MANDATORY:** [ARCH-007] **Module granularity**: Define clear boundaries for each module based on business capabilities or domain concepts. Each module should have a single responsibility and encapsulate its internal implementation details.
- **MANDATORY:** [ARCH-008] **Interface / contract format**: Define API contracts using OpenAPI for REST, GraphQL SDL for GraphQL, or protobuf for gRPC. Contracts should be the source of truth for API design and implementation. Use AVRO or JSON Schema for event based communication contracts. Avoid code-first approaches that  lead to drift between implementation and design.
- **MANDATORY:** [ARCH-009] **Architecture as code**: Define layer, module and application interaction constraints using a formal architecture-as-code approach
- **MANDATORY:** [ARCH-010] **Communication pattern**: Synchronous HTTP is simpler to implement and reason about and avoids eventual consistency issues. Use it in simple applications and where fast response time is not critical. Use asynchronous HTTP for non-blocking operations. Event-driven architectures (message queue / pub-sub) provides better scalability, reliability and decoupling and is used for service based or microservices architectures.
- **MANDATORY:** [ARCH-011] **Event technology**: If event-driven, use Kafka for scalability, reliability and lack of back-pressure. 
- **MANDATORY:** [ARCH-012] **Shared libraries**: Use shared libraries for common utilities, domain models, and API contracts to avoid duplication and ensure consistency across services especially when services represent similar use cases. However, be cautious of tight coupling and versioning issues that can arise from shared libraries.
- **MANDATORY:** [ARCH-013] **Third-party integrations**: Prefer to use API based integrations based on REST or queue based integrations using Kafka. Avoid tight coupling with third-party SDKs or libraries that can create maintenance issues.
- **MANDATORY:** [ARCH-014] **Real-time features**: First choice should be long pooling for simplicity, then WebSockets for true real-time bidirectional communication, and finally server-sent events (SSE) for unidirectional real-time updates from server to client.
---

### 3. Data Layer

- **MANDATORY:** [ARCH-015] **SQL vs NoSQL**: Use open source relational databases (PostgreSQL, MySQL) where reliability and strong consistency guarantees are required. Use NoSQL databases (MongoDB, DynamoDB) when flexible schemas, horizontal scalability or high write throughput are needed.
- **MANDATORY:** [ARCH-016] **Database connection pooling**: Use connection pooling
- **MANDATORY:** [ARCH-017] **Caching layer**: Add a caching layer (Redis, Memcached) for frequently accessed data or expensive computations when speed is top priority. Use cache-aside pattern for simplicity and control over cache invalidation.
- **MANDATORY:** [ARCH-018] **File/blob storage**: Use cloud blob storage (Azure Blob Storage, AWS S3) for large files, unstructured data or binary data that doesnt need elaborate querying capabilities. Use database BLOBs for json when transactional consistency with other data or advanced querying capabilities are required.
- **MANDATORY:** [ARCH-019] **Audit**: Add auditing in any enterprise application. Minimum audit includes create and update date and user. Add more advanced versioned change history if required by the domain.

---

### 4. Authentication & Authorisation

- **MANDATORY:** [ARCH-020] **Auth mechanism**: Use JWT + Outh2 for user facing UI and APIs. Use API keys for service-to-service authentication. Avoid rolling your own auth mechanism or using basic auth for production applications.
- **MANDATORY:** [ARCH-021] **Identity provider**: Prefer cloud based identity providers (Azure AD, AWS Cognito) for ease of integration and management. Use open source identity providers (Keycloak, Ory) when customisation or on-premises hosting is required.
- **MANDATORY:** [ARCH-022] **Authorisation model**: Use open source implementations of common authorisation models (RBAC, ABAC) to avoid reinventing the wheel.
---

### 5. API Contract & Versioning

- **MANDATORY:** [ARCH-023] **API versioning strategy**: URL prefix (`/v1/`), version attribute in events.
- **MANDATORY:** [ARCH-024] **Schema-first**: OpenAPI spec generated from code
- **MANDATORY:** [ARCH-025] **Contract testing**: Use contract testing tools (Pact, Postman) to ensure API implementations meet the defined contracts and to prevent breaking changes.
- **MANDATORY:** [ARCH-026] **Pagination strategy**: Use pagination on all search endpoints and all endpoints whose response can grow unbounded. Use offset-based pagination for simplicity.
---

### 6. Events and Messaging

- **MANDATORY:** [ARCH-027] **Event-driven architecture**: Use events to decouple services and enable asynchronous communication. Define clear event schemas and topics.
- **MANDATORY:** [ARCH-028] **Message broker**: Use a message broker (Kafka, RabbitMQ, Azure Service Bus) for event distribution and processing.
- **MANDATORY:** [ARCH-029] **Event versioning**: Maintain backward compatibility for events and use versioning when breaking changes are necessary.
- **MANDATORY:** [ARCH-030] **Event storage**: Persist events in an event store or database for auditing and replay purposes.
- **MANDATORY:** [ARCH-031] **Event processing**: Use idempotent event handlers and implement retry logic to handle transient failures in event processing.
- **MANDATORY:** [ARCH-032] **Scaling event processing**: Use consumer groups and partitioning in Kafka to scale event processing horizontally. Describe the expected load and scaling strategy for event processing in the architecture design. Describe scaling limitations and bottlenecks of the chosen event processing approach and how they will be mitigated.
- **MANDATORY:** [ARCH-033] **Eventual consistency**: If using an event-driven architecture, eventual consistency has to be detected and pointed out in the design. Event-driven communication has to be avoided for critical user flows and operations that require strong consistency guarantees.
- **MANDATORY:** [ARCH-034] **Event structure**: Define a standard structure for events, including metadata (timestamp, event type, source) and payload. Use a consistent format (e.g., JSON, Avro) for event payloads. Generate the structure in a machine-readable format (e.g., JSON Schema, Avro schema) and store it in the `events.md` file to ensure consistency and ease of use across the system.

### 7. Infrastructure & Deployment

- **MANDATORY:** [ARCH-035] **Containerisation**: Use Docker when the application needs to be always online, has complex dependencies or needs to run in different environments with minimal changes. Use serverless (Azure Functions, AWS Lambda) for simple, event-driven applications with low to moderate traffic and where low cost is a priority.
- **MANDATORY:** [ARCH-036] **Cloud provider**: Separate cloud specific code from business logic to different modules to allow for easier migration if needed. Use cloud provider managed services (Azure App Service, AWS Elastic Beanstalk) for ease of deployment and management when using containerisation. Use serverless offerings (Azure Functions, AWS Lambda) when using serverless architecture.
- **MANDATORY:** [ARCH-037] **Deployment strategy**: Use automated deployment pipelines (GitHub Actions, Azure DevOps) to ensure consistent and reliable deployments.
- **MANDATORY:** [ARCH-038] **Testing** Run tests on all branches in all environments. Use staging environment that mirrors production for final testing before release.
- **MANDATORY:** [ARCH-039] **Environment topology**: Use at least four environments (local, dev, staging, prod).
- **MANDATORY:** [ARCH-040] **Secrets management**: `.env` files for local development, Vault, cloud-native secrets manager for production.
- **MANDATORY:** [ARCH-041] **Deployment strategy**: Use rolling updates when enough resources are available.
- **MANDATORY:** [ARCH-042] **Health check endpoints**: Standard `/health` (liveness) and `/ready` (readiness) endpoints required.
- **MANDATORY:** [ARCH-043] **Auto-scaling policy**: Use Http scalers for services that have APIs or event-based scalers for services that process events. Define clear scaling thresholds based on performance goals and expected traffic patterns.

---

### 8. Observability

- **MANDATORY:** [ARCH-044] **Logging**: Structured JSON logs. Use cloud-native logging services (Azure Monitor, AWS CloudWatch) or open source solutions (ELK stack) for log aggregation and analysis.
- **MANDATORY:** [ARCH-045] **Metrics**: Prometheus + Grafana, or cloud-native monitoring.
- **MANDATORY:** [ARCH-046] **Tracing**: Use OpenTelemetry for distributed tracing and integrate with a tracing backend (Jaeger, Zipkin, or cloud-native tracing services).

### 9. AI
- **MANDATORY:** [ARCH-047] **Provider independence**: Design AI integration in a provider-agnostic way using OpenLLM.
- **MANDATORY:** [ARCH-048] **Tracing and monitoring**: Ensure AI interactions are logged and monitored for performance and cost management using.
- **MANDATORY:** [ARCH-049] **Minimal model usage**: Use AI for tasks that provide clear value and cannot be easily solved with traditional programming or existing data science algorithms to manage costs and complexity.
- **MANDATORY:** [ARCH-050] **Evaluation and testing**: Implement robust evaluation and testing strategies for AI outputs, including unit tests for deterministic outputs and human-in-the-loop review for non-deterministic outputs.
- **MANDATORY:** [ARCH-051] **Human in the loop**: Ensure there is a mechanism for human review in all workflows that use AI and override to prevent harmful or incorrect AI outputs from causing issues in production.
- **MANDATORY:** [ARCH-052] **Data privacy**: Ensure that any data sent to AI providers is anonymized and does not contain personally identifiable information (PII) or sensitive data, in compliance with data privacy regulations.
---

### 10. Security Architecture

- **MANDATORY:** [ARCH-053] **Transport**: Use TLS everywhere. Use Let's Encrypt if no provider-managed TLS is available.
- **MANDATORY:** [ARCH-054] **Input validation**: Validation must be enforced at the API boundary at minimum; define explicitly whether it is also enforced at the domain layer, and document the rationale.
- **MANDATORY:** [ARCH-055] **Query parameter sanitisation**: Use ORM parameterised queries to prevent SQL injection; raw SQL queries must be audited.
- **MANDATORY:** [ARCH-056] **CORS policy**: Use restrictive CORS policy in all environments, with explicit allowed origins. Avoid wildcard `*` in production.
- **MANDATORY:** [ARCH-057] **Dependency scanning**: Automated CVE scanning in CI.
---

### 11. Scalability & Resilience

- **MANDATORY:** [ARCH-058] **Horizontal scaling**: Define stateful or thread unsafe applications that cannot be horizontally scaled without additional work. Use stateless design principles to enable horizontal scaling where possible.
- **MANDATORY:** [ARCH-059] **Retry & circuit-breaker patterns**: Required for all APIs
- **MANDATORY:** [ARCH-060] **Disaster recovery**: Define backup and recovery strategies for databases and critical data. Use cloud provider managed backup solutions when available.
- **MANDATORY:** [ARCH-061] **Load testing**: Define load and stress testing requirements, use cases and procedures, including target RPS/TPS and acceptable latency thresholds.
---

### 12. Architecture Decision Records (ADRs)

- **MANDATORY:** [ARCH-062] Maintain ADRs in the `adrs/` directory to document all significant architectural decisions, including the context, decision, alternatives considered, and rationale.
- **MANDATORY:** [ARCH-063] ADRs must be created for decisions such as choice of architecture style, API design conventions, data storage solutions, authentication mechanisms, and deployment strategies.
- **MANDATORY:** [ARCH-064] When a significant architectural change is proposed, an ADR must be created to document the change and its rationale, even if it is a reversal of a previous decision.
- **MANDATORY:** [ARCH-065] When ADR is based on a constitution principle, the ADR must explicitly reference the relevant principle ID (e.g., "This decision is based on [ARCH-002]") to maintain traceability between the constitution and architectural decisions.
- **MANDATORY:** [ARCH-066] ADRs must be reviewed and updated as the architecture changes to ensure they remain relevant and accurate.
---

### 13. Architecture Diagrams
- **MANDATORY:** [ARCH-067] It is mandatory to create architecture diagrams for the system. These diagrams should include:
  - **MANDATORY:** [ARCH-068] High-level system architecture diagram showing major components and their interactions including applications, databases and event queues.
  - **MANDATORY:** [ARCH-069] Deployment architecture diagram showing how the application is deployed in Azure environment.
- **MANDATORY:** [ARCH-070] Diagrams must be created using a mermaid format and stored into separate files into the `diagrams/` folder. They must be included in the feature artifacts and updated as needed when the architecture evolves.
- **MANDATORY:** [ARCH-071] Diagrams must be kept up to date and reflect the current state of the architecture. They must be reviewed and updated as part of the architectural decision process and during implementation when changes are made.
- **MANDATORY:** [ARCH-072] Diagrams must be clear, concise and easy to understand, providing a visual representation of the architecture that complements the written documentation and ADRs.
- **MANDATORY:** [ARCH-073] Diagrams must minimise the number of intersections and overlapping lines and objects to ensure readability. They should use consistent symbols and notation to represent different types of components and interactions.

## 2. Project-Specific Principles

_Add project-specific overrides or additions to the principles in Section 1. Use the same MANDATORY bullet format with sequential `[ARCH-NNN]` IDs. Leave this section empty if there are no project-specific constraints._

---

## 3. Gate Checklists
### 1. Plan Gate Checklist
- [ ] [ARCH-CHECK-001] What is the chosen architecture style (monolith, microservices, service-based) and why?
- [ ] [ARCH-CHECK-002] What architectural characteristics and constraints are most relevant for each application and service (e.g., modularity, scalability, deployment independence, reliability, high availability)?
- [ ] [ARCH-CHECK-003] What are the applications and services that will make up the system?
- [ ] [ARCH-CHECK-004] Did we define all communication patterns and technologies between applications and services (e.g., REST, gRPC, message queues)?
- [ ] [ARCH-CHECK-005] Did we define the data storage technologies and strategies (e.g., SQL vs NoSQL, caching, file storage)?
- [ ] [ARCH-CHECK-006] What API style(s) will be used (REST, GraphQL, gRPC) and why?
- [ ] [ARCH-CHECK-007] What is the repository structure (monorepo vs polyrepo) and why?
- [ ] [ARCH-CHECK-008] What is the expected scale of the application in terms of users, transactions, and data volume?
- [ ] [ARCH-CHECK-009] Are there any specific regulatory or compliance requirements that need to be considered in the architecture?
- [ ] [ARCH-CHECK-010] What are the expected performance and latency requirements for the application?
- [ ] [ARCH-CHECK-011] Are there any existing systems or technologies that need to be integrated with, and what are their constraints?
- [ ] [ARCH-CHECK-012] Are there any specific security requirements or threat models that need to be addressed in the architecture?
- [ ] [ARCH-CHECK-013] What authentication and authorization mechanisms will be used?
- [ ] [ARCH-CHECK-014] What is the cloud provider and deployment strategy for each application and service?
- [ ] [ARCH-CHECK-015] Is there an architecture diagram for cloud provider?
- [ ] [ARCH-CHECK-016] What is the expected timeline for development and how does it influence the choice of architecture style and technologies?
- [ ] [ARCH-CHECK-017] Are there any libraries or frameworks that are preferred or mandated for use in the project?
- [ ] [ARCH-CHECK-018] Where will the application be deployed (e.g., Azure, AWS, on-premises) and how does that influence architectural decisions?
- [ ] [ARCH-CHECK-019] Are all diagrams included in the plan artifacts and do they reflect the current architectural decisions?
- [ ] [ARCH-CHECK-020] Are all diagrams valid mermaid syntax and render correctly?
- [ ] [ARCH-CHECK-021] Is the structure of all events described in `events.md` file and do they reflect the current design of the system?
- [ ] [ARCH-CHECK-022] Is the structure of all API contracts described in `contracts/` folder and do they reflect the current design of the system?
- [ ] [ARCH-CHECK-023] Are all database schemas described in `data-model.md` folder and do they reflect the current design of the system?
- [ ] [ARCH-CHECK-024] Are all ADRs included in the plan artifacts and do they reflect the current architectural decisions?
- [ ] [ARCH-CHECK-025] Is the logging strategy defined, including structured JSON logs and a log aggregation platform (e.g. Azure Monitor, ELK stack)?
- [ ] [ARCH-CHECK-026] Is the metrics strategy defined (e.g. Prometheus + Grafana or cloud-native monitoring)?
- [ ] [ARCH-CHECK-027] Is distributed tracing defined using OpenTelemetry and integrated with a tracing backend (e.g. Jaeger, Zipkin, or cloud-native)?
- [ ] [ARCH-CHECK-028] Is AI integration designed in a provider-agnostic way (e.g. using OpenLLM) to avoid vendor lock-in?
- [ ] [ARCH-CHECK-029] Are AI interactions (prompts, responses, latency, and costs) logged and monitored?
- [ ] [ARCH-CHECK-030] Is AI usage limited to tasks that provide clear value and cannot be easily solved with traditional approaches?
- [ ] [ARCH-CHECK-031] Are evaluation and testing strategies defined for AI outputs, including unit tests for deterministic outputs and human-in-the-loop review for non-deterministic outputs?
- [ ] [ARCH-CHECK-032] Is there a mechanism for human review and override of AI outputs in all production workflows?
- [ ] [ARCH-CHECK-033] Is the frontend/backend coupling strategy defined (fully decoupled deployments vs. server-rendered/SSR), and is the rationale documented?
- [ ] [ARCH-CHECK-034] Are background processing requirements identified, and is a task queue technology chosen (e.g. Celery, ARQ) based on language compatibility and scalability needs?
- [ ] [ARCH-CHECK-035] Are module boundaries defined based on business capabilities or domain concepts, with each module having a single responsibility?
- [ ] [ARCH-CHECK-036] Is architecture-as-code tooling used to define and enforce layer, module, and application interaction constraints?
- [ ] [ARCH-CHECK-037] Is the shared library strategy defined — which code is shared, and are versioning and coupling risks addressed?
- [ ] [ARCH-CHECK-038] Are real-time feature requirements identified, and is the appropriate technology selected (long polling, WebSockets, or SSE)?
- [ ] [ARCH-CHECK-039] Is database connection pooling configured for all database connections?
- [ ] [ARCH-CHECK-040] Is auditing defined for all entities, including at minimum created/updated timestamps and user, with more advanced change history where required?
- [ ] [ARCH-CHECK-041] Is API versioning strategy defined (URL prefix `/v1/`, event version attribute) and applied consistently?
- [ ] [ARCH-CHECK-042] Is contract testing in place (e.g. Pact, Postman) to verify API implementations against defined contracts and prevent breaking changes?
- [ ] [ARCH-CHECK-043] Is pagination applied to all search endpoints and endpoints with unbounded responses, using offset-based pagination?
- [ ] [ARCH-CHECK-044] Is event versioning strategy defined to maintain backward compatibility, with a process for introducing breaking changes?
- [ ] [ARCH-CHECK-045] Are events persisted in an event store or database for auditing and replay purposes?
- [ ] [ARCH-CHECK-046] Are event handlers designed to be idempotent, with retry logic defined for transient failures?
- [ ] [ARCH-CHECK-047] Is the event processing scaling strategy defined, including consumer groups, partitioning, expected load, and known bottlenecks?
- [ ] [ARCH-CHECK-048] Are workflows that rely on eventual consistency identified, and are critical flows requiring strong consistency guaranteed via synchronous communication?
- [ ] [ARCH-CHECK-049] Are tests run on all branches in all environments, with a staging environment that mirrors production for pre-release testing?
- [ ] [ARCH-CHECK-050] Is the environment topology defined with at least four environments (local, dev, staging, prod)?
- [ ] [ARCH-CHECK-051] Is the secrets management strategy defined, using `.env` files for local development and a secrets manager (Vault or cloud-native) for production?
- [ ] [ARCH-CHECK-052] Is rolling update strategy used for deployments when sufficient resources are available?
- [ ] [ARCH-CHECK-053] Do all services expose standard `/health` (liveness) and `/ready` (readiness) endpoints?
- [ ] [ARCH-CHECK-054] Is the auto-scaling policy defined, using HTTP scalers for API services and event-based scalers for event-processing services, with defined thresholds?
- [ ] [ARCH-CHECK-055] Is data privacy for AI defined, ensuring that PII and sensitive data are anonymised or redacted before being sent to AI providers?
- [ ] [ARCH-CHECK-056] Is TLS enforced everywhere, with a certificate management strategy defined (e.g. Let's Encrypt, provider-managed TLS)?
- [ ] [ARCH-CHECK-057] Is input validation enforced at the API boundary, with an explicit decision on whether it is also enforced at the domain layer?
- [ ] [ARCH-CHECK-058] Are ORM parameterised queries used throughout to prevent SQL injection, and are raw SQL queries audited?
- [ ] [ARCH-CHECK-059] Is a restrictive CORS policy defined with explicit allowed origins, and is wildcard `*` forbidden in production?
- [ ] [ARCH-CHECK-060] Is automated CVE dependency scanning configured in CI (e.g. Dependabot, Snyk, Trivy)?
- [ ] [ARCH-CHECK-061] Are stateful or thread-unsafe components identified, and is stateless design applied elsewhere to enable horizontal scaling?
- [ ] [ARCH-CHECK-062] Are retry and circuit-breaker patterns implemented for all inter-service API calls?
- [ ] [ARCH-CHECK-063] Is a disaster recovery strategy defined, including database backup and recovery procedures using cloud-managed backup solutions where available?
- [ ] [ARCH-CHECK-064] Are load and stress testing requirements defined, including target RPS/TPS and acceptable latency thresholds?
- [ ] [ARCH-CHECK-065] Is the mobile application requirement defined — whether native (iOS/Android), cross-platform (Flutter, React Native), or mobile web — and does the architecture explicitly support the chosen approach?
