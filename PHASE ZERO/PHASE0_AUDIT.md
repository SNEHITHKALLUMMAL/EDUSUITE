# EduSuite Phase 0 Audit

- **Date:** 2026-10-17
- **Status:** Complete. Awaiting review before Phase 1.
- **Phase 0 scope:** documentation and repository audit only. No application code was written.

## 1. Scope and method

**Inspected**

- `edusuite_master_prompt.json`: parsed with Python's `json` module (valid; 17 top-level keys). Read in full.
- `SOFTWARE_REQUIREMENTS_SPECIFICATION.pdf` (SRS v1.0, 30 pages): text layer extracted with `pdftotext`. Requirement identifiers were parsed and counted against the SRS: 237 prefixed requirements, 14 business rules, 16 acceptance criteria. All matched. The 14 test scenarios were entered by hand from the SRS table, because the PDF text layer merges the scenario and expected-result columns.
- `EMBASE_Pro_Suite_Step_1_Understand_the_Idea.pdf`: read from the content supplied with the request. Not re-extracted.
- The working environment and any existing repository (section 2.2).

**Not done, because there is nothing to run yet:** no tests, builds, linting or database checks. Nothing in this audit claims a feature works.

## 2. Inventory

### 2.1 Source documents and their role in the priority order

| Document | What it is | Priority level (JSON `source_of_truth`) |
|---|---|---|
| `edusuite_master_prompt.json` | Project specification for the workflow: stack, roles, modules, phases, security rules, change management, definition of done | 1. Explicitly approved user requirements (for stack, process and scope control) |
| SRS v1.0 (17 Oct 2026) | Functional, non-functional, security, database and acceptance requirements | 2. Primary functional source |
| EMBASE Pro Suite, Step 1 | Product-discovery document: hypotheses, validation plan, go/no-go checkpoint, risks | 3. Other project documentation. Context, not a requirements source |

### 2.2 Repository and environment findings

| Check | Finding |
|---|---|
| Existing Git repository | None found (searched `/home/claude` and `/mnt/user-data`, depth 4). Initialised `edusuite/` on branch `main`. No prior work existed to preserve. |
| Existing frontend, backend, database, deployment setup | None. No `pubspec.yaml`, `package.json`, `schema.prisma` or `docker-compose.yml` found. |
| Uncommitted changes | None. The repository was empty. |
| Toolchain present in this sandbox | git 2.43.0, Node v22.22.2, npm 10.9.7 |
| Toolchain absent in this sandbox | Flutter, Dart, PostgreSQL client/server, Docker |
| Network (HTTP HEAD probes, 8 s timeout) | Reachable: `registry.npmjs.org`, `github.com`, `archive.ubuntu.com`. No response: `pub.dev`, `storage.googleapis.com` (Flutter artifact host), `binaries.prisma.sh` (host Prisma has used for engine downloads), `api.cloudinary.com`. These hosts are not on the sandbox network allow-list. |
| Persistence | The sandbox filesystem resets between tasks. Only files copied to the shared outputs folder reach the user. Continuity therefore depends on the user keeping the repository (including `docs/`) and bringing it back. |

**Consequence for Phase 1:** its exit criteria (frontend and backend start, database connectivity, migration runs, Flutter analysis passes) cannot be verified inside this sandbox. See R-04 and question Q1.

## 3. Conflicts and gaps

### 3.1 Conflicts between documents

Resolution follows the JSON priority order: user requirements, then SRS, then other documentation, then existing decisions, then documented defaults.

