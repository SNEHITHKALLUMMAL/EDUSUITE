# EduSuite Decision Log

Records significant technical and product decisions.

**Status values**

- **Accepted:** directly required by the JSON specification, the SRS, or a documented fact. Changing it needs an explicit decision.
- **Proposed:** a reasoned default made in Phase 0. It becomes Accepted when confirmed or when the phase that depends on it starts. Revisit freely until then.
- **Open:** not yet decided. Listed so it is not forgotten.

Add new entries at the bottom. Do not rewrite history. Mark old entries Superseded and link the replacement.

---

## D-001: Source-of-truth priority
- **Status:** Accepted. **When:** Phase 0.
- **Decision:** Conflicts are resolved in this order: explicitly approved user requirements, the SRS, other project documentation, existing architecture and implementation, then documented defaults.
- **Reason:** Stated in the JSON `source_of_truth.priority_order`.
- **Consequences:** The Step 1 document informs risks and scope discipline but does not override the SRS or the JSON (see C-02, C-03, C-05).

## D-002: Product name in code and documentation
- **Status:** Proposed. **When:** Phase 0.
- **Decision:** Use **EduSuite** for repository, package and documentation names.
- **Reason:** The SRS and the JSON both use it. Only the Step 1 document uses "EMBASE Pro Suite".
- **Alternatives:** EMBASE Pro Suite.
- **Consequences:** Renaming later is cheap before Phase 1 and more tedious after. Confirm with the user (Q2).

## D-003: Monorepo layout
- **Status:** Accepted. **When:** Phase 0.
- **Decision:** One repository with `apps/api` (Node.js), `apps/mobile` (Flutter, including the web target), and `docs/`. A root `docker-compose.yml` for local services may be added in Phase 1.
- **Reason:** SRS sections 3.4 and 3.5 show these paths. A single repository is easiest for a solo developer.
- **Consequences:** The Flutter project is named `mobile` even though it also builds for web. This follows the SRS and is noted here to avoid confusion.

## D-004: Backend stack and style
- **Status:** Accepted. **When:** Phase 0.
- **Decision:** Node.js with JavaScript (not TypeScript) as ES modules, Express.js, REST under `/api/v1`, Zod for validation, OpenAPI for documentation, and a modular monolith. Each module separates routes, controllers, services, validation and database access.
- **Reason:** JSON `technology_stack.backend` and `architecture.backend`; SRS 2.5 and 3.5.
- **Alternatives:** Microservices (rejected by both documents for the MVP).
- **Consequences:** No static types on the API side. Tests and Zod schemas carry more of the correctness burden.

## D-005: Frontend stack and style
- **Status:** Accepted. **When:** Phase 0.
- **Decision:** Flutter and Dart, Riverpod for state, GoRouter for routing, Dio for HTTP, feature-based folders, centralized theme and configuration, no API URLs or secrets inside feature widgets.
- **Reason:** JSON `technology_stack.frontend`; SRS 3.4.
- **Consequences:** Package versions are chosen in Phase 1 and pinned.

## D-006: Database and data access
- **Status:** Accepted as the baseline. **When:** Phase 0.
- **Decision:** PostgreSQL with Prisma. Version-controlled migrations. Foreign keys, constraints and transactions. Decimal types for money.
- **Reason:** JSON `technology_stack.database`; SRS 3.2 and 6. The SRS says the choice must be confirmed before the first production migration.
- **Consequences:** Prisma's behaviour with Row-Level Security and connection pooling must be checked (G-14). Prisma tooling needs a network path to its engine downloads, which this sandbox lacks (R-04).

## D-007: Target platforms
- **Status:** Accepted. **When:** Phase 0.
- **Decision:** Android and web are the verified targets. The Flutter project is created so iOS can be added, but iOS is not built, tested or claimed to work.
- **Reason:** SRS NFR-PORT-001 and 002 limit the initial release to Android and web. The JSON lists iOS as a target. This reconciles the two (C-04). iOS builds need macOS and Xcode, which are unavailable (R-14).
- **Consequences:** Reports will say "Android and web" unless iOS has actually been built and tested.

## D-008: Tenancy model
- **Status:** Accepted for the model. Row-Level Security Proposed. **When:** Phase 0.
- **Decision:** A shared database where every institution-owned record carries an institution identifier. Tenant context is derived on the server from the authenticated user's verified membership, never from a client-supplied institution ID. Where a table references another tenant-owned table, prefer composite keys that include the institution, so the database rejects cross-tenant relationships (DR-005). PostgreSQL Row-Level Security is evaluated as an extra layer in a Phase 2 spike.
- **Reason:** SRS 3.3, 6.4, SEC-007..012; JSON `multi_tenant_security`.
- **Alternatives:** Schema-per-tenant or database-per-tenant (more isolation, much more operational work for one developer). The SRS lists stronger isolation as a future option.
- **Consequences:** Every query in a service must be tenant-scoped. Cross-tenant tests are mandatory from Phase 2 and are release-blocking.

