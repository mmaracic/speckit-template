 # Product Owner Guidelines & Constraints

First section of this document captures product owner constitution principles to be used when generating the project constitution via `speckit.constitution`. Second section defines the gate checklists. Here there is only one checklist and it applies to the specify speckit step.
Every mandatory bullet with an `PO-` or `PO-CHECK-` ID must be copied into the generated constitution and downstream plan checks with one-to-one traceability. Mandatory bullets must not be skipped, merged away, or summarized.
---

## 0. Copy and Traceability Principles

- **MANDATORY:** [PO-COPY-001] Every mandatory source item in this file must be copied into generated constitution artifacts.
- **MANDATORY:** [PO-COPY-002] Mandatory source items must not be skipped, merged, or summarized into generic wording.
- **MANDATORY:** [PO-COPY-003] If a generated constitution needs more sections or principles to preserve fidelity, add them instead of collapsing items.
- **MANDATORY:** [PO-COPY-004] Generated constitution artifacts must include a traceability matrix that maps each `PO-` and `PO-CHECK-` ID.
- **MANDATORY:** [PO-COPY-005] Each source ID must map to exactly one explicit constitution clause or one explicit plan-check entry, unless a justified exception is documented.

---

## 1. Mandatory Constitution principles
### 1. Product Vision

- **MANDATORY:** [PO-001] **One-liner**: Describe what is this product? Who is it for? What problem does it solve?
  > _e.g. "A developer productivity tool that helps teams track specification changes across sprints."_
- **MANDATORY:** [PO-002] **Success definition**: Define what "done" looks like from a business perspective.
  > _e.g. "Success is defined as achieving 10,000 active users within the first 6 months, with a user satisfaction score of at least 4.5 out of 5."_
- **MANDATORY:** [PO-003] **Out of scope (explicitly)**: Define what this product will never do.
  > _e.g. "This product will never include a built-in code editor or real-time collaboration features."_

---

### 2. Target Audience

- **MANDATORY:** [PO-004] **Primary users**: Define who are the primary users? (role, technical level, geography)
- **MANDATORY:** [PO-005] **Secondary user groups**: Define if there are secondary user groups (admins, guests, API consumers)?
- **MANDATORY:** [PO-006] **Language / locale support**: Define if the product needs to support multiple languages or locales?

---

### 3. Business Constraints

- **MANDATORY:** [PO-007] **Deadline / milestone**: Define if there is a hard delivery date or external event driving the timeline.
- **MANDATORY:** [PO-008] **Budget**: Define if there are hosting, licensing, or third-party service cost ceilings.
- **MANDATORY:** [PO-009] **Compliance**: Define if there are legal, regulatory, or data-privacy requirements (GDPR, HIPAA, etc.)?
- **MANDATORY:** [PO-010] **Branding**: Define if the product must conform to an existing brand guide or design system?

---

### 4. Features
- **MANDATORY:** [PO-011] **Ambiguity**: Features must be unambiguously defined and scoped to avoid misinterpretation during implementation.
- **MANDATORY:** [PO-012] **Actor-Action-Outcome format**: Each feature must be described in terms of who (actor) needs to do what (action), why (context, condition) and what the expected outcome is.
    > _e.g. "As a registered user (actor), I want to receive an email notification (action) when a new comment is added to my post (context, condition), so that I can respond to it (outcome)."_
- **MANDATORY:** [PO-013] **Workflows**: When features include multiple actions or outcomes and have multiple steps or states, we must define a clear workflow with all the steps, states, and transitions. Each step has to have clear inputs, outputs, and stop conditions.
    > _e.g. "When a user submits a support ticket, the workflow is: 1) Ticket is created in 'Open' state; 2) Support team reviews and changes state to 'In Progress'; 3) Once resolved, state changes to 'Closed' and user receives notification."_
- **MANDATORY:** [PO-014] **Error handling**: For each feature, we must define how errors and edge cases are handled to ensure a robust user experience.
    > _e.g. "If the email service is down when trying to send a notification, the system should retry up to 3 times with exponential backoff, and log the failure if all attempts fail."_
- **MANDATORY:** [PO-015] **Prioritization**: Features must be prioritized based on business value, user impact, and implementation complexity to guide development efforts effectively.
    > _e.g. "User authentication is a Must Have feature for MVP, while multi-tenant support is a Should Have for post-MVP."_
---

### 5. Non-Functional Requirements

- **MANDATORY:** [PO-016] **Performance**: Define the maximum acceptable page load time and API response SLA.
- **MANDATORY:** [PO-017] **Availability**: Define the required uptime (e.g. 99.9%) and acceptable maintenance windows.
- **MANDATORY:** [PO-018] **Scalability**: Define the expected concurrent users at launch and the 12-month horizon.
- **MANDATORY:** [PO-019] **Security**: Define the authentication method (OAuth2, SSO, username/password) and role-based access requirements.

---

### 6. Technology Constraints

