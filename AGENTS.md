# RaahSathi — AGENTS.md

> **Authority:** This file and `PLAN.md` are the supreme project instructions for Round 1.
> **Project:** RaahSathi
> **Hackathon:** Build What Moves India
> **Round 1 deadline:** 28 August 2026, 8:00 p.m. IST
> **Team:** 2 people
> **Scope:** Browser-based, Delhi-only, independent driving-licence public-service PoC using synthetic data only.

## 1. Supreme-authority rule

RaahSathi is a **fresh implementation**. Previous repositories, scaffolds, phases, and source code from the earlier four-person project are not authoritative. Earlier research remains useful only as problem and service input.

`AGENTS.md` and `PLAN.md` must never be silently changed, bypassed, or superseded by Codex.

If implementation appears to require changing architecture, stack, security, ownership, scope, migrations, or these files, Codex must:

1. stop before making the conflicting change;
2. state the exact conflict;
3. explain the smallest proposed change and its consequences;
4. explicitly ask for permission;
5. wait for approval.

Always ask first before changing:

- Next.js, NestJS, PostgreSQL, or Prisma;
- frontend/backend ownership;
- auth/session or authorization model;
- REST `/api/v1` strategy;
- security guarantees;
- Hindi requirement;
- synthetic-data-only rule;
- Round 1 priorities or scope cuts;
- critical testing requirements;
- significant dependencies;
- destructive migrations;
- `AGENTS.md` or `PLAN.md`.

When this file and `PLAN.md` appear to conflict, stop and ask which instruction should prevail. Do not infer that the newer file silently overrides the other.

## 2. Product and hackathon context

RaahSathi is an independent PoC that rethinks the citizen experience for digital driving-licence services. It is not an official government service.

Round 1 requires a live public browser app, a two-minute video, a short text summary, and partner details. The project should therefore demonstrate a small number of **real systemic improvements** across as many useful services as can safely reuse them.

The product must:

- clearly disclose that it is an independent prototype;
- use only synthetic people, licences, payments, documents, RTOs, and provider events;
- never use official emblems or branding or imply government affiliation;
- never claim formal compliance, certification, or “government-grade security”;
- describe its security as production-oriented PoC design for sensitive public-service workflows.

Do not access, automate, test, submit to, scrape, reverse-engineer, or interfere with a live government service. Public documentation may be used as evidence, but transactional government pages and undocumented or private APIs are out of scope.

## 3. Six problem areas RaahSathi is designed to address

### 3.1 Payment-state inconsistency

Problem: external payment can succeed while the application remains pending.

Required redesign behavior:

- browser is never authoritative for payment success;
- delayed or duplicate provider results are safe;
- one logical successful payment advances the application exactly once;
- closing the browser does not break convergence.

Do not claim the existing system definitely lacks webhooks or idempotency; exact internals were not verified.

### 3.2 Fragile workflow recovery

Problem: navigation or re-entry can produce form-resubmission or restart behavior.

Required redesign behavior:

- application progress is durable backend state;
- completed sections survive refresh, logout, browser restart, and session renewal;
- re-entry never requires replaying an unsafe mutation;
- status and next action are reconstructed from the database.

### 3.3 Downtime, slowness, and failure resilience

Do not claim the existing service has “one old server” or no scaling. Historical material already indicates centralized infrastructure with horizontal scaling.

Round 1 should instead demonstrate:

- small failure blast radius;
- explicit dependency states;
- graceful degradation;
- bounded retries;
- unrelated workflows staying usable when a simulated provider fails.

### 3.4 Legacy licence gaps

Round 1 may implement a small synthetic flow:

`canonical search -> possible legacy match -> reconciliation request -> request status`

Never silently merge ambiguous records. No admin resolution UI is required in Round 1.

### 3.5 Aadhaar, OTP, and e-sign recovery

Do not claim RaahSathi can make an external identity provider always available.

