# Product Owner Guidelines & Constraints

This document captures product owner input to be used when generating the project constitution via `speckit.constitution`.
Answer or refine each section before running the constitution agent.

---

## 1. Product Vision

- **One-liner**: What is this website? Who is it for? What problem does it solve?
  > _e.g. "A developer productivity tool that helps teams track specification changes across sprints."_
- **Success definition**: What does "done" look like from a business perspective?
- **Out of scope (explicitly)**: What will this product never do?

---

## 2. Target Audience

- Who are the primary users? (role, technical level, geography)
- Are there secondary user groups (admins, guests, API consumers)?
- Does the product need to support multiple languages or locales?

---

## 3. Business Constraints

- **Deadline / milestone**: Is there a hard delivery date or external event driving the timeline?
- **Budget**: Are there hosting, licensing, or third-party service cost ceilings?
- **Compliance**: Are there legal, regulatory, or data-privacy requirements (GDPR, HIPAA, etc.)?
- **Branding**: Must the product conform to an existing brand guide or design system?

---

## 4. Feature Priorities (MVP vs. Later)

List features as **Must Have**, **Should Have**, or **Won't Have (now)**:

| Feature | Priority | Notes |
|---------|----------|-------|
| User authentication | Must Have | |
| Public-facing landing page | Must Have | |
| Admin dashboard | Should Have | |
| API documentation page | Should Have | |
| Multi-tenant support | Won't Have | Post-MVP |

> Replace the rows above with your actual features.

---

## 5. Non-Functional Requirements

- **Performance**: Maximum acceptable page load time? API response SLA?
- **Availability**: Required uptime (e.g. 99.9%)? Maintenance windows acceptable?
- **Scalability**: Expected concurrent users at launch vs. 12-month horizon?
- **Security**: Authentication method (OAuth2, SSO, username/password)? Role-based access?
- **Accessibility**: WCAG 2.1 AA compliance required?
- **SEO**: Is search engine indexability important?

---

## 6. Technology Constraints

- **Backend**: Python is confirmed. Any framework preference (FastAPI, Django, Flask)?
- **Frontend**: React is confirmed. Any UI library preference (MUI, Tailwind, Chakra)?
- **Database**: Relational (PostgreSQL/MySQL) or NoSQL? Managed service or self-hosted?
- **Hosting / deployment**: Cloud provider preference? Containerised (Docker/K8s) or PaaS?
- **CI/CD**: GitHub Actions, GitLab CI, or other?
- **Existing integrations**: Any third-party services that must be connected (Stripe, SendGrid, etc.)?

---

## 7. Content & Data

- Who owns and maintains the website content after launch?
- Is there a CMS requirement, or will content be code-managed?
- Are there initial data-seeding requirements (demo data, reference data)?
- What is the data-retention policy?

---

## 8. Quality Gates

- **Testing**: Minimum code coverage threshold?
- **Code review**: PR approval count, required reviewers?
- **Definition of Done**: What criteria must every feature meet before it is considered complete?
- **Release cadence**: Continuous deployment, weekly releases, or milestone-based?

---

## 9. Risks & Assumptions

List known risks and the assumptions being made:

| Risk / Assumption | Likelihood | Impact | Mitigation |
|-------------------|-----------|--------|------------|
| Third-party API may change | Medium | High | Version-lock + adapter pattern |
| Design assets not ready at kickoff | High | Medium | Use placeholder design system |

> Replace with your actual risks and assumptions.

---

## 10. Open Questions

Questions that must be answered before the constitution can be finalised:

- [ ] What is the project name?
- [ ] Who has final sign-off authority on features?
- [ ] Is user-generated content (UGC) in scope? If so, what moderation approach?
- [ ] Are analytics / telemetry required? Which platform?
- [ ] Is offline / PWA support required?
- [ ] What is the expected lifespan of this product (prototype, 1-year, long-term)?
