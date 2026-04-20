# Frontend Developer React Guidelines & Constraints

First section of this document captures react developer constitution principles to be used when generating the project constitution via `speckit.constitution`. Second section captures project-specific overrides and additions. Third section defines the gate checklists. Here there is only one checklist and it applies to the task and implementation speckit steps.
Every mandatory bullet with an `FE-` or `FE-CHECK-` ID in this file is the source of truth and MUST be referenced by the generated constitution and downstream implementation checks. Mandatory bullets must not be skipped, merged away, or summarized.
---

## 0. Reference and Source-of-Truth Principles

- **MANDATORY:** [FE-REF-001] This role file is the authoritative source for frontend-react governance requirements.
- **MANDATORY:** [FE-REF-002] Generated constitution artifacts MUST reference this role file path and revision metadata instead of copying all mandatory bullets.
- **MANDATORY:** [FE-REF-003] Mandatory source items must not be skipped, merged, or summarized into generic wording when generating implementation checks.
- **MANDATORY:** [FE-REF-004] Implementation artifacts MUST explicitly cover each `FE-CHECK-` item with evidence, sourced from this role file.
- **MANDATORY:** [FE-REF-005] Any justified exception to a mandatory source item MUST be explicitly documented with rationale and approval.

---

## 1. Mandatory Constitution principles
### 1. React Setup & Tooling

- **MANDATORY:** [FE-01-01] **Build tool**: Mandatory use of Vite
- **MANDATORY:** [FE-01-02] **Language**: Mandatory use of TypeScript
- **MANDATORY:** [FE-01-03] **Package manager**: Use npm
- **MANDATORY:** [FE-01-04] **Monorepo**: Frontend is a separate folder from backend
- **MANDATORY:** [FE-01-05] **Node version**: Minimum node version 22
- **MANDATORY:** [FE-01-06] **Dependency versions**: Use exact pinned versions for all dependencies in `package.json` file, with a shared lockfile committed to the repo.

---

### 2. UI Framework & Design Systems

- **MANDATORY:** [FE-02-01] **Component library**: Axios for API calls, Shadcn/ui for components
- **MANDATORY:** [FE-02-02] **CSS approach**: Use styled-components and Tailwind CSS

---

### 3. State Management

- **MANDATORY:** [FE-03-01] **Global state**: Use React Context for global state management

---

### 4. Routing

- **MANDATORY:** [FE-04-01] **Router library**: Use React Router for client-side routing

---

### 5. API Integration

- **MANDATORY:** [FE-05-01] **How is the API base URL configured**: Use environment variables (e.g., `VITE_API_BASE_URL`) for API base URL configuration, and ensure that these variables are properly documented and included in the project setup instructions.

---

### 6. Testing

- **MANDATORY:** [FE-06-01] **Unit / component tests**: Use Vitest + React Testing Library

---

### 7. Code Quality & Conventions

- **MANDATORY:** [FE-07-01] **Linter**: Use ESLint with standard ruleset
- **MANDATORY:** [FE-07-02] **Formatter**: Use Prettier with shared config committed to the repo
- **MANDATORY:** [FE-07-03] **Naming conventions**: Use PascalCase naming for components, camelCase for hooks, kebab-case for files
- **MANDATORY:** [FE-07-04] **Folder structure**: Co-locate components with their styles and tests in a folder
- **MANDATORY:** [FE-07-05] **Page folder structure**: Create a file for each page under `src/pages`
- **MANDATORY:** [FE-07-06] **Component folder structure**: Create a file for each reusable component under `src/components`, with the component file, styles, and tests co-located inside.
- **MANDATORY:** [FE-07-07] **Api layer**: Create a dedicated folder for API client code
- **MANDATORY:** [FE-07-08] **Hooks**: Create a dedicated folder for custom hooks
- **MANDATORY:** [FE-07-09] **Model folder**: Create a dedicated folder for shared TypeScript types and interfaces
- **MANDATORY:** [FE-07-10] **Import conventions**: Absolute imports from `src/` only,
- **MANDATORY:** [FE-07-11] **Effect hook**: Minimize the use of `useEffect` for side effects.
- **MANDATORY:** [FE-07-12] **Component types**: Use function components with React.FC or explicit prop types, avoid class components.
- **MANDATORY:** [FE-07-13] **File content**: One component per file, with the filename matching the component name. Model and API files can export multiple related types/interfaces.
- **MANDATORY:** [FE-07-14] **Refs**: Do not use refs hooks.
- **MANDATORY:** [FE-07-15] **State forwarding**: Forward state and handlers via props instead of using refs to manipulate child components. This promotes a unidirectional data flow and makes the component behavior more predictable and easier to debug.
- **MANDATORY:** [FE-07-16] **Function responsibility**: Functions must do one thing and do it well. If a function is doing more than one thing, it should be refactored into smaller functions.

---

### 8. Performance Standards

- **MANDATORY:** [FE-08-01] **Memoisation policy**: Use `React.memo` / `useMemo` / `useCallback` when necessary to prevent expensive re-renders, but avoid premature optimization.

---

### 9. Security