Model outcomes explicitly and preserve workflow state:

`VERIFIED`, `OTP_INVALID`, `USER_MISMATCH`, `TIMEOUT`, `PROVIDER_UNAVAILABLE`, `RETRY_REQUIRED`.

### 3.6 Opaque appointment availability

Primary experience:

`service -> RTO -> calendar -> date -> time slots/capacity -> direct booking`

Unavailable states must be explicit, for example:

- `CAPACITY_FULL`
- `SLOTS_NOT_RELEASED`
- `CENTER_UNAVAILABLE`
- `BOOKING_SERVICE_UNAVAILABLE`

Waitlist is a fallback, not the primary booking path.

## 4. Product principles

Every implemented citizen journey should answer:

1. What is my current status?
2. What should I do next?
3. Why can I not continue?
4. When can I continue, if known?
5. Is my progress safe?
6. What is simulated?

Build shared primitives instead of disconnected service-specific implementations.

## 5. Round 1 priorities

### P0 — must genuinely work

- synthetic mobile OTP login;
- PostgreSQL-backed secure sessions;
- server-side ownership authorization;
- New Learner Licence;
- Permanent Driving Licence;
- durable section drafts and resume;
- backend-derived status, next action, and blocking reason;
- immutable application history and audit;
- fee snapshot and simulated payment;
- idempotent payment convergence;
- multiple seeded Delhi RTOs;
- calendar availability;
- time-slot capacity;
- explicit availability reasons;
- concurrency-safe booking;
- preference-aware strict-FIFO waitlist;
- temporary slot offers;
- English **and Hindi for every implemented citizen-facing flow**;
- critical automated security and correctness tests;
- deployed public citizen UI and API.

### P1 — add by reuse after P0 is stable

- Renewal;
- Duplicate or Replacement DL;
- Change of Address;
- Mobile Number Update;
- small persisted 5–10 question learner test;
- synthetic legacy lookup and reconciliation request/status.

### P2 — only after P0/P1 stability

- OpenAI help and explanation assistant;
- deeper legacy or test features;
- extra visual effects;
- lower-value catalogue breadth.

Codex must not cut scope itself. If schedule pressure requires a cut, report it and ask permission. Hindi is not a cut candidate.

## 6. Team roles

### Person A — Frontend / Citizen Experience Owner

Background: stronger Flutter/UI experience; learning React/Next.js.

Owns:

- `apps/web/**`;
- Next.js citizen app;
- mobile-first UX;
- Tailwind CSS and shadcn/ui;
- selective Aceternity UI;
- React Hook Form and Zod;
- English/Hindi presentation;
- login, dashboard, and services;
- guided forms;
- save/resume UX;
- status/history;
- payment and identity recovery UI;
- RTO, calendar, and time-slot UI;
- waitlist/offer UI;
- licence and legacy citizen screens;
- API integration;
- frontend accessibility and security hygiene;
- Vercel deployment;
- visible demo journey.

Frontend must never be authoritative for eligibility, payment success, capacity, queue order, authorization, or workflow transitions.

### Person B — Backend / Security & Data Owner

Background: more familiar with JavaScript/backend development.

Owns:

- `apps/api/**`;
- NestJS;
- Prisma, PostgreSQL, and Neon;
- schema, migrations, and seeds;
- authentication and session infrastructure;
- CSRF, CORS, and rate limits;
- resource ownership authorization;
- REST and OpenAPI;
- workflows and status derivation;
- payments;
- identity-provider simulation;
- appointments, capacity, and transactions;
- waitlist and offers;
- licences and legacy reconciliation;
- audit and history;
- Jest, Supertest, and race tests;
- API and database deployment.

### Shared

- API contracts and OpenAPI-generated types;
- end-to-end tests;
- demo seed and scenario;
- deployment integration;
- submission;
- any permission-gated change.

