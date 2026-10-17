# EduSuite Project State

Continuity file for development sessions. Update it at the end of every milestone.

- **Last updated:** 2026-10-17
- **Current phase:** Phase 0, Documentation and Repository Audit: **complete, awaiting review**
- **Current milestone:** none in progress
- **Next phase:** Phase 1, Foundation and Development Environment: **not started**

## Where things are

| File | Purpose |
|---|---|
| `docs/source/` | The three input documents, unchanged: JSON master prompt, SRS v1.0, Step 1 discovery document |
| `docs/PHASE0_AUDIT.md` | Inventory, conflicts (C-xx), gaps (G-xx), risks (R-xx), assumptions (A-xx), module mapping, questions (Q-xx) |
| `docs/IMPLEMENTATION_PLAN.md` | Plan for Phases 1 to 8 |
| `docs/IMPLEMENTATION_STATUS.md` | Per-requirement tracker against the SRS (237 requirements, 14 business rules, 16 acceptance criteria, 14 test scenarios, 23 JSON-spec additions) |
| `docs/DECISIONS.md` | Decision log (D-001 to D-017) |

## Completed

Phase 0 tasks:

- [x] Inspected the JSON specification, the SRS and the Step 1 document
- [x] Inspected the Git status and repository structure (no repository or project files existed; one was initialised)
- [x] Identified existing frontend, backend, database and deployment setup (none)
- [x] Created the requirements checklist (`IMPLEMENTATION_STATUS.md`)
- [x] Mapped requirements to modules and acceptance criteria (`PHASE0_AUDIT.md` section 5)
- [x] Identified conflicts, gaps, risks and assumptions (`PHASE0_AUDIT.md` sections 3 and 4)
- [x] Created `PROJECT_STATE.md`, `DECISIONS.md` and `IMPLEMENTATION_STATUS.md`
- [x] Produced the implementation plan

Application features completed: **none**. No application code exists.

## In progress

Nothing.

## Known issues and blockers

| Item | Detail |
|---|---|
| **Verification environment (R-04, D-014, Q1)** | The Phase 0 sandbox has no Flutter, Dart, PostgreSQL or Docker. It cannot reach `pub.dev`, `storage.googleapis.com` or `binaries.prisma.sh`. Its filesystem resets between tasks. Phase 1 exit criteria cannot be verified there. |
| Requirement gaps | 28 gaps and 12 conflicts are recorded. None blocks Phase 1. Several block later phases: attendance rules (Phase 4), grading and fee rules (Phase 5). See `PHASE0_AUDIT.md` section 6. |
| Business validation (R-01) | Step 1's go/no-go checkpoint is unmet. No customer segment, workflow or paying decision-maker is confirmed. |
| Real data | Do not load real student data until legal review (G-13). |

## Important architecture decisions

Full detail in `docs/DECISIONS.md`.

- Modular monolith. Node.js, plain JavaScript as ES modules, Express, REST under `/api/v1`, Zod, OpenAPI (D-004).
- Flutter and Dart with Riverpod, GoRouter and Dio, organised by feature (D-005).
- PostgreSQL with Prisma and version-controlled migrations (D-006).
- Android and web are the verified targets. iOS is not claimed (D-007).
- Shared database. Tenant context comes from verified membership, never from a client-supplied institution ID. Row-Level Security is a Phase 2 spike (D-008).
- Global users plus memberships. Super Admin has no default access to tenant data (D-009, D-016).
- Money is decimal. Payments and adjustments are append-only (D-011).
- Deferred until approved: CSV import, exports, MFA, push, payment gateway, room allocation, notification preferences (D-013).

## Commands to run and test

No application exists yet, so there are no project commands. Phase 1 will add them and this section must then be filled in.

Available now:

```bash
git status            # repository state
git log --oneline     # history
```

`IMPLEMENTATION_STATUS.md` was generated once from the SRS text in Phase 0 and is maintained by hand from now on.

## Next recommended task

1. Answer **Q1** (where Phase 1 will be built and verified), and confirm **Q2** (name: EduSuite).
2. Run Phase 1 per `docs/IMPLEMENTATION_PLAN.md` on a machine with Flutter, Node.js and PostgreSQL (or Docker), and run its exit checks.
3. In parallel, collect the inputs that later phases need: attendance rules (Q6), grading rules (Q7), fee rules (Q8), email provider (Q4), pilot segment (Q3).
