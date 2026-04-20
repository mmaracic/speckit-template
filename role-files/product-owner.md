 # Product Owner Guidelines & Constraints

First section of this document captures product owner constitution principles to be used when generating the project constitution via `speckit.constitution`. Second section captures project-specific overrides and additions. Third section defines the gate checklists. Here there is only one checklist and it applies to the specify speckit step.
Every mandatory bullet with an `PO-` or `PO-CHECK-` ID in this file is the source of truth and MUST be referenced by the generated constitution and downstream specification checks. Mandatory bullets must not be skipped, merged away, or summarized.
---

## 0. Reference and Source-of-Truth Principles

- **MANDATORY:** [PO-REF-001] This role file is the authoritative source for product-owner governance requirements.
- **MANDATORY:** [PO-REF-002] Generated constitution artifacts MUST reference this role file path and revision metadata instead of copying all mandatory bullets.
- **MANDATORY:** [PO-REF-003] Mandatory source items must not be skipped, merged, or summarized into generic wording when generating specification checks.
- **MANDATORY:** [PO-REF-004] Specification artifacts MUST explicitly cover each `PO-CHECK-` item with evidence, sourced from this role file.
- **MANDATORY:** [PO-REF-005] Any justified exception to a mandatory source item MUST be explicitly documented with rationale and approval.

---

## 1. Mandatory Constitution principles
### 1. Product Vision

- **MANDATORY:** [PO-01-01] **One-liner**: Must describe what the product is, who it is for, and what problem it solves.
  > _e.g. "A developer productivity tool that helps teams track specification changes across sprints."_
- **MANDATORY:** [PO-01-02] **Success definition**: Define what "done" looks like from a business perspective.
  > _e.g. "Success is defined as achieving 10,000 active users within the first 6 months, with a user satisfaction score of at least 4.5 out of 5."_
- **MANDATORY:** [PO-01-03] **Out of scope (explicitly)**: Define what this product will never do.
  > _e.g. "This product will never include a built-in code editor or real-time collaboration features."_

---

### 2. Target Audience

- **MANDATORY:** [PO-02-01] **Primary users**: Define who the primary users are (role, technical level, geography).
- **MANDATORY:** [PO-02-02] **Secondary user groups**: Define whether there are secondary user groups (admins, guests, API consumers), or explicitly state there are none.
- **MANDATORY:** [PO-02-03] **Language / locale support**: Define whether the product must support multiple languages or locales, or explicitly state that only a single locale is required.

---

### 3. Business Constraints

- **MANDATORY:** [PO-03-01] **Deadline / milestone**: Define if there is a hard delivery date or external event driving the timeline.
- **MANDATORY:** [PO-03-02] **Budget**: Define if there are hosting, licensing, or third-party service cost ceilings.
- **MANDATORY:** [PO-03-03] **Compliance**: Define whether there are legal, regulatory, or data-privacy requirements (GDPR, HIPAA, etc.), or explicitly state there are none.
- **MANDATORY:** [PO-03-04] **Branding**: Define whether the product must conform to an existing brand guide or design system, or explicitly state there are no branding constraints.

---

### 4. Feature definition & decomposition
- **MANDATORY:** [PO-04-01] **Ambiguity**: Features must be unambiguously defined and scoped to avoid misinterpretation during implementation.
- **MANDATORY:** [PO-04-02] **Actor-Action-Outcome format**: Each feature must be described in terms of who (actor) needs to do what (action), why (context, condition) and what the expected outcome is.
    > _e.g. "As a registered user (actor), I want to receive an email notification (action) when a new comment is added to my post (context, condition), so that I can respond to it (outcome)."_
- **MANDATORY:** [PO-04-03] **Workflows**: When features include multiple actions or outcomes and have multiple steps or states, we must define a clear workflow with all the steps, states, and transitions. Each workflow must include trigger, preconditions, step inputs and outputs, stop conditions, postconditions, failure paths, and data touched.
    > _e.g. "When a user submits a support ticket, the workflow is: 1) Ticket is created in 'Open' state; 2) Support team reviews and changes state to 'In Progress'; 3) Once resolved, state changes to 'Closed' and user receives notification."_
- **MANDATORY:** [PO-04-04] **Error handling**: For each feature, we must define how errors and edge cases are handled to ensure a robust user experience.
    > _e.g. "If the email service is down when trying to send a notification, the system should retry up to 3 times with exponential backoff, and log the failure if all attempts fail."_
- **MANDATORY:** [PO-04-05] **Prioritization**: Features must be prioritized based on business value, user impact, and implementation complexity to guide development efforts effectively.
    > _e.g. "User authentication is a Must Have feature for MVP, while multi-tenant support is a Should Have for post-MVP."_
- **MANDATORY:** [PO-04-06] **Feature to story mapping**: Features must be decomposed into user stories in such way that each story contains only one actor and only one workflow, to ensure clear ownership and avoid acceptance criteria ambiguity.
    > _e.g. "The feature 'User Management' can be decomposed into stories like: 'As an admin, I want to create a new user, so that I can manage access to the system.'_