Codex should stay in the assigned owner’s area. Do not casually refactor the other owner’s code. Cross-owner changes must be narrowly scoped, explicitly called out, and coordinated before implementation.

## 7. Locked architecture

Frontend:

- Next.js with TypeScript;
- Tailwind CSS;
- shadcn/ui;
- Aceternity UI selectively;
- React Hook Form;
- Zod.

Backend:

- NestJS with TypeScript;
- REST `/api/v1`;
- NestJS Swagger/OpenAPI;
- strict NestJS DTO validation.

Database:

- PostgreSQL on Neon;
- Prisma;
- one product database.

Authentication:

- synthetic OTP;
- opaque server-managed sessions persisted in PostgreSQL;
- secure HttpOnly cookie;
- no auth tokens in `localStorage` or `sessionStorage`.

Deployment target:

- Next.js: Vercel;
- NestJS: a managed Node host such as Railway;
- database: Neon.

Trust path:

`Browser -> HTTPS -> Next.js -> credentialed HTTPS -> NestJS -> server-only Prisma -> PostgreSQL`

Prefer same-site custom app/API subdomains when available. If separate hosting domains require cross-site cookies, preserve Secure cookies, CSRF protection, and strict origin controls rather than weakening security.

## 8. Fresh repository and boundaries

Preferred shape:

```text
RaahSathi/
├── apps/
│   ├── web/        # Person A
│   └── api/        # Person B
├── packages/
│   └── contracts/  # generated/shared wire artifacts only if needed
├── docs/
├── AGENTS.md
├── PLAN.md
├── README.md
├── package.json
└── pnpm-workspace.yaml
```

Use a pnpm workspace. Keep application business logic inside its owning app. `packages/contracts` may contain generated OpenAPI types and small wire-format utilities, but it must not become a shared business-logic package or allow the frontend to import backend internals.

Do not copy earlier application code into this repository. Before adding code or assets from any external source, verify its licence and attribution requirements.

## 9. API, workflow, and data invariants

- All externally exposed API routes live under `/api/v1` and use a consistent JSON error envelope with a stable machine-readable code, safe user-facing message key, correlation ID, and optional field errors.
- Validate, normalize, and reject unexpected input at the API boundary. Apply explicit request-size and file constraints where relevant.
- Derive workflow status, next action, eligibility, fees, blocking reasons, and availability on the backend. The frontend renders these results and never reconstructs authoritative state independently.
- Every protected lookup and mutation must verify the authenticated citizen owns the target resource. Use deny-by-default authorization; never rely on route secrecy or hidden UI controls.
- Consequential mutations must be idempotent where browser retry, provider retry, or network ambiguity can occur. Persist idempotency state rather than relying on process memory.
- Use database transactions, constraints, and row-level locking where needed to prevent double payment, double advancement, oversold appointments, duplicate active offers, and queue-order violations.
- Persist application history, payment events, appointment events, waitlist events, offers, and security-relevant audit records as append-only facts. Corrections create new facts; they do not rewrite history.
- Store a fee snapshot on the application/payment intent so later catalogue changes do not alter the amount already presented to the citizen.
- Strict FIFO means allocation order is based on persisted enqueue time among entries whose service, RTO, date/time preferences, eligibility, and current state match the released capacity. A later entrant must never bypass an earlier matching eligible entrant.
- Slot offers expire at a persisted server-side time. Acceptance must atomically verify ownership, offer state, expiry, and capacity before creating the booking.
- Use UTC for stored timestamps and include an explicit timezone when presenting dates or deadlines. Citizen-facing Delhi times use `Asia/Kolkata`.
- All list endpoints must be bounded and paginated. Add indexes for ownership, status, queue order, availability, and other demonstrated query paths.
- Do not place identity data, OTPs, session tokens, document content, or other sensitive values in URLs, logs, analytics, exceptions, audit payloads, screenshots, or client-readable caches.

## 10. Security and privacy baseline

