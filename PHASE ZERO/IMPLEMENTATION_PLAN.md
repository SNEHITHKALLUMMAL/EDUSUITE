# EduSuite Implementation Plan

Phases follow the JSON `project_phases`. Phase 0 is complete. Each phase is delivered as small vertical slices, one milestone at a time.

## Working rules for every milestone

1. Read the relevant SRS rows in `docs/IMPLEMENTATION_STATUS.md` and the existing code. Check `git status` first.
2. Pick the smallest coherent slice. State assumptions briefly.
3. Implement database, backend, frontend and integration for that slice, with migrations, validation and tests.
4. Run formatting, static analysis, tests and builds. Fix confirmed failures before moving on.
5. Review authorization, tenant isolation and sensitive-data handling. Review the Git diff.
6. Update `PROJECT_STATE.md`, `IMPLEMENTATION_STATUS.md` and `DECISIONS.md`.
7. Report what was done, what was run, failures, blockers and the next step. Status is "Tested" only after a check has actually run.

**Release-blocking in every phase:** cross-tenant data leakage, broken authorization, exposed secrets, and unauthorized access to private assets.

## Overview

| Phase | Goal | Decisions needed first | Main SRS groups |
|---|---|---|---|
| 1 | Foundation and development environment | Q1 (where to verify), Q2 (name) | COM, NFR-MAIN, NFR-PORT, UIR, SEC-013..018 |
| 2 | Authentication and tenant security | Q4, Q5, G-14, G-15, G-05 | FR-AUTH, FR-INS, FR-RBAC, FR-AUDIT, SEC-001..012, DR-001..005 |
| 3 | Institutional and academic core | Q3, ERD, G-06, G-07, G-19 | FR-STU, FR-STAFF, FR-ACAD, FR-PARENT, FR-SEARCH |
| 4 | Attendance and timetable | Q6, G-18 | FR-ATT, FR-TIME |
| 5 | Fees, payments, examinations, results | Q7, Q8, Q9 | FR-FEE, FR-EXAM |
| 6 | Files, announcements, notifications | G-12 spike, Q9 | FR-FILE, FR-NOT, SEC-019..024 |
| 7 | Dashboards and reports | G-27 criteria | FR-REP |
| 8 | Security hardening and deployment | Q10, G-13, G-21, G-26 | NFR-PERF, NFR-REC, NFR-REL, SEC-025..030 |

## Phase 1: Foundation and development environment

**Goal:** both apps start, talk to each other, and the database is reachable and migratable.

**Proposed slices, in order**

1. API skeleton in `apps/api`: app/server split, environment config validated with Zod, structured logging, request IDs, security headers, CORS allow-list, rate limiting, centralized error handler with one consistent error shape (G-23), request validation helper.
2. PostgreSQL via Docker Compose, Prisma setup, and a baseline migration. Proposed baseline: the `institutions` table only, since every tenant-owned table hangs off it. Confirm at the start of the phase.
3. `GET /api/v1/health` that checks database connectivity. OpenAPI document with the health endpoint.
4. API tests (Vitest, Supertest) for health, validation and error format. ESLint and Prettier.
5. Flutter app in `apps/mobile` for Android and web (iOS folder generated but unverified). Core layers: config, theme and design tokens, router, Dio client, error mapping, shared state widgets (loading, empty, error). One screen that calls the health endpoint, to prove the integration end to end.
6. `.env.example` files, root README with run and test commands, `.gitignore` check for secrets, CI workflow if the host supports it.

**Exit criteria (from the JSON, must be demonstrated by running them)**

- Frontend and backend start successfully.
- Database connectivity is verified.
- The initial migration runs successfully.
- The health endpoint responds correctly.
- Basic automated checks pass (API tests, `dart analyze`, Flutter tests).

**Not possible in the Phase 0 sandbox:** Flutter, Dart, PostgreSQL and Docker are missing, and package and artifact hosts are unreachable (R-04, D-014).

## Phase 2: Authentication and tenant security

- **Slices:** the ERD for the identity core; users, institutions, memberships, roles, permissions and refresh sessions; password hashing (Argon2id preferred); login, logout, refresh rotation and revocation; tenant-context and authorization middleware; audit log foundation; development seed data; Flutter auth screens and session handling; Row-Level Security spike.
- **Decisions to confirm:** D-009, D-016, web session strategy (G-15), mailer approach (G-03), login identifier (G-04), audit immutability (G-22).
- **Exit criteria:** end-to-end authentication works; protected endpoints enforce authorization; tenant isolation tests pass; expired, revoked and invalid tokens are rejected (TC001, TC002, TC005, TC012); secrets stay out of source control and frontend bundles.

## Phase 3: Institutional and academic core

- **Slices:** institution settings; academic years, departments, courses, classes, sections, subjects; staff and student profiles; guardian links; minimal admissions; enrollments; teacher assignments; search and pagination conventions; matching Flutter screens.
- **Prerequisite:** the ERD with resolved vocabulary (G-17, C-06), recorded in `DECISIONS.md`.
- **Exit criteria:** core records are manageable through authorized workflows; relationship and uniqueness rules are enforced; isolation tests cover every implemented module (AC-004, AC-005, AC-010; TC003, TC011).

## Phase 4: Attendance and timetable

- **Slices:** attendance sessions and records with the uniqueness rule; corrections with permission and audit; summaries and configurable percentage rule; timetable entries with teacher and class conflict checks; teacher, student and guardian views.
- **Prerequisite:** attendance rules (Q6, G-02).
- **Exit criteria:** workflows run end to end; unauthorized changes are rejected; calculations and conflict checks are tested (AC-006, AC-007; TC004, TC006).

## Phase 5: Fees, payments, examinations, results

- **Slices:** fee categories and structures; invoices; append-only payments and adjustments in transactions with idempotency; receipts; outstanding balances; exam configuration and schedules; marks validation; result calculation; draft, reviewed and published states; student result views.
- **Prerequisites:** grading rules (Q7, G-01), fee rules (Q8, G-08, G-09). A payment gateway only if approved (Q9).
- **Exit criteria:** financial records are consistent and permission-protected; result calculations match documented rules; integrity tests pass (AC-008, AC-009, AC-014; TC007, TC008, TC009).

## Phase 6: Files, announcements, notifications

- **Slices:** a verification spike on Cloudinary private delivery with a real account (G-12); backend-controlled signed uploads; asset metadata and ownership in SQL; profile images and branding; student documents if the spike supports them; announcements with role targeting; in-app notifications; push only if approved.
- **Exit criteria:** file operations enforce tenant and role permissions; unauthorized asset access is prevented; notifications reach only authorized recipients (AC-011; TC010).

## Phase 7: Dashboards and reports

- **Slices:** role-specific dashboards; attendance, admission, fee and exam summaries; tenant and role restrictions on every report; query optimisation; exports only if approved.
- **Exit criteria:** dashboards show database-backed values; reports enforce restrictions; calculations are tested against sample data (proposed AC-P2).

## Phase 8: Security hardening and deployment

- **Slices:** authentication and authorization review; full cross-tenant test pass; SQL, constraint, index and migration review; Cloudinary access review; CORS, rate limits, headers and environment review; dependency audit; performance test on a documented workload; backup and restore demonstration (AC-015); deployment, monitoring and release checklist; known limitations list.
- **Prerequisites:** hosting and database provider (G-21), legal review (G-13), workload definition (G-26).
- **Exit criteria:** critical security issues fixed or recorded as blockers; tests and builds actually run; deployment and recovery documented; production-readiness statements backed by evidence.