- **MANDATORY:** [PO-020] **Browser support**: Define the minimum browser versions to support.
- **MANDATORY:** [PO-021] **Device support**: Define the mobile, tablet, and desktop requirements.
- **MANDATORY:** [PO-022] **Existing integrations**: Define any third-party services that must be connected (Stripe, SendGrid, etc.).

---

### 7. Content & Data

- **MANDATORY:** [PO-023] **Content ownership**: Define who owns and maintains the website content after launch.
- **MANDATORY:** [PO-024] **CMS requirement**: Define if there is a CMS requirement, or if content will be code-managed.
- **MANDATORY:** [PO-025] **Data seeding**: Define if there are initial data-seeding requirements (demo data, reference data).
- **MANDATORY:** [PO-026] **Data retention**: Define the data-retention policy.

---

### 8. Quality Gates

- **MANDATORY:** [PO-027] **Testing**: Define the minimum code coverage threshold.
- **MANDATORY:** [PO-028] **Code review**: Define the PR approval count and required reviewers.
- **MANDATORY:** [PO-029] **Definition of Done**: Define the criteria that must be met for a feature to be considered complete.
- **MANDATORY:** [PO-030] **Release cadence**: Define the release cadence (continuous deployment, weekly releases, or milestone-based).

---

### 9. Risks & Assumptions

- **MANDATORY:** [PO-031] **Risks and assumptions**: List known risks and the assumptions being made.


## 2. Gate Checklists
### 1. Specify Gate Checklist

- [ ] [PO-CHECK-001] Is a one-liner provided that describes what the product is, who it is for, and what problem it solves?
- [ ] [PO-CHECK-002] Is a success definition provided that describes what "done" looks like from a business perspective?
- [ ] [PO-CHECK-003] Is an explicit out-of-scope statement provided that defines what this product will never do?
- [ ] [PO-CHECK-004] Are the primary users defined, including their role, technical level, and geography?
- [ ] [PO-CHECK-005] Are secondary user groups (admins, guests, API consumers) identified or explicitly ruled out?
- [ ] [PO-CHECK-006] Is the language and locale support requirement defined?
- [ ] [PO-CHECK-007] Is a deadline or milestone defined, or is the absence of one explicitly stated?
- [ ] [PO-CHECK-008] Are budget constraints (hosting, licensing, third-party cost ceilings) defined or explicitly ruled out?
- [ ] [PO-CHECK-009] Are compliance requirements (GDPR, HIPAA, etc.) identified or explicitly ruled out?
- [ ] [PO-CHECK-010] Are branding constraints (brand guide, design system) defined or explicitly ruled out?
- [ ] [PO-CHECK-011] Are all features unambiguously defined and scoped with no room for misinterpretation?
- [ ] [PO-CHECK-012] Is each feature described using the Actor-Action-Outcome format (who, what, why, expected outcome)?
- [ ] [PO-CHECK-013] For features with multiple steps or states, is a workflow defined with all steps, states, transitions, inputs, outputs, and stop conditions?
- [ ] [PO-CHECK-014] Is error handling and edge-case behaviour defined for each feature?
- [ ] [PO-CHECK-015] Are features prioritised by business value, user impact, and implementation complexity?
- [ ] [PO-CHECK-016] Are maximum acceptable page load time and API response SLA defined?
- [ ] [PO-CHECK-017] Are required uptime and acceptable maintenance windows defined?
- [ ] [PO-CHECK-018] Are expected concurrent users at launch and at the 12-month horizon defined?
- [ ] [PO-CHECK-019] Are the authentication method and role-based access requirements defined?
- [ ] [PO-CHECK-020] Are the minimum browser versions to support defined?
- [ ] [PO-CHECK-021] Are mobile, tablet, and desktop support requirements defined?
- [ ] [PO-CHECK-022] Are all required third-party integrations (Stripe, SendGrid, etc.) identified or explicitly ruled out?
- [ ] [PO-CHECK-023] Is content ownership after launch defined?
- [ ] [PO-CHECK-024] Is the CMS requirement defined, or is it explicitly stated that content will be code-managed?
- [ ] [PO-CHECK-025] Are initial data-seeding requirements (demo data, reference data) defined or explicitly ruled out?
- [ ] [PO-CHECK-026] Is the data-retention policy defined?
- [ ] [PO-CHECK-027] Is the minimum code coverage threshold defined?
- [ ] [PO-CHECK-028] Are the PR approval count and required reviewers defined?
- [ ] [PO-CHECK-029] Is the Definition of Done defined with explicit criteria every feature must meet?
- [ ] [PO-CHECK-030] Is the release cadence (continuous deployment, weekly, or milestone-based) defined?
- [ ] [PO-CHECK-031] Are known risks and assumptions listed?
- [ ] [PO-CHECK-032] Are analytics / telemetry requirements defined? Which platform?
- [ ] [PO-CHECK-033] Is offline / PWA support required?
- [ ] [PO-CHECK-034] What is the expected lifespan of this product (prototype, 1-year, long-term)?