## D-009: Identity, memberships and permissions
- **Status:** Proposed. **When:** Phase 0.
- **Decision:** Global `users` linked to institutions through `institution_memberships`. Super Admin is a platform-level capability separate from memberships. Permissions are data: `roles`, `permissions`, `role_permissions`, assigned through memberships. The six SRS roles are seeded.
- **Reason:** SRS 6.3 allows one user in several institutions (C-08). FR-RBAC-007 requires adding roles without redesign. JSON lists these entities.
- **Consequences:** Login identifiers are globally unique (G-04). The active institution is chosen at sign-in or switch and checked against membership on every request. Confirm in Phase 2.

## D-010: Data-model reconciliation
- **Status:** Accepted as process. **When:** Phase 0.
- **Decision:** Use the union of the SRS 6.2 entities and the JSON candidate entities as input. Produce an ERD before the first complex module (Phase 3 at the latest, with the identity core in Phase 2). Record resolved vocabulary (class, section, classroom, course) here.
- **Reason:** C-06; JSON requires an entity relationship model before complex modules.
- **Consequences:** The ERD is a Phase 2 and 3 deliverable and gets its own decision entry.

## D-011: Money and financial records
- **Status:** Accepted. **When:** Phase 0.
- **Decision:** Money is stored as `NUMERIC` and handled without floating point. Payments and adjustments are append-only. Recording a payment and updating the invoice balance happen in one transaction, with an idempotency key against duplicate submissions. Financial rows are never silently overwritten or deleted.
- **Reason:** SRS 6.5, FR-FEE-010, BR-007, BR-008, NFR-REL-003, UC-05.
- **Consequences:** Corrections are new rows, not edits. Reports derive balances from the records.

## D-012: Test and quality tooling
- **Status:** Proposed. **When:** Phase 0.
- **Decision:** Vitest and Supertest for the API, `flutter_test` for the app, ESLint and Prettier, `dart analyze` and `dart format`, and CI where practical.
- **Reason:** SRS 3.2 names "Vitest/Jest and Supertest". Vitest handles ES modules natively, which fits D-004.
- **Alternatives:** Jest (ESM support still needs extra setup).
- **Consequences:** Confirmed when Phase 1 sets up the toolchain.

## D-013: Deferred "should" and conditional items
- **Status:** Accepted. **When:** Phase 0.
- **Decision:** Do not build, until approved: CSV student import (FR-STU-011), report exports (FR-REP-009), multi-factor authentication (SEC-006), push notifications (FR-NOT-004), a payment gateway (FR-FEE-012), room allocation (JX-TIME-01), and notification preferences (JX-NOT-01). Build timetable conflict checks, because the JSON phase plan requires them.
- **Reason:** SRS uses "should" for these, the JSON marks them conditional or "when approved", and a solo developer must control scope.
- **Consequences:** Each needs an explicit approval and an entry here before work starts.

## D-014: Where Phase 1 is verified
- **Status:** Open. **When:** Phase 0.
- **Decision:** Not yet made. See question Q1 in `docs/PHASE0_AUDIT.md`.
- **Reason:** The Phase 0 sandbox lacks Flutter, Dart, PostgreSQL and Docker and cannot reach `pub.dev`, Flutter's artifact host or Prisma's engine host. It also does not persist between sessions.
- **Consequences:** Phase 1 cannot be reported complete until its exit checks have actually been run somewhere with the toolchain.

## D-015: Source documents kept in the repository
- **Status:** Accepted. **When:** Phase 0.
- **Decision:** The three input documents are stored unchanged in `docs/source/`.
- **Reason:** Later sessions must be able to read the exact SRS sections and JSON rules without relying on chat history.
- **Consequences:** Binary PDFs live in Git. They are small enough that this is acceptable.

## D-016: Platform operator access to tenant data
- **Status:** Proposed. **When:** Phase 0.
- **Decision:** Super Admin has no access to institution records by default. Any support access is an explicit, time-limited grant by the institution, fully audited. The mechanism is designed in Phase 2.
- **Reason:** SRS 2.3.1, BR-014, SEC-012. Step 1 asks for "limited, logged and justified" support access (G-05).
- **Consequences:** Platform operations that need tenant data cannot be done without the institution's involvement.

## D-017: Pilot segment and academic model
- **Status:** Open. **When:** Phase 0.
- **Decision:** Not yet made. Until the user chooses school or college, model both with a generic structure (academic year, department, course, class, section, subject) and build no segment-specific rules.
- **Reason:** Step 1 advises one pilot segment. The SRS (FR-ACAD-009) requires a configuration suitable for both (G-16, G-25).
- **Consequences:** Some screens may carry options a single segment never uses.
