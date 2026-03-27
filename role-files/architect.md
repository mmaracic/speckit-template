# Architect Guidelines & Constraints

First section of this document captures architect constitution principles to be used when generating the project constitution via `speckit.constitution`. Second section defines the plan gate checklist.
---

## 1. Mandatory Constitution principles
### 1. System Architecture Style

- **Repository structure**: Recommend Monorepo when a high degree of coupling between components and a single team works on them. Recommend Polyrepo when components are expected to be independently deployable or reusable across projects or multiple teams work on them.
- **Overall style**: Always use modular style. Recommend Modular Monolith for simplicity and ease of development when the system is not expected to grow beyond a certain complexity. Recommend Microservices for large, complex systems with clear bounded contexts and the need for independent scalability or deployment. Recommend service based architecture as a middle ground when some modularity, scalability and independent deployment is desired but speed is a priority too.
- **API style**: REST is a good default for most applications due to its simplicity and wide support. GraphQL is beneficial when clients need flexible queries and to minimize over-fetching. gRPC is ideal for high-performance, low-latency communication between internal services. Hybrid approaches can be used when different API styles are needed for different use cases (e.g., REST for public API, gRPC for internal services).
- **Frontend/backend coupling**: Fully decoupled architectures (separate deployments) are recommended for larger applications with distinct frontend and backend teams, or when the frontend needs to be reusable across multiple platforms (web, mobile). Server-rendered (SSR/hybrid) architectures can be beneficial for smaller applications or when SEO is a concern, but may introduce tighter coupling between frontend and backend.
- **Background processing**: Use background processing for long-running or resource-intensive tasks that do not need to be completed within the request-response cycle. Choose a task queue (e.g., Celery, ARQ) based on language and ecosystem compatibility, ease of use, and scalability requirements.

---

### 2. Module Boundaries
- **Module granularity**: Define clear boundaries for each module based on business capabilities or domain concepts. Each module should have a single responsibility and encapsulate its internal implementation details.
- **Interface / contract format**: Define API contracts using OpenAPI for REST, GraphQL SDL for GraphQL, or protobuf for gRPC. Contracts should be the source of truth for API design and implementation. Use AVRO or JSON Schema for event based communication contracts. Avoid code-first approaches that can lead to drift between implementation and design.
- **Architecture as code**: Define layer, module and application interaction constraints using a formal architecture-as-code approach
- **Communication pattern**: Synchronous HTTP is simpler to implement and reason about
and avoids eventual consistency issues, making it a good default for simple applications. Asynchronous HTTP can be used for non-blocking operations. Event-driven architectures (message queue / pub-sub) can provide better scalability, reliability and decoupling and is recommended servive based or microservices architectures.
- **Event technology**: If event-driven, use Kafka for scalability, reliability and lack of back-pressure. 
- **Shared libraries**: Use shared libraries for common utilities, domain models, and API contracts to avoid duplication and ensure consistency across services especially when services represent similar use cases. However, be cautious of tight coupling and versioning issues that can arise from shared libraries.
- **Third-party integrations**: Prefer to use API based integrations based on REST or queue based integrations using Kafka. Avoid tight coupling with third-party SDKs or libraries that can create maintenance issues.
- **Real-time features**: First choice should be long pooling for simplicity, then WebSockets for true real-time bidirectional communication, and finally server-sent events (SSE) for unidirectional real-time updates from server to client.
---

### 3. Data Layer

- **SQL vs NoSQL**: Use open source relational databases (PostgreSQL, MySQL) where reliability and strong consistency guarantees are required. Use NoSQL databases (MongoDB, DynamoDB) when flexible schemas, horizontal scalability or high write throughput are needed.
- **Database connection pooling**: Use connection pooling
- **Caching layer**: Add a caching layer (Redis, Memcached) for frequently accessed data or expensive computations when speed is top priority. Use cache-aside pattern for simplicity and control over cache invalidation.
- **File/blob storage**: Use cloud blob storage (Azure Blob Storage, AWS S3) for large files, unstructured data or binary data that doesnt need elaborate querying capabilities. Use database BLOBs for json when transactional consistency with other data or advanced querying capabilities are required.
- **Audit**: Add auditing in any enterprise application. Minimum audit includes create and update date and user. More advanced versioned change history can be added if required by the domain.

---

### 4. Authentication & Authorisation