| ID | Conflict | Sources | Resolution | User input needed? |
|---|---|---|---|---|
| C-01 | Product name: "EMBASE Pro Suite" vs "EduSuite" | Step 1 vs SRS, JSON | Use **EduSuite** in code and docs. | Confirm whether EMBASE is a commercial brand (Q2). |
| C-02 | Step 1 says validate with real users before coding (go/no-go checkpoint, status "concept hypothesis awaiting customer validation", "not production code"). The instruction is to build. | Step 1 §9-12 vs JSON, SRS | Build, as instructed. Recorded as business risk R-01. Keep to SRS MVP scope. No real data until validation and legal review. | No |
| C-03 | Platform scope: Step 1 excludes mobile apps and suggests a responsive web app first. SRS and JSON require Flutter for Android and web. | Step 1 §7, §10 vs SRS §2.5, NFR-PORT, JSON | **Flutter** (user requirement plus SRS). Flutter web delivers the responsive-web path. | No |
| C-04 | iOS: JSON lists Android, iOS and Web as targets. SRS NFR-PORT-001/002 target Android and web first, with iOS added after platform validation. | JSON vs SRS | Android and web are the verified targets. iOS-ready by construction, but not built or verified (D-007). | No |
| C-05 | First-release roles: Step 1 candidate scope lists four roles, with students and guardians as beneficiaries. SRS has six roles and acceptance criteria that require student and guardian access (AC-006, AC-009, AC-010). | Step 1 §7 vs SRS FR-RBAC-001, §13.2 | **SRS: six roles.** | No |
| C-06 | Data-model vocabulary differs. SRS: Classrooms, Attendance Sessions, Class Enrollments, Fee Invoices. JSON: classes + sections, attendance_records only, enrollments, student_fees, plus admissions, guardians, receipts, roles/permissions/role_permissions, refresh_sessions. | SRS §6.2 vs JSON `candidate_entities` | Union of both, reconciled in an ERD before Phase 3 (D-010). The JSON says to treat its list as a starting point. | No |
| C-07 | Fee model: SRS has invoices, payments and adjustments (BR-007). JSON has student fee assignments, receipts, waivers and discounts. | SRS §4.9, §6.2 vs JSON | Proposed: an invoice is the student's fee obligation; payments are append-only; receipts are generated per payment; adjustments (waivers, discounts, refunds) are append-only rows. Confirm in the Phase 5 ERD. | Rules only (Q8) |
| C-08 | User scope: SRS 3.3 says "institution-scoped users", but SRS 6.3 and UC-01 allow one user to hold memberships in several institutions. | SRS §3.3 vs §6.3, §10.1 | Global `users` plus `institution_memberships`; active institution chosen at sign-in or switch and verified server-side (D-009). | Confirm in Phase 2 |
| C-09 | SRS marks some items "should": timetable conflict detection (FR-TIME-004), CSV import (FR-STU-011), exports (FR-REP-009), push (FR-NOT-004), MFA (SEC-006). JSON phases list conflict checks as work to implement, and exports and push as conditional. | SRS vs JSON | Implement conflict checks. Defer CSV import, exports, MFA and push until approved (D-013). | Approval later |
| C-10 | JSON includes admission applications, admission status tracking and student documents. The SRS covers direct student creation and enrollment status, with documents as "may". | JSON vs SRS FR-STU, FR-FILE-003 | No conflict. Include a minimal admissions workflow in Phase 3 and documents in Phase 6. | Scope check (G-19) |
| C-11 | JSON mentions room allocation, notification preferences and branding "where required". SRS has only branding assets (FR-FILE-002). | JSON vs SRS | Treat as conditional. Build only if required. | Later |
| C-12 | SRS §16 development plan has 10 phases and a different order from the JSON's phases 0-8. | SRS §16 vs JSON | JSON phases govern execution. SRS §16 is informational. | No |

### 3.2 Gaps and ambiguities

"Needed by" is the phase that cannot start safely without the answer. None blocks Phase 1.