- **MANDATORY:** [PO-04-07] **Acceptance criteria**: Each user story must have clear and unambiguous acceptance criteria that define the conditions under which the story is considered complete.
    > _e.g. "Given a registered user, when they attempt to log in with valid credentials, then they should be granted access to their dashboard."_
- **MANDATORY:** [PO-04-08] **Documentation**: All user stories must list the document in the documentation folder that was used to define the feature.
---

### 5. Non-Functional Requirements

- **MANDATORY:** [PO-05-01] **Performance**: Define the maximum acceptable page load time and API response SLA.
- **MANDATORY:** [PO-05-02] **Availability**: Define the required uptime (e.g. 99.9%) and acceptable maintenance windows.
- **MANDATORY:** [PO-05-03] **Scalability**: Define the expected concurrent users at launch and the 12-month horizon.
- **MANDATORY:** [PO-05-04] **Security**: Define the authentication method (OAuth2, SSO, username/password) and role-based access requirements.

---

### 6. Technology Constraints

- **MANDATORY:** [PO-06-01] **Browser support**: Define the minimum browser versions to support.
- **MANDATORY:** [PO-06-02] **Device support**: Define the mobile, tablet, and desktop requirements.
- **MANDATORY:** [PO-06-03] **Existing integrations**: Define any third-party services that must be connected (Stripe, SendGrid, etc.).
- **MANDATORY:** [PO-06-04] **Application types**: Define the types of applications to be supported (web, mobile, desktop, etc.).
- **MANDATORY:** [PO-06-05] **Application platforms**: Define the platforms on which the applications must run (iOS, Android, Windows, macOS, Linux, etc.).
- **MANDATORY:** [PO-06-06] **Application technologies**: Define any required technologies or frameworks (React, Flutter, Electron, etc.) or explicitly state that there are no technology constraints.
- **MANDATORY:** [PO-06-07] **Offline support**: Define whether offline functionality or PWA support is required or explicitly state that it is not required.
- **MANDATORY:** [PO-06-08] **Analytics**: Define whether analytics or telemetry requirements exist, and if so, which platform to use (Google Analytics, Mixpanel, etc.) or explicitly state that there are no analytics requirements.

---

### 7. Content & Data

- **MANDATORY:** [PO-07-01] **Content ownership**: Define who owns and maintains the website content after launch.
- **MANDATORY:** [PO-07-02] **CMS requirement**: Define if there is a CMS requirement, or if content will be code-managed.
- **MANDATORY:** [PO-07-03] **Data seeding**: Define if there are initial data-seeding requirements (demo data, reference data).
- **MANDATORY:** [PO-07-04] **Data retention**: Define the data-retention policy.

---

### 8. Quality Gates

- **MANDATORY:** [PO-08-01] **Testing**: Define the minimum code coverage threshold.
- **MANDATORY:** [PO-08-02] **Code review**: Define the PR approval count and required reviewers.
- **MANDATORY:** [PO-08-03] **Definition of Done**: Define the criteria that must be met for a feature to be considered complete.
- **MANDATORY:** [PO-08-04] **Release cadence**: Define the release cadence (continuous deployment, weekly releases, or milestone-based).

---

### 9. Risks & Assumptions

- **MANDATORY:** [PO-09-01] **Risks and assumptions**: List known risks and the assumptions being made.

---

## 2. Project-Specific Principles

_Add project-specific overrides or additions to the principles in Section 1. Use the same MANDATORY bullet format with subsection-scoped `[PO-SS-II]` IDs. Leave this section empty if there are no project-specific constraints._

---

## 3. Gate Checklists
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
- [ ] [PO-CHECK-013] For features with multiple steps or states, is a workflow defined with trigger, preconditions, all steps, state transitions, step inputs/outputs, stop conditions, postconditions, failure paths, and data touched?
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
- [ ] [PO-CHECK-035] Are features decomposed into user stories where each story contains only one actor and one workflow, ensuring clear ownership and unambiguous acceptance criteria?
- [ ] [PO-CHECK-036] Are the types of applications to be developed (web, mobile, desktop, etc.) defined?
- [ ] [PO-CHECK-037] Are the platforms on which the applications must run (iOS, Android, Windows, macOS, Linux, etc.) defined?
- [ ] [PO-CHECK-038] Are required technologies or frameworks (React, Flutter, Electron, etc.) defined, or is it explicitly stated that there are no technology constraints?
- [ ] [PO-CHECK-039] Does each user story have clear and unambiguous acceptance criteria that define the conditions under which the story is considered complete?
- [ ] [PO-CHECK-040] Are all stories traceable back to features requested by the documentation in the document folder?
- [ ] [PO-CHECK-041] Are all `FR` requirements traceable back to user requirements or the constitution principles? if not, clarify with the user. Do not invent requirements that are not supported by the constitution or user requests.
- [ ] [PO-CHECK-042] Every assumption should be checked against the user requirements and constitution principles to ensure it is valid. If any assumption is not supported by the constitution or user requirements, clarify with the user and do not invent assumptions that are not supported.