- **Auth mechanism**: Use JWT + Outh2 for user facing UI and APIs. Use API keys for service-to-service authentication. Avoid rolling your own auth mechanism or using basic auth for production applications.
- **Identity provider**: Prefer cloud based identity providers (Azure AD, AWS Cognito) for ease of integration and management. Use open source identity providers (Keycloak, Ory) when customisation or on-premises hosting is required.
- **Authorisation model**: Use open source implementations of common authorisation models (RBAC, ABAC) to avoid reinventing the wheel.
---

### 5. API Contract & Versioning

- **API versioning strategy**: URL prefix (`/v1/`), version attribute in events.
- **Schema-first**: OpenAPI spec generated from code
- **Contract testing**: Use contract testing tools (Pact, Postman) to ensure API implementations meet the defined contracts and to prevent breaking changes.
- **Pagination strategy**: Use pagination on all search endpoints and all endpoints whose response can grow unbounded. Use offset-based pagination for simplicity.
---

### 6. Events and Messaging

- **Event-driven architecture**: Use events to decouple services and enable asynchronous communication. Define clear event schemas and topics.
- **Message broker**: Use a message broker (Kafka, RabbitMQ, Azure Service Bus) for event distribution and processing.
- **Event versioning**: Maintain backward compatibility for events and use versioning when breaking changes are necessary.
- **Event storage**: Persist events in an event store or database for auditing and replay purposes.
- **Event processing**: Use idempotent event handlers and implement retry logic to handle transient failures in event processing.
- **Scaling event processing**: Use consumer groups and partitioning in Kafka to scale event processing horizontally. Describe the expected load and scaling strategy for event processing in the architecture design. Describe scaling limitations and bottlenecks of the chosen event processing approach and how they will be mitigated.
- **Eventual consistency**: If using an event-driven architecture, eventual consistency has to be detected and pointed out in the design. Event-driven communication has to be avoided for critical user flows and operations that require strong consistency guarantees.
- **Event structure**: Define a standard structure for events, including metadata (timestamp, event type, source) and payload. Use a consistent format (e.g., JSON, Avro) for event payloads. Generate the structure in a machine-readable format (e.g., JSON Schema, Avro schema) and store it in the `events.md` file to ensure consistency and ease of use across the system.

### 7. Infrastructure & Deployment

- **Containerisation**: Use Docker when the application needs to be always online, has complex dependencies or needs to run in different environments with minimal changes. Use serverless (Azure Functions, AWS Lambda) for simple, event-driven applications with low to moderate traffic and where low cost is a priority.
- **Cloud provider**: Separate cloud specific code from business logic to different modules to allow for easier migration if needed. Use cloud provider managed services (Azure App Service, AWS Elastic Beanstalk) for ease of deployment and management when using containerisation. Use serverless offerings (Azure Functions, AWS Lambda) when using serverless architecture.
- **Deployment strategy**: Use automated deployment pipelines (GitHub Actions, Azure DevOps) to ensure consistent and reliable deployments.
- **Testing** Run tests on all branches in all environments. Use staging environment that mirrors production for final testing before release.
- **Environment topology**: Use at least four environments (local, dev, staging, prod).
- **Secrets management**: `.env` files for local development, Vault, cloud-native secrets manager for production.
- **Deployment strategy**: Use rolling updates when enough resources are available.
- **Health check endpoints**: Standard `/health` (liveness) and `/ready` (readiness) endpoints required.
- **Auto-scaling policy**: Use Http scalers for services that have APIs or event-based scalers for services that process events. Define clear scaling thresholds based on performance goals and expected traffic patterns.

---

### 8. Observability

- **Logging**: Structured JSON logs. Use cloud-native logging services (Azure Monitor, AWS CloudWatch) or open source solutions (ELK stack) for log aggregation and analysis.
- **Metrics**: Prometheus + Grafana, or cloud-native monitoring.
- **Tracing**: Use OpenTelemetry for distributed tracing and integrate with a tracing backend (Jaeger, Zipkin, or cloud-native tracing services).

### 9. AI
- **Provider independence**: Design AI integration in a provider-agnostic way using OpenLLM.
- **Tracing and monitoring**: Ensure AI interactions are logged and monitored for performance and cost management using.
- **Minimal model usage**: Use AI for tasks that provide clear value and cannot be easily solved with traditional programming or existing data science algorithms to manage costs and complexity.
- **Evaluation and testing**: Implement robust evaluation and testing strategies for AI outputs, including unit tests for deterministic outputs and human-in-the-loop review for non-deterministic outputs.
- **Human in the loop**: Ensure there is a mechanism for human review in all workflows that use AI and override to prevent harmful or incorrect AI outputs from causing issues in production.
- **Data privacy**: Ensure that any data sent to AI providers is anonymized and does not contain personally identifiable information (PII) or sensitive data, in compliance with data privacy regulations.
---

