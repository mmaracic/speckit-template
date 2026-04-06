# Frontend Developer React Guidelines & Constraints

First section of this document captures react developer constitution principles to be used when generating the project constitution via `speckit.constitution`. Second section captures project-specific overrides and additions. Third section defines the gate checklists. Here there is only one checklist and it applies to the task and implementation speckit steps.
Every mandatory bullet with an `FE-` or `FE-CHECK-` ID must be copied into the generated constitution and downstream plan checks with one-to-one traceability. Mandatory bullets must not be skipped, merged away, or summarized.
---

## 0. Copy and Traceability Principles

- **MANDATORY:** [FE-COPY-001] Every mandatory source item in this file must be copied into generated constitution artifacts.
- **MANDATORY:** [FE-COPY-002] Mandatory source items must not be skipped, merged, or summarized into generic wording.
- **MANDATORY:** [FE-COPY-003] If a generated constitution needs more sections or principles to preserve fidelity, add them instead of collapsing items.
- **MANDATORY:** [FE-COPY-004] Generated constitution artifacts must include a traceability matrix that maps each `FE-` and `FE-CHECK-` ID.
- **MANDATORY:** [FE-COPY-005] Each source ID must map to exactly one explicit constitution clause or one explicit plan-check entry, unless a justified exception is documented.

---

## 1. Mandatory Constitution principles
### 1. React Setup & Tooling

- **MANDATORY:** [FE-001] **Build tool**: Mandatory use of Vite
- **MANDATORY:** [FE-002] **Language**: Mandatory use of TypeScript
- **MANDATORY:** [FE-003] **Package manager**: Use npm
- **MANDATORY:** [FE-004] **Monorepo**: Frontend is a separate repository from backend
- **MANDATORY:** [FE-005] **Node version**: Minimum node version 22

---

### 2. UI Framework & Design Systems

- **MANDATORY:** [FE-006] **Component library**: Axios for API calls, Shadcn/ui for components
- **MANDATORY:** [FE-007] **CSS approach**: Use styled-components and Tailwind CSS

---

### 3. State Management

- **MANDATORY:** [FE-008] **Global state**: Use React Context for global state management
---

### 4. Routing

- **MANDATORY:** [FE-009] **Router library**: Use React Router for client-side routing
---

### 5. API Integration

- **MANDATORY:** [FE-010] **How is the API base URL configured**: Use environment variables (e.g., `VITE_API_BASE_URL`) for API base URL configuration, and ensure that these variables are properly documented and included in the project setup instructions.

---

### 6. Testing

- **MANDATORY:** [FE-011] **Unit / component tests**: Use Vitest + React Testing Library

---

### 7. Code Quality & Conventions

- **MANDATORY:** [FE-012] **Linter**: Use ESLint with standard ruleset
- **MANDATORY:** [FE-013] **Formatter**: Use Prettier with shared config committed to the repo
- **MANDATORY:** [FE-014] **Naming conventions**: Use PascalCase naming for components, camelCase for hooks, kebab-case for files
- **MANDATORY:** [FE-015] **Folder structure**: Co-locate components with their styles and tests in a folder
- **MANDATORY:** [FE-016] **Page folder structure**: Create a file for each page under `src/pages`
- **MANDATORY:** [FE-017] **Component folder structure**: Create a file for each reusable component under `src/components`, with the component file, styles, and tests co-located inside.
- **MANDATORY:** [FE-018] **Api layer**: Create a dedicated folder for API client code
- **MANDATORY:** [FE-019] **Hooks**: Create a dedicated folder for custom hooks
- **MANDATORY:** [FE-020] **Model folder**: Create a dedicated folder for shared TypeScript types and interfaces
- **MANDATORY:** [FE-021] **Import conventions**: Absolute imports from `src/` only,
- **MANDATORY:** [FE-022] **Effect hook**: Minimize the use of `useEffect` for side effects.
- **MANDATORY:** [FE-023] **Component types**: Use function components with React.FC or explicit prop types, avoid class components.
- **MANDATORY:** [FE-024] **File content**: One component per file, with the filename matching the component name. Model and API files can export multiple related types/interfaces.
- **MANDATORY:** [FE-025] **Refs**: Do not use refs hooks.
---

### 8. Performance Standards

- **MANDATORY:** [FE-026] **Memoisation policy**: Use `React.memo` / `useMemo` / `useCallback` when necessary to prevent expensive re-renders, but avoid premature optimization.
---

### 9. Security

- **MANDATORY:** [FE-027] **Token storage**: Auth tokens are stored in httpOnly cookies. `localStorage` is forbidden
- **MANDATORY:** [FE-028] **XSS prevention**: `dangerouslySetInnerHTML` is banned. A Content Security Policy (CSP) header is enforced.
- **MANDATORY:** [FE-029] **CSRF protection**: State-mutating requests must be protected (SameSite cookies, CSRF token)
- **MANDATORY:** [FE-030] **Sensitive data in state**: Tokens, passwords, or PII must not be stored in global state or browser storage.
- **MANDATORY:** [FE-031] **Third-party scripts**: Third-party scripts (analytics, chat widgets) must be reviewed and sandboxed.
- **MANDATORY:** [FE-032] **Dependency auditing**: `npm audit` / `pnpm audit` must be run in CI with a fail threshold.
- **MANDATORY:** [FE-033] **Environment variables**: All `VITE_` prefixed variables must be non-sensitive by policy. (Vite inlines them into the bundle)
- **MANDATORY:** [FE-034] **Subresource Integrity (SRI)**: Externally loaded scripts and stylesheets must be integrity-checked.
- **MANDATORY:** [FE-035] **Open redirect prevention**: Internal navigation targets must be validated before redirect.

---

## 2. Project-Specific Principles

_Add project-specific overrides or additions to the principles in Section 1. Use the same MANDATORY bullet format with sequential `[FE-NNN]` IDs. Leave this section empty if there are no project-specific constraints._

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