- Collect the minimum synthetic data needed to demonstrate the journey. Seeds, tests, screenshots, videos, logs, and deployed demo records must contain synthetic values only.
- Hash OTPs and session secrets at rest. OTPs must have short expiry, one-time use, attempt limits, resend cooldowns, and rate limits. Never log plaintext OTPs outside an explicitly labelled local-only demo mechanism approved in `PLAN.md`.
- Session cookies must be opaque, HttpOnly, Secure in deployed environments, narrowly scoped, and configured with the strongest SameSite setting compatible with the selected deployment. Rotate sessions after authentication and other privilege transitions; enforce idle and absolute expiry.
- Protect state-changing requests with CSRF controls and strict origin validation. Configure CORS as an exact allowlist with credential use only where required; never combine credentials with a wildcard origin.
- Apply rate limits to authentication, OTP, payment, identity, appointment, waitlist, offer, and other abuse-sensitive endpoints. Avoid user or record enumeration through response bodies, timing differences where practical, and unbounded lookup endpoints.
- Use security headers and a restrictive Content Security Policy. Do not weaken them globally to accommodate a visual component.
- Keep all secrets server-side, outside source control and client bundles. Provide `.env.example` files with names and safe descriptions only, never values.
- Use parameterized Prisma operations. Do not construct SQL from untrusted strings. Raw SQL requires a documented need and focused tests.
- Sanitize structured logs and attach correlation IDs. Record attributable audit events for consequential actions without recording secrets or prohibited data.
- Use bounded dependency timeouts, safe retries with backoff and jitter, and explicit fallback states. Never retry non-idempotent operations blindly.
- Keep the web and API horizontally scalable: durable state belongs in PostgreSQL or an explicitly approved external service, never only in in-process memory.

## 11. Simulation and external-dependency rules

- Every simulated provider or government action must be labelled as simulated in the citizen UI and represented as simulated in persisted records.
- Provider simulators must expose deterministic scenarios for success, delay, duplicate callback, timeout, unavailable service, invalid OTP, and user mismatch where applicable.
- A browser redirect or client callback is never sufficient evidence of payment or identity success. The API converges state from persisted, authenticated provider events or an internal simulation endpoint with equivalent checks.
- Simulated failures must not corrupt durable progress. The citizen must receive a safe next action and be able to resume after recovery.
- Do not give the OpenAI assistant product-database credentials or citizen records. If P2 is authorized, it receives only a question plus public context keys such as service, page, locale, and reason code. It may explain but cannot decide eligibility, rank users, mutate state, or execute actions. Provide deterministic English and Hindi fallback guidance.

## 12. Citizen experience, Hindi, and accessibility

- Every implemented citizen-facing screen, validation message, status, blocking reason, recovery instruction, notification, and generated citizen-visible record must be available in English and Hindi.
- Do not ship raw backend error text to citizens. Map stable API codes to reviewed bilingual copy while retaining the correlation ID for support and debugging.
- Preserve the chosen locale across navigation and authenticated sessions without placing sensitive state in the URL.
- Design mobile-first for narrow screens, touch use, slow connections, and interrupted sessions. Core tasks must remain usable without animation or high-bandwidth assets.
- Meet WCAG 2.2 AA intent for semantic structure, keyboard use, focus visibility and order, labels, error association, contrast, reduced motion, and screen-reader announcements for dynamic status changes.
- Avoid colour-only status communication. Pair status with text and, where useful, an icon.
- Use progressive disclosure and plain language. Explain official terminology when it cannot be avoided.
- Always show the current status, one primary next action, a blocking reason when applicable, timing or eligibility information when known, progress-safety reassurance, and simulation disclosure.

## 13. Engineering workflow