| ID | Gap or ambiguity | SRS ref | Needed by | Proposed default |
|---|---|---|---|---|
| G-01 | Grading and result calculation rules are not defined (grade bands, aggregation, pass/fail, whether a published result can be unpublished). JSON Phase 5 requires calculations to match "documented rules". | FR-EXAM-006, FR-EXAM-009, AC-009 | 5 | None. Needs institution rules. |
| G-02 | Attendance statuses beyond Present/Absent/Late, whether Late counts as present, the percentage formula, and the correction policy and time window. | FR-ATT-002, 006, 012 | 4 | Make statuses and formula institution-configurable. Record defaults when Phase 4 starts. |
| G-03 | No email or SMS provider for password reset and invitations. | FR-AUTH-005, 010 | 2 | Provider-agnostic mailer interface with a development-only console transport. |
| G-04 | Login identifier is undefined. Students and guardians may not have email. | FR-AUTH-001 | 2 | Email as the identifier. Revisit for students. |
| G-05 | How platform support staff may access tenant data when needed. SRS says Super Admin must not automatically see student records, and Step 1 asks for "limited, logged, justified" support access. | BR-014, SEC-012 | 2 | Default deny. Any support access is an explicit, time-limited, audited grant by the institution. |
| G-06 | What counts as a "verified" guardian relationship. | FR-PARENT-001, 005 | 3 | Administrator-created and administrator-verified link; audited. |
| G-07 | Retention and deletion policy for student, financial, audit and file data. SRS says define it but does not. | SRS 6.6, BR-013, FR-STU-010 | 3 | Archive or deactivate. No hard delete of student or financial records. |
| G-08 | Refund, reversal, waiver and discount rules, and which roles may perform them. | FR-FEE-011, UIR-008 | 5 | Append-only adjustment rows with explicit permission and audit. |
| G-09 | "Overdue" status: computed or stored, grace period, late fees (none specified). | FR-FEE-005 | 5 | Computed from due date and balance. No late fees. |
| G-10 | Payment gateway is neither selected nor approved. SEC-030 implies India, which narrows the field. | FR-FEE-012 | 5 (optional) | Not built. Record payments manually. |
| G-11 | Push notifications (FCM) are not approved. Notification preferences are undefined. | FR-NOT-004 | 6 | In-app notifications only. |
| G-12 | Cloudinary private-file handling and plan limits are unverified. Cloudinary documents private and authenticated delivery types and signed URLs, but fit for non-image documents, plan limits and data location are not confirmed. SRS itself requires verification before production. Student documents contain personal data held by a third party. | FR-FILE-012, SEC-022 | 6 | Verification spike with a real account at the start of Phase 6. Profile images and branding first. |
| G-13 | Indian data-protection compliance (SRS names it). Student data includes children's data. Assessment needed before real data. Not legal advice. Current commencement status of the Digital Personal Data Protection Act, 2023 and its rules has not been verified here. | SEC-028, SEC-030 | Before real data | Development and test data only. |
| G-14 | Whether PostgreSQL Row-Level Security works with Prisma and connection pooling. JSON says consider as defense in depth. | JSON `multi_tenant_security` | 2 | Time-boxed spike in Phase 2. Application-layer isolation and composite foreign keys are the baseline either way. |
| G-15 | Web session strategy: cookies (CSRF needed) or tokens (XSS exposure), and the API origin for Flutter web. | SEC-015, 016 | 2 | Access token in memory, refresh token in an httpOnly secure cookie with CSRF protection on web. Secure storage on mobile. |
| G-16 | Schools and colleges differ. Meaning of academic calendar, "configured profile fields", and per-institution configuration is vague. | FR-INS-008, FR-STU-004, FR-ACAD-009 | 3 | Fixed core schema with institution-level settings. No custom-field engine. |
| G-17 | Class, section, classroom and course are used inconsistently. "Conflicting enrollment" is undefined. | FR-ACAD-003, 007 | 3 (ERD first) | Resolve in the ERD. |
| G-18 | Timetable model: periods vs free times, effective dates, recurrence, room conflicts. | FR-TIME-001..007 | 4 | Weekly recurring entries. Rooms optional. |
| G-19 | Admission workflow stages, who approves, and whether applications arrive through a public form. A public form would add an unauthenticated endpoint. | JSON Admissions | 3 | Staff-entered applications only. No public endpoint. |
| G-20 | Language and localisation, and tolerance of poor connectivity. Step 1 lists these as adoption factors. SRS lists offline sync as out of scope. | Step 1 §5 | None | English only. Online only. |
| G-21 | Hosting provider, managed database provider and environments are undecided. This affects HTTPS, backups, RPO and RTO and availability targets. | SRS 2.4, NFR-REC, NFR-REL-001, COM-001 | 8 | Decide before Phase 8. Local development until then. |
| G-22 | How audit logs are protected against modification. | FR-AUDIT-005 | 2 | Append-only table, with the application database role granted insert and select only. |
| G-23 | API error format, request IDs and conventions are only described as "consistent". | COM, NFR | 1 | Define once in Phase 1 and record in DECISIONS. |
| G-24 | Default and maximum page sizes. | FR-SEARCH-005 | 3 | Decide with the first list endpoint. |
| G-25 | Pilot segment (school or college) is unselected. Step 1 recommends choosing one. It affects grading, fees and structure. | Step 1 §3, §11 | 3 | Model both. Build no segment-specific rules. |
| G-26 | "Normal operating load" for the performance target is undefined. | NFR-PERF-001, 005 | 8 | Define a documented workload before load testing. |
| G-27 | No acceptance criteria exist for notifications, reports and dashboards, audit logs, or search and pagination. | SRS §13.2 | 6-7 | Propose criteria below (section 5.3) for approval. |
| G-28 | Per-institution switches ("where enabled", "where permitted") for student and guardian fee visibility. Step 1 mentions tenant-level enabled modules. | SRS 2.3.4, 2.3.5 | 3 | Institution settings table with a small set of flags. |

## 4. Risks and assumptions

### 4.1 Risks