- **MANDATORY:** [FE-09-01] **Token storage**: Auth tokens are stored in httpOnly cookies. `localStorage` is forbidden
- **MANDATORY:** [FE-09-02] **XSS prevention**: `dangerouslySetInnerHTML` is banned. A Content Security Policy (CSP) header is enforced.
- **MANDATORY:** [FE-09-03] **CSRF protection**: State-mutating requests must be protected (SameSite cookies, CSRF token)
- **MANDATORY:** [FE-09-04] **Sensitive data in state**: Tokens, passwords, or PII must not be stored in global state or browser storage.
- **MANDATORY:** [FE-09-05] **Third-party scripts**: Third-party scripts (analytics, chat widgets) must be reviewed and sandboxed.
- **MANDATORY:** [FE-09-06] **Dependency auditing**: `npm audit` / `pnpm audit` must be run in CI with a fail threshold.
- **MANDATORY:** [FE-09-07] **Environment variables**: All `VITE_` prefixed variables must be non-sensitive by policy. (Vite inlines them into the bundle)
- **MANDATORY:** [FE-09-08] **Subresource Integrity (SRI)**: Externally loaded scripts and stylesheets must be integrity-checked.
- **MANDATORY:** [FE-09-09] **Open redirect prevention**: Internal navigation targets must be validated before redirect.

---

### 10. Task decomposition

- **MANDATORY:** [FE-10-01] Must decompose stories into tasks in such a way that each task corresponds to one container i.e. application service, database, or other infrastructure component. This principle fits different repo structures (monorepo with multiple services, or separate repos per service) and ensures that each task has a clear implementation scope and owner and that it can be tested and deployed independently.

---

## 2. Project-Specific Principles

_Add project-specific overrides or additions to the principles in Section 1. Use the same MANDATORY bullet format with subsection-scoped `[FE-SS-II]` IDs. Leave this section empty if there are no project-specific constraints._

---

## 3. Gate Checklists
### 1. Task and implementation Gate Checklist

- [ ] [FE-CHECK-001] Is Vite used as the build tool?
- [ ] [FE-CHECK-002] Is TypeScript used throughout the project with no plain JS files?
- [ ] [FE-CHECK-003] Is npm used as the package manager (no yarn or pnpm)?
- [ ] [FE-CHECK-004] Is the frontend a separate repository from the backend?
- [ ] [FE-CHECK-005] Is Node version 22 or higher specified in `.nvmrc` or `engines` in `package.json`?
- [ ] [FE-CHECK-006] Are Shadcn/ui components used for UI and Axios for API calls?
- [ ] [FE-CHECK-007] Are styled-components and Tailwind CSS used for styling?
- [ ] [FE-CHECK-008] Is React Context used for global state management?
- [ ] [FE-CHECK-009] Is React Router used for client-side routing?
- [ ] [FE-CHECK-010] Is the API base URL configured via `VITE_API_BASE_URL` environment variable and documented in project setup?
- [ ] [FE-CHECK-011] Are Vitest and React Testing Library used for unit and component tests?
- [ ] [FE-CHECK-012] Is ESLint configured with the standard ruleset?
- [ ] [FE-CHECK-013] Is Prettier configured with a shared config committed to the repo?
- [ ] [FE-CHECK-014] Are naming conventions followed (PascalCase for components, camelCase for hooks, kebab-case for files)?
- [ ] [FE-CHECK-015] Are components co-located with their styles and tests in a folder?
- [ ] [FE-CHECK-016] Is there a `src/pages` directory with a dedicated file for each page?
- [ ] [FE-CHECK-017] Is there a `src/components` directory with each component's file, styles, and tests co-located inside a component folder?
- [ ] [FE-CHECK-018] Is there a dedicated folder for API client code?
- [ ] [FE-CHECK-019] Is there a dedicated folder for custom hooks?
- [ ] [FE-CHECK-020] Is there a dedicated folder for shared TypeScript types and interfaces?
- [ ] [FE-CHECK-021] Are all imports using absolute paths from `src/`?
- [ ] [FE-CHECK-022] Are side effects handled with `useEffect` or custom hooks, with no inline side effects in render?
- [ ] [FE-CHECK-023] Are all components function components using React.FC or explicit prop types? No class components?
- [ ] [FE-CHECK-024] Does each file contain exactly one component, with the filename matching the component name?
- [ ] [FE-CHECK-025] Are ref hooks (`useRef`) avoided throughout the codebase?
- [ ] [FE-CHECK-026] Is memoisation (`React.memo`, `useMemo`, `useCallback`) applied only where re-render cost is justified?
- [ ] [FE-CHECK-027] Are auth tokens stored in httpOnly cookies and is `localStorage` for tokens explicitly forbidden?
- [ ] [FE-CHECK-028] Is `dangerouslySetInnerHTML` banned and is a CSP header enforced?
- [ ] [FE-CHECK-029] Are state-mutating requests protected with SameSite cookies or a CSRF token?
- [ ] [FE-CHECK-030] Are tokens, passwords, and PII excluded from global state and browser storage?
- [ ] [FE-CHECK-031] Are all third-party scripts reviewed and sandboxed before use?
- [ ] [FE-CHECK-032] Is `npm audit` run in CI with a configured fail threshold?
- [ ] [FE-CHECK-033] Are all `VITE_` environment variables confirmed to be non-sensitive?
- [ ] [FE-CHECK-034] Are externally loaded scripts and stylesheets protected with Subresource Integrity (SRI)?
- [ ] [FE-CHECK-035] Are internal navigation targets validated before performing any redirect?
- [ ] [FE-CHECK-036] Will the UI use themes?
- [ ] [FE-CHECK-037] Will the UI support multiple languages (i18n)?
- [ ] [FE-CHECK-038] Will the UI have a dark mode?
- [ ] [FE-CHECK-039] Does each function have a single responsibility, and are functions that do more than one thing refactored into smaller functions?
- [ ] [FE-CHECK-040] Are stories decomposed into tasks that correspond to one container (application service, database, or infrastructure component) to ensure clear implementation scope, ownership, and independent testing and deployment?
- [ ] [FE-CHECK-041] Is the implementation and tests, when it exists, fully aligned with documentation, constitution, specification and tasks?
- [ ] [FE-CHECK-042] Do the dependencies in `package.json` have exact pinned versions?