- Read this file and the relevant parts of `PLAN.md` before changing code. Inspect current implementation and tests before proposing a solution.
- Work only on the assigned task. Preserve user work and unrelated changes; do not perform opportunistic refactors.
- Use small, reviewable branches and commits. Keep generated files and lockfile changes in the same change that requires them.
- Document new environment variables, API behavior, migrations, seeds, and operator or demo steps in the same pull request.
- Public API changes start with the backend contract/OpenAPI definition. Regenerate shared types and update consumers in the same coordinated change.
- Prisma schema changes require a named migration, compatible deployment order, seed updates where needed, and a rollback or forward-recovery note. Never use destructive reset commands against shared or production-like databases.
- Significant new dependencies require permission. Before asking, explain the use case, maintenance/security implications, bundle or runtime cost, and why existing dependencies are insufficient.
- Do not suppress type, lint, security, or test failures merely to make a check pass. Fix the cause or report the blocker.
- Keep documentation honest about what works, what is simulated, what is seeded, and what remains out of scope.

## 14. Required testing

Every change must include tests proportional to its risk. Critical controls require automated tests, not demo-only verification.

Minimum P0 backend coverage:

- OTP expiry, one-time use, attempt limits, resend cooldown, and rate limiting;
- session creation, rotation, expiry, logout, CSRF rejection, and strict origin/CORS behavior;
- unauthenticated denial, cross-user denial, record enumeration resistance, and unexpected-field rejection;
- durable draft save/resume across logout and a new session;
- valid and invalid workflow transitions plus backend-derived status/next action;
- duplicate, delayed, reordered, and replayed payment events advancing an application exactly once;
- appointment capacity under concurrent requests with no overselling;
- matching waitlist FIFO order, preference filtering, offer exclusivity, expiry, acceptance races, and capacity recovery;
- provider timeout/unavailable recovery without loss of application progress;
- append-only history and audit creation for consequential actions.

Minimum P0 frontend and end-to-end coverage:

- English and Hindi completion of the primary learner-to-permanent-licence journey;
- refresh, browser restart, logout/login, and retry recovery at critical steps;
- status, next action, blocking reason, timing, progress safety, and simulation labels;
- payment pending-to-success convergence without trusting a success page;
- calendar, availability reasons, direct booking, waitlist entry, offer, and confirmation;
- keyboard-only operation and automated accessibility checks on critical screens;
- mobile viewport behavior and a throttled-network smoke test.

Race tests must use real PostgreSQL transaction behavior, not only mocked repositories or SQLite. External network calls must not be required for deterministic CI tests.

## 15. Definition of done

A task is done only when:

- behavior matches this file, `PLAN.md`, and the assigned acceptance criteria;
- authorization and validation are enforced server-side;
- state and recovery behavior remain correct under retry, refresh, and dependency failure;
- relevant English and Hindi UI is complete;
- simulations are visibly and persistently labelled;
- tests for normal, failure, authorization, replay, and concurrency-sensitive paths pass as applicable;
- lint, type-check, unit, integration, and relevant end-to-end checks pass;
- migrations, generated contracts, seeds, configuration examples, and documentation are updated together;
- no secret, real personal data, prohibited branding, or unsupported claim is introduced;
- the pull request clearly states what is real, what is simulated, how it was verified, and any known limitation.

For the Round 1 release, P0 is done only when the public citizen UI and API are deployed, the primary journey works end to end with synthetic data in both languages, critical tests pass, and the two-minute demo can be repeated from a documented seed state.

## 16. Pull-request checklist

Every pull request should answer:

- Which priority and user problem does this change address?
- Which owner area does it touch, and was cross-owner work coordinated?
- What public API, schema, migration, environment, or dependency changes occur?
- What security, privacy, authorization, idempotency, and concurrency risks were considered?
- Which English and Hindi citizen states changed?
- What is real, simulated, or intentionally out of scope?
- Which automated and manual checks were run, and what were the results?
- How can a reviewer reproduce the behavior with synthetic data?

If any answer reveals a conflict with a locked decision, stop and request approval before continuing.