| ID | Risk | Severity | Mitigation |
|---|---|---|---|
| R-01 | Building before validating demand. Step 1's go/no-go items are unmet (no confirmed segment, workflow, decision-maker or payment path). | High (business) | Keep MVP to the SRS. Run discovery interviews in parallel. Avoid real data until validated. |
| R-02 | One developer, 16 requirement groups, 237 requirements. Scope creep and operations burden. | High | One milestone at a time. Defer "should" and "may" items. Vertical slices. |
| R-03 | Cross-tenant data leakage. Release-blocking by JSON rule. | Critical | Tenant context from verified membership only. Tenant-scoped data access layer. Composite foreign keys including institution. RLS spike. Cross-tenant tests from Phase 2, extended every phase. |
| R-04 | The sandbox cannot run Flutter, PostgreSQL or Docker, cannot reach `pub.dev` or Flutter and Prisma artifact hosts, and resets between tasks. Phase 1 exit criteria cannot be verified here. | High (process) | Build and verify on the user's machine or another environment with the toolchain. Carry the repository and `docs/` between sessions. Never mark Phase 1 complete without running the checks. |
| R-05 | Private student documents on Cloudinary (G-12). | High | Verification spike. Authenticated delivery or an alternative for sensitive documents. Tenant-checked access. |
| R-06 | Legal and regulatory exposure for children's personal data (G-13). | High | Legal review before real data. Data minimisation (DR-010). |
| R-07 | Financial integrity: rounding, double posting, partial updates, silent edits. | High | Decimal types only. Transactions. Idempotency keys. Append-only payments and adjustments. Audit trail. |
| R-08 | Flutter web session handling: XSS and CSRF (G-15). | Medium | Decide in Phase 2. Test explicitly. |
| R-09 | Prisma with RLS and pooling can leak tenant context across requests (JX-SEC-05). | Medium | Spike first. Only adopt with request-scoped transactions and tests. |
| R-10 | Solo operation of 99.5% availability, backups and restore (G-21). | Medium | Managed database with automated backups. Written runbooks. Test restore (AC-015). |
| R-11 | Legacy data migration is hard and CSV import is deferred (FR-STU-011). Step 1 flags this as an adoption risk. | Medium | Schedule import as the first post-MVP increment if the pilot needs it. |
| R-12 | Dependency drift and supply-chain issues. | Medium | Pin versions. Run `npm audit` and Dart outdated checks each phase. Minimise dependencies. |
| R-13 | Unclear grading and attendance rules cause rework (G-01, G-02). | Medium | Get rules before Phases 4 and 5. Make them configuration, not code. |
| R-14 | iOS cannot be built or tested without macOS and Xcode. | Low | Claim only Android and web. Keep iOS-ready. |

### 4.2 Assumptions

| ID | Assumption |
|---|---|
| A-01 | The JSON `source_of_truth.priority_order` governs all conflicts. |
| A-02 | Monorepo layout per SRS 3.4 and 3.5: `apps/api`, `apps/mobile` (Flutter, including its web target), `docs/`. |
| A-03 | Node.js 22 (the version present here), ES modules, plain JavaScript. Express version chosen and recorded at the start of Phase 1. |
| A-04 | Local development uses PostgreSQL, preferably through Docker Compose. Version recorded in Phase 1. |
| A-05 | Money uses `NUMERIC` with an institution-level currency. No default currency is assumed. |
| A-06 | Timestamps are stored as UTC `timestamptz`. Attendance dates use the institution's time zone (BR-011). |
| A-07 | Primary keys are UUIDs (SRS 6.5). |
| A-08 | The six SRS roles are seeded. Permissions are data (roles, permissions, role_permissions), so roles can be added later (FR-RBAC-007). |
| A-09 | No real student data is used before legal review. Seed data and credentials are development-only. |
| A-10 | Admissions are minimal: a staff-entered application record with a status, linked to a student on acceptance. |
| A-11 | Student and financial records are archived or deactivated, not hard-deleted. |
| A-12 | Test tooling: Vitest and Supertest for the API, `flutter_test` for the app, ESLint and Prettier, `dart analyze` and `dart format`. |
| A-13 | No microservices, no background job system and no offline sync in the MVP. |

## 5. Mapping: requirements, modules, acceptance criteria

### 5.1 JSON functional modules to SRS requirements

