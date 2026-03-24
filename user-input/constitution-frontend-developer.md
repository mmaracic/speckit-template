# Frontend Developer Guidelines & Constraints

This document captures frontend developer input to be used when generating the project constitution via `speckit.constitution`.
Answer or refine each section before running the constitution agent.

---

## 1. React Setup & Tooling

- **Build tool**: Mandatory use of Vite
- **Language**: Mandatory use of TypeScript
- **Package manager**: npm, yarn, or pnpm?
- **Monorepo**: Is the frontend a separate repository, or co-located with the backend?
- **Node version**: Minimum Node.js version required?

---

## 2. UI Framework & Design System

- **Component library**: MUI, Chakra UI, Ant Design, shadcn/ui, Tailwind + Headless UI, or custom?
- **CSS approach**: CSS Modules, styled-components, Tailwind CSS, or plain CSS?
- **Icon set**: Which icon library (Lucide, Heroicons, FontAwesome)?
- **Design tokens**: Are spacing, typography, and colour tokens defined centrally?
- **Dark mode**: Required from the start, or post-MVP?
- **Responsive breakpoints**: Mobile-first? Which breakpoints are standard?

---

## 3. State Management

- **Global state**: React Context, Redux Toolkit, Zustand, Jotai, or none?
- **Server state / data fetching**: React Query (TanStack Query), SWR, RTK Query, or fetch/axios directly?
- **Form state**: React Hook Form, Formik, or native controlled components?
- **URL state**: Is URL search params used as state for filters/pagination?

---

## 4. Routing

- **Router library**: React Router v6+, TanStack Router, or Next.js App Router?
- **Route protection**: How are authenticated routes guarded?
- **Lazy loading**: Are routes code-split by default?
- **404 / error boundaries**: Standard error boundary strategy?

---

## 5. API Integration

- **How is the API base URL configured**: Environment variables only?
- **API client generation**: Generated from OpenAPI spec (openapi-typescript, orval), or hand-written?
- **Authentication token handling**: Interceptors, httpOnly cookies, or manual headers?
- **Error normalisation**: Is there a shared error type/map for API responses?

---

## 6. Testing

- **Unit / component tests**: Vitest + React Testing Library, or Jest?
- **End-to-end tests**: Playwright, Cypress, or none for MVP?
- **Snapshot tests**: Allowed or discouraged?
- **Minimum coverage**: Is a coverage threshold enforced in CI?
- **Storybook**: Is component isolation via Storybook required?

---

## 7. Code Quality & Conventions

- **Linter**: ESLint with which ruleset (airbnb, standard, recommended)?
- **Formatter**: Prettier? Shared config committed to the repo?
- **Import ordering**: Enforced via eslint-plugin-import or similar?
- **Component file structure**: One component per file? Co-located test files?
- **Naming conventions**: PascalCase components, camelCase hooks, kebab-case files?
- **Barrel exports (`index.ts`)**: Allowed or discouraged?

---

## 8. Performance Standards

- **Core Web Vitals targets**: LCP, CLS, INP thresholds?
- **Bundle size budget**: Maximum initial bundle size?
- **Image optimisation**: `<img>` with explicit dimensions, lazy loading, WebP/AVIF?
- **Font loading**: System fonts, Google Fonts, or self-hosted? Font display strategy?
- **Memoisation policy**: When is `React.memo` / `useMemo` / `useCallback` appropriate?

---

## 9. Accessibility (a11y)

- **Standard**: WCAG 2.1 AA minimum?
- **axe / eslint-plugin-jsx-a11y**: Linting for accessibility violations in CI?
- **Keyboard navigation**: All interactive elements must be keyboard-reachable?
- **Screen reader testing**: Which screen readers are tested against?

---

## 10. Security

- **Token storage**: Are auth tokens stored in httpOnly cookies (preferred) or memory? `localStorage` is explicitly forbidden?
- **XSS prevention**: Is `dangerouslySetInnerHTML` banned? Is a Content Security Policy (CSP) header enforced?
- **CSRF protection**: Are state-mutating requests protected (SameSite cookies, CSRF token)?
- **Sensitive data in state**: Are tokens, passwords, or PII ever stored in global state or browser storage? Policy?
- **Third-party scripts**: How are third-party scripts (analytics, chat widgets) reviewed and sandboxed?
- **Dependency auditing**: Is `npm audit` / `pnpm audit` run in CI with a fail threshold?
- **Environment variables**: Are all `VITE_` prefixed variables non-sensitive by policy? (Vite inlines them into the bundle)
- **Subresource Integrity (SRI)**: Are externally loaded scripts and stylesheets integrity-checked?
- **Open redirect prevention**: Are internal navigation targets validated before redirect?

---

## 11. Internationalisation (i18n)

- **Required at launch**: Yes / No?
- **Library**: react-i18next, LinguiJS, or other?
- **String extraction**: Are translation keys extracted automatically?
- **Right-to-left (RTL)**: Must the layout support RTL languages?

---

## 12. Open Questions

Questions that must be answered before the constitution can be finalised:

- [ ] Is server-side rendering (SSR) or static generation (SSG) needed for SEO?
- [ ] Are micro-frontends or module federation in scope?
- [ ] What is the browser support matrix (evergreen only, IE11, Safari minimum version)?
- [ ] Is a PWA / service worker required?
- [ ] How are environment-specific feature flags managed on the frontend?
- [ ] Is analytics instrumentation (GA4, Mixpanel, Plausible) required and privacy-compliant?