### 10. Security Architecture

- **Transport**: Use TLS everywhere. Use Let's Encrypt if no provider-managed TLS is available.
- **Input validation**: Where is validation enforced — API boundary, domain layer, or both?
- **Query parameter sanitisation**: Use ORM parameterised queries to prevent SQL injection; raw SQL queries must be audited.
- **CORS policy**: Use restrictive CORS policy in all environments, with explicit allowed origins. Avoid wildcard `*` in production.
- **Dependency scanning**: Automated CVE scanning in CI.
---

### 11. Scalability & Resilience

- **Horizontal scaling**: Define stateful or thread unsafe applications that cannot be horizontally scaled without additional work. Use stateless design principles to enable horizontal scaling where possible.
- **Retry & circuit-breaker patterns**: Required for all APIs
- **Disaster recovery**: Define backup and recovery strategies for databases and critical data. Use cloud provider managed backup solutions when available.
- **Load testing**: Define load and stress testing requirements, use cases and procedures, including target RPS/TPS and acceptable latency thresholds.
---

### 12. Architecture Decision Records (ADRs)

- Maintain ADRs in the `adrs/` directory to document all significant architectural decisions, including the context, decision, alternatives considered, and rationale.
- ADRs must be created for decisions such as choice of architecture style, API design conventions, data storage solutions, authentication mechanisms, and deployment strategies.
- ADRs must be reviewed and updated as the architecture evolves to ensure they remain relevant and accurate.
---

### 13. Architecture Diagrams
- It is mandatory to create architecture diagrams for the system. These diagrams should include:
  - High-level system architecture diagram showing major components and their interactions including applications, databases and event queues.
  - Deployment architecture diagram showing how the application is deployed in Azure environment.
- Diagrams must be created using a mermaid format and stored into separate files into the `diagrams/` folder. They must be included in the feature artifacts and updated as needed when the architecture evolves.
- Diagrams must be kept up to date and reflect the current state of the architecture. They must be reviewed and updated as part of the architectural decision process and during implementation when changes are made.
- Diagrams must be clear, concise and easy to understand, providing a visual representation of the architecture that complements the written documentation and ADRs.
- Diagrams must minimise the number of intersections and overlapping lines and objects to ensure readability. They should use consistent symbols and notation to represent different types of components and interactions.

## 2. Plan Gate Checklist
### 1. Open Questions
- [ ] What is the chosen architecture style (monolith, microservices, service-based) and why?
- [ ] What architectural characteristics and constraints are most relevant for each application and service (e.g., modularity, scalability, deployment independence, reliability, high availability)?
- [ ] What are the applications and services that will make up the system?
- [ ] Did we define all communication patterns and technologies between applications and services (e.g., REST, gRPC, message queues)?
- [ ] Did we define the data storage technologies and strategies (e.g., SQL vs NoSQL, caching, file storage)?
- [ ] What API style(s) will be used (REST, GraphQL, gRPC) and why?
- [ ] What is the repository structure (monorepo vs polyrepo) and why?
- [ ] What is the expected scale of the application in terms of users, transactions, and data volume?
- [ ] Are there any specific regulatory or compliance requirements that need to be considered in the architecture?
- [ ] What are the expected performance and latency requirements for the application?
- [ ] Are there any existing systems or technologies that need to be integrated with, and what are their constraints?
- [ ] Are there any specific security requirements or threat models that need to be addressed in the architecture?
- [ ] What authentication and authorization mechanisms will be used?
- [ ] What is the cloud provider and deployment strategy for each application and service?
- [ ] Is there an architecture diagram for cloud provider?
- [ ] What is the expected timeline for development and how does it influence the choice of architecture style and technologies?
- [ ] Are there any libraries or frameworks that are preferred or mandated for use in the project?
- [ ] Where will the application be deployed (e.g., Azure, AWS, on-premises) and how does that influence architectural decisions?
- [ ] Are all diagrams included in the plan artifacts and do they reflect the current architectural decisions?
- [ ] Are all diagrams valid mermaid syntax and render correctly?
- [ ] Is the structure of all events described in `events.md` file and do they reflect the current design of the system?
- [ ] Is the structure of all API contracts described in `contracts/` folder and do they reflect the current design of the system?
- [ ] Are all database schemas described in `data-model.md` folder and do they reflect the current design of the system?
- [ ] Are all ADRs included in the plan artifacts and do they reflect the current architectural decisions?