| JSON module | SRS requirement groups | Phase | Acceptance criteria | Test scenarios |
|---|---|---|---|---|
| Authentication and Account Management | FR-AUTH, SEC-001..006, BR-012 | 2 | AC-001 | TC001, TC002, TC012 |
| Institution and Tenant Management | FR-INS, FR-RBAC, DR-001..005, SEC-007..012, BR-001, BR-002, BR-014 | 2, then 3 for configuration | AC-002, AC-003 | TC005 |
| Admissions and Student Management | FR-STU, JX-ADM-01 (documents JX-ADM-02 in 6) | 3 | AC-004 | TC003 |
| Staff and Teacher Management | FR-STAFF | 3 | AC-005 | TC004 (with attendance) |
| Academic Management | FR-ACAD, JX-ACAD-01, BR-003 | 3 | AC-005 | |
| Parent and Guardian Management | FR-PARENT, BR-009 | 3 | AC-010 | TC011 |
| Attendance Management | FR-ATT, BR-004, BR-005, BR-011 | 4 | AC-006 | TC004, TC006 |
| Timetable Management | FR-TIME | 4 | AC-007 | |
| Fees and Payments | FR-FEE, BR-007, BR-008, JX-FEE-01..03 | 5 | AC-008, AC-014 | TC008, TC009 |
| Examinations and Results | FR-EXAM, BR-006 | 5 | AC-009 | TC007 |
| Announcements and Notifications | FR-NOT | 6 | none in SRS (G-27) | |
| Files (Cloudinary), used by several modules | FR-FILE, SEC-019..024, BR-010 | 6 | AC-011 | TC010 |
| Dashboards and Reports | FR-REP, FR-SEARCH, JX-REP-01 | 7 (search from 3) | none in SRS (G-27) | |
| Audit and Platform Operations | FR-AUDIT, JX-OPS-01..04, NFR-REC | 1 (health, logging), 2 (audit), 8 (recovery) | AC-015, AC-016 | TC014 |
| Cross-cutting | UIR, COM, NFR-*, SEC-013..018, SEC-025..030, DR-006..010 | 1 onward | AC-012, AC-013, AC-016 | TC013 |

### 5.2 Release-blocking checks (JSON `claude_execution_rules.security_rule`)

These are treated as release-blocking in every phase: cross-tenant data leakage, broken authorization, exposed secrets, and unauthorized access to private assets. Each phase from 2 onward must add and run tests for the data it introduces.

### 5.3 Proposed additional acceptance criteria (not approved, not part of the SRS)

Offered to close G-27. They only restate existing SRS requirements as checkable conditions.

| Proposed | Condition | Covers |
|---|---|---|
| AC-P1 | Announcements reach only the intended role groups in the same institution, and notification history shows only the user's own items. | FR-NOT-002, 003, 006, 007 |
| AC-P2 | Each dashboard and report shows database-backed values for the user's institution and role only, and matches hand-computed values on sample data. | FR-REP-001..008 |
| AC-P3 | Security-relevant and administrative actions create audit records with actor, institution, action, entity and time, readable only by authorized roles. | FR-AUDIT-001..005 |
| AC-P4 | List endpoints paginate, enforce a maximum page size, validate sort and filter parameters, and never return other institutions' records. | FR-SEARCH-001..005 |

## 6. Questions for the user

None blocks the start of Phase 1 from a requirements standpoint. Q1 decides where Phase 1 can be verified.

| # | Question | Needed before | Default if unanswered |
|---|---|---|---|
| Q1 | Where should implementation and verification run? This sandbox lacks Flutter, PostgreSQL and Docker, cannot reach key package hosts, and does not persist between sessions. | Phase 1 | Code may be written here, but Phase 1 stays "In Progress" until the checks run on a machine with the toolchain. |
| Q2 | Is the product named EduSuite or EMBASE Pro Suite? | Phase 1 | EduSuite |
| Q3 | Pilot segment: school or college? | Phase 3 | Model both, no segment-specific rules. |
| Q4 | Email or SMS provider for password reset and invitations? | Phase 2 | Console transport for development. |
| Q5 | Login identifier: email only, or also username or phone? | Phase 2 | Email |
| Q6 | Attendance rules: statuses, treatment of Late, percentage formula, correction window? | Phase 4 | Configurable. Defaults recorded when Phase 4 starts. |
| Q7 | Grading and result rules for the pilot institution? | Phase 5 | None. Phase 5 result calculation is blocked without them. |
| Q8 | Fee rules: refunds, waivers, late fees, overdue grace period? | Phase 5 | Append-only adjustments, no late fees. |
| Q9 | Approve a payment gateway (which one) or push notifications (FCM)? | Phase 5 / 6 | Not built. |
| Q10 | Hosting and database provider, and timing of legal review for real student data? | Phase 8 / pilot | Decide before Phase 8. |
