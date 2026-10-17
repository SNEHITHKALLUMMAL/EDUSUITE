# EduSuite Implementation Status

Tracks progress against the SRS (`docs/source/SOFTWARE_REQUIREMENTS_SPECIFICATION.pdf`) and the JSON master prompt (`docs/source/edusuite_master_prompt.json`).

- **Last updated:** 2026-10-17, end of Phase 0 (documentation and repository audit)
- **Allowed statuses:** Not Started, In Progress, Implemented, Tested, Blocked
- **Rule:** a row may be marked *Tested* only when an automated test or a recorded manual check has actually been run. Cite it in the "Code / tests" column.
- **Source:** requirement IDs and wording were extracted programmatically from the SRS PDF text layer (237 prefixed requirements, 14 business rules, 16 acceptance criteria; counts verified against the SRS). Wording is the SRS wording.
- **Level:** SRS keyword in the requirement text (Shall = mandatory, Should = recommended, May = optional).
- **Phase:** target JSON phase. `1+` or `2+` = starts in that phase and is re-checked in every later phase. `Conditional` = needs explicit approval before it is built. `Later increment` = SRS defers it past the first MVP increment.
- **Notes column references:** `G-xx` gaps, `C-xx` conflicts, `D-xx` decisions. See `docs/PHASE0_AUDIT.md` and `docs/DECISIONS.md`.

## Summary (Phase 0)

| Item | Count | Not Started | In Progress | Implemented | Tested | Blocked |
|---|---|---|---|---|---|---|
| SRS requirements (FR/NFR/SEC/DR/UIR/COM) | 237 | 237 | 0 | 0 | 0 | 0 |
| Business rules (BR) | 14 | 14 | 0 | 0 | 0 | 0 |
| Acceptance criteria (AC) | 16 | 16 | 0 | 0 | 0 | 0 |
| Test scenarios (TC) | 14 | 14 | 0 | 0 | 0 | 0 |
| JSON-spec additions (JX) | 23 | 23 | 0 | 0 | 0 | 0 |

Requirements by target phase (SRS FR/NFR/SEC/DR/UIR/COM only):

| Target phase | Count |
|---|---|
| 1 | 15 |
| 1+ | 26 |
| 1-2 | 6 |
| 2 | 40 |
| 2+ | 5 |
| 3 | 43 |
| 4 | 20 |
| 4-5 | 2 |
| 5 | 22 |
| 6 | 25 |
| 7 | 9 |
| 7 (if required) | 1 |
| 8 | 13 |
| 8 (ongoing) | 6 |
| Conditional | 2 |
| Later increment | 1 |
| Later (should) | 1 |

## 1. SRS requirements

### Authentication and user management (SRS 4.1) (12)

| ID | Requirement | Level | Module | Phase | Status | Code / tests | Outstanding work |
|---|---|---|---|---|---|---|---|
| FR-AUTH-001 | The system shall allow authorized users to sign in using their configured credentials. | Shall | Authentication | 2 | Not Started | - | Login identifier strategy undecided (G-04). |
| FR-AUTH-002 | The system shall validate credentials on the backend before issuing an authenticated session. | Shall | Authentication | 2 | Not Started | - | - |
| FR-AUTH-003 | The system shall support secure password hashing and shall never store plaintext passwords. | Shall | Authentication | 2 | Not Started | - | - |
| FR-AUTH-004 | The system shall support logout and invalidation of the corresponding refresh session. | Shall | Authentication | 2 | Not Started | - | - |
| FR-AUTH-005 | The system shall support password reset through a verified recovery mechanism. | Shall | Authentication | 2 | Not Started | - | Needs email/SMS provider decision (G-03); dev-only transport until then. |
| FR-AUTH-006 | The system shall provide an authenticated endpoint for retrieving the current user's profile and permissions. | Shall | Authentication | 2 | Not Started | - | - |
| FR-AUTH-007 | The system shall support account activation, suspension and deactivation. | Shall | Authentication | 2 | Not Started | - | - |
| FR-AUTH-008 | The system shall apply role-based authorization to protected operations. | Shall | Authentication | 2 | Not Started | - | - |
| FR-AUTH-009 | The system shall prevent suspended users from obtaining new authenticated sessions. | Shall | Authentication | 2 | Not Started | - | - |
| FR-AUTH-010 | The system shall allow authorized administrators to invite users and assign institutional roles. | Shall | Authentication | 2 | Not Started | - | Invite delivery channel (G-03) and login identifier (G-04). |
| FR-AUTH-011 | The system shall maintain session expiry and refresh-token rotation policies. | Shall | Authentication | 2 | Not Started | - | - |
| FR-AUTH-012 | The system shall record security-relevant authentication events without logging passwords, tokens or other secrets. | Shall | Authentication | 2 | Not Started | - | - |

### Institution management (SRS 4.2) (8)

| ID | Requirement | Level | Module | Phase | Status | Code / tests | Outstanding work |
|---|---|---|---|---|---|---|---|
| FR-INS-001 | The system shall allow the Super Admin to create a new institution. | Shall | Institution management | 2 | Not Started | - | - |
| FR-INS-002 | The system shall maintain institution name, identifier, address, contact information and status. | Shall | Institution management | 2 | Not Started | - | - |
| FR-INS-003 | The system shall allow authorized institutional administrators to update institution information. | Shall | Institution management | 3 | Not Started | - | - |
| FR-INS-004 | The system shall support institution-specific academic years, departments and organizational settings. | Shall | Institution management | 3 | Not Started | - | - |
| FR-INS-005 | The system shall allow the Super Admin to activate, suspend or deactivate an institution. | Shall | Institution management | 2 | Not Started | - | - |
| FR-INS-006 | The system shall prevent suspended institutions from performing restricted operational activities. | Shall | Institution management | 2 | Not Started | - | - |
| FR-INS-007 | The system shall ensure that one institution cannot access another institution's private records. | Shall | Institution management | 2 | Not Started | - | Release-blocking if violated. Cross-tenant tests required (TC005). |
| FR-INS-008 | The system shall support configurable institutional time zones and academic calendars. | Shall | Institution management | 3 | Not Started | - | Academic calendar semantics undefined (G-16). |

### Role and permission management (SRS 4.3) (7)

| ID | Requirement | Level | Module | Phase | Status | Code / tests | Outstanding work |
|---|---|---|---|---|---|---|---|
| FR-RBAC-001 | The system shall support the roles Super Admin, Institution Admin, Teacher, Student, Parent/Guardian and Accountant. | Shall | Authorization (RBAC) | 2 | Not Started | - | - |
| FR-RBAC-002 | The system shall assign permissions according to the authenticated user's role and institutional membership. | Shall | Authorization (RBAC) | 2 | Not Started | - | - |
| FR-RBAC-003 | The backend shall validate authorization for every protected operation. | Shall | Authorization (RBAC) | 2 | Not Started | - | - |
| FR-RBAC-004 | The system shall restrict users to records and functions authorized for their roles. | Shall | Authorization (RBAC) | 2 | Not Started | - | - |
| FR-RBAC-005 | The system shall allow authorized administrators to change user roles within their permitted administrative scope. | Shall | Authorization (RBAC) | 2 | Not Started | - | Meaning of "permitted administrative scope" to define (G-05). |
| FR-RBAC-006 | The system shall audit sensitive role and permission changes. | Shall | Authorization (RBAC) | 2 | Not Started | - | - |
| FR-RBAC-007 | The system shall support future addition of roles and granular permissions. | Shall | Authorization (RBAC) | 2 | Not Started | - | Roles/permissions table design (D-009). |

### Student admission and management (SRS 4.4) (12)

| ID | Requirement | Level | Module | Phase | Status | Code / tests | Outstanding work |
|---|---|---|---|---|---|---|---|
| FR-STU-001 | The system shall allow authorized users to create student records. | Shall | Student management | 3 | Not Started | - | - |
| FR-STU-002 | Each student shall have a unique student identifier. | Shall | Student management | 3 | Not Started | - | - |
| FR-STU-003 | The system shall support institution-specific admission numbers. | Shall | Student management | 3 | Not Started | - | - |
| FR-STU-004 | The system shall store student name, date of birth where required, contact information, enrollment status and other configured profile fields. | Shall | Student management | 3 | Not Started | - | "Configured profile fields": fixed schema vs configurable fields undefined (G-16). |
| FR-STU-005 | The system shall support department, class and academic-year enrollment. | Shall | Student management | 3 | Not Started | - | - |
| FR-STU-006 | The system shall allow authorized users to update student records. | Shall | Student management | 3 | Not Started | - | - |
| FR-STU-007 | The system shall support student search by name, admission number and applicable academic filters. | Shall | Student management | 3 | Not Started | - | - |
| FR-STU-008 | The system shall support pagination when listing student records. | Shall | Student management | 3 | Not Started | - | - |
| FR-STU-009 | The system shall support transfer, graduation, withdrawal or other configured enrollment status changes. | Shall | Student management | 3 | Not Started | - | Enrollment-status vocabulary to confirm. |
| FR-STU-010 | The system shall prevent unauthorized deletion or disclosure of student records. | Shall | Student management | 3 | Not Started | - | Deletion and retention policy undefined (G-07). |
| FR-STU-011 | The system should support importing student information from validated CSV files in a later MVP increment. | Should | Student management | Later increment | Not Started | - | SRS "should"; later MVP increment; not planned unless approved (D-013). |
| FR-STU-012 | The system shall maintain a record of significant changes to student data. | Shall | Student management | 3 | Not Started | - | - |

### Staff and teacher management (SRS 4.5) (7)

| ID | Requirement | Level | Module | Phase | Status | Code / tests | Outstanding work |
|---|---|---|---|---|---|---|---|
| FR-STAFF-001 | The system shall allow authorized administrators to create staff records. | Shall | Staff management | 3 | Not Started | - | - |
| FR-STAFF-002 | The system shall store employee identifier, name, contact information, job role and employment status. | Shall | Staff management | 3 | Not Started | - | - |
| FR-STAFF-003 | The system shall support department and subject assignments. | Shall | Staff management | 3 | Not Started | - | - |
| FR-STAFF-004 | The system shall allow authorized administrators to assign teachers to classes and subjects. | Shall | Staff management | 3 | Not Started | - | - |
| FR-STAFF-005 | The system shall allow staff to view their permitted profiles and assignments. | Shall | Staff management | 3 | Not Started | - | - |
| FR-STAFF-006 | The system shall restrict teacher access to assigned academic resources unless additional permissions are granted. | Shall | Staff management | 3 | Not Started | - | - |
| FR-STAFF-007 | The system shall maintain appropriate audit records for changes to staff assignments and permissions. | Shall | Staff management | 3 | Not Started | - | - |

### Academic management (SRS 4.6) (9)

| ID | Requirement | Level | Module | Phase | Status | Code / tests | Outstanding work |
|---|---|---|---|---|---|---|---|
| FR-ACAD-001 | The system shall support the creation of academic years. | Shall | Academic management | 3 | Not Started | - | - |
| FR-ACAD-002 | The system shall support institution-specific departments and courses. | Shall | Academic management | 3 | Not Started | - | - |
| FR-ACAD-003 | The system shall support the creation and maintenance of classes or sections. | Shall | Academic management | 3 | Not Started | - | - |
| FR-ACAD-004 | The system shall support subjects and subject-to-class associations. | Shall | Academic management | 3 | Not Started | - | - |
| FR-ACAD-005 | The system shall support teacher assignments to subjects and classes. | Shall | Academic management | 3 | Not Started | - | - |
| FR-ACAD-006 | The system shall support student enrollment into classes for a specified academic period. | Shall | Academic management | 3 | Not Started | - | - |
| FR-ACAD-007 | The system shall prevent invalid or conflicting enrollment relationships. | Shall | Academic management | 3 | Not Started | - | "Conflicting enrollment" rules to define (G-17). |
| FR-ACAD-008 | The system shall preserve historical enrollment information when a student changes class or academic year. | Shall | Academic management | 3 | Not Started | - | - |
| FR-ACAD-009 | The system shall support academic configuration appropriate to different schools and colleges. | Shall | Academic management | 3 | Not Started | - | Pilot segment (school vs college) undecided (G-25). |

### Attendance management (SRS 4.7) (13)

| ID | Requirement | Level | Module | Phase | Status | Code / tests | Outstanding work |
|---|---|---|---|---|---|---|---|
| FR-ATT-001 | The system shall allow authorized teachers to create attendance sessions for assigned classes. | Shall | Attendance | 4 | Not Started | - | - |
| FR-ATT-002 | The system shall allow authorized users to record attendance using configured statuses such as Present, Absent and Late. | Shall | Attendance | 4 | Not Started | - | Status set beyond Present/Absent/Late (e.g. Excused) undefined (G-02). |
| FR-ATT-003 | The system shall associate attendance sessions with the institution, class, date and applicable subject or period. | Shall | Attendance | 4 | Not Started | - | - |
| FR-ATT-004 | The system shall associate attendance records with individual students. | Shall | Attendance | 4 | Not Started | - | - |
| FR-ATT-005 | The system shall prevent duplicate attendance records for the same student within the same session. | Shall | Attendance | 4 | Not Started | - | - |
| FR-ATT-006 | The system shall allow authorized users to correct attendance records according to institutional policy. | Shall | Attendance | 4 | Not Started | - | Correction policy and time window undefined (G-02). |
| FR-ATT-007 | The system shall record the user and timestamp associated with attendance changes. | Shall | Attendance | 4 | Not Started | - | - |
| FR-ATT-008 | The system shall calculate attendance summaries over a specified period. | Shall | Attendance | 4 | Not Started | - | - |
| FR-ATT-009 | The system shall allow students to view their authorized attendance history. | Shall | Attendance | 4 | Not Started | - | - |
| FR-ATT-010 | The system shall allow guardians to view attendance information for linked students where permitted. | Shall | Attendance | 4 | Not Started | - | - |
| FR-ATT-011 | The system shall support attendance reports by student, class, subject and date range. | Shall | Attendance | 4 | Not Started | - | - |
| FR-ATT-012 | The system shall calculate attendance percentages according to an institution-configurable rule. | Shall | Attendance | 4 | Not Started | - | Percentage rule undefined (G-02). |
| FR-ATT-013 | The system shall use the institution's configured time zone when determining attendance dates. | Shall | Attendance | 4 | Not Started | - | - |

### Timetable management (SRS 4.8) (7)

| ID | Requirement | Level | Module | Phase | Status | Code / tests | Outstanding work |
|---|---|---|---|---|---|---|---|
| FR-TIME-001 | The system shall allow authorized users to create timetable entries. | Shall | Timetable | 4 | Not Started | - | - |
| FR-TIME-002 | Each timetable entry shall support class, subject, teacher, day, start time and end time. | Shall | Timetable | 4 | Not Started | - | - |
| FR-TIME-003 | The system shall validate that the end time is later than the start time. | Shall | Timetable | 4 | Not Started | - | - |
| FR-TIME-004 | The system should identify teacher and class scheduling conflicts. | Should | Timetable | 4 | Not Started | - | SRS "should"; JSON requires conflict checks, so implement (C-09). Rooms optional (C-11). |
| FR-TIME-005 | The system shall allow teachers to view their assigned timetable. | Shall | Timetable | 4 | Not Started | - | - |
| FR-TIME-006 | The system shall allow students to view their class timetable. | Shall | Timetable | 4 | Not Started | - | - |
| FR-TIME-007 | The system shall support updates to timetable entries while preserving appropriate change history. | Shall | Timetable | 4 | Not Started | - | - |

### Fee management (SRS 4.9) (13)

| ID | Requirement | Level | Module | Phase | Status | Code / tests | Outstanding work |
|---|---|---|---|---|---|---|---|
| FR-FEE-001 | The system shall allow authorized financial users to configure fee categories and structures. | Shall | Fee management | 5 | Not Started | - | - |
| FR-FEE-002 | The system shall allow authorized users to generate fee invoices for students. | Shall | Fee management | 5 | Not Started | - | - |
| FR-FEE-003 | Each invoice shall contain an identifier, student reference, institution, issue date, due date, amount and payment status. | Shall | Fee management | 5 | Not Started | - | - |
| FR-FEE-004 | The system shall calculate invoice balances using recorded financial transactions. | Shall | Fee management | 5 | Not Started | - | - |
| FR-FEE-005 | The system shall support fee statuses such as Unpaid, Partially Paid, Paid and Overdue. | Shall | Fee management | 5 | Not Started | - | Overdue: computed vs stored; grace period (G-09). |
| FR-FEE-006 | The system shall allow authorized users to record verified payments. | Shall | Fee management | 5 | Not Started | - | - |
| FR-FEE-007 | The system shall prevent unauthorized alteration or deletion of financial records. | Shall | Fee management | 5 | Not Started | - | - |
| FR-FEE-008 | The system shall support fee reports by class, student, date range and payment status. | Shall | Fee management | 5 | Not Started | - | - |
| FR-FEE-009 | The system shall allow authorized students and guardians to view relevant invoices and payment status. | Shall | Fee management | 5 | Not Started | - | - |
| FR-FEE-010 | The system shall use database transactions when recording payments and updating invoice balances. | Shall | Fee management | 5 | Not Started | - | - |
| FR-FEE-011 | The system shall maintain an audit trail of invoice creation, payment recording, refunds and adjustments. | Shall | Fee management | 5 | Not Started | - | Refund/adjustment rules and roles undefined (G-08). |
| FR-FEE-012 | If an online payment gateway is integrated, the system shall verify payment notifications on the backend before marking invoices as paid. | Shall | Fee management | Conditional | Not Started | - | Gateway not selected or approved (G-10). |
| FR-FEE-013 | The system shall support a configured institutional currency. | Shall | Fee management | 5 | Not Started | - | No default currency specified; per-institution setting. |

### Examination and result management (SRS 4.10) (10)

| ID | Requirement | Level | Module | Phase | Status | Code / tests | Outstanding work |
|---|---|---|---|---|---|---|---|
| FR-EXAM-001 | The system shall allow authorized users to create examination records. | Shall | Examination management | 5 | Not Started | - | - |
| FR-EXAM-002 | Each examination shall be associated with the relevant institution and academic context. | Shall | Examination management | 5 | Not Started | - | - |
| FR-EXAM-003 | The system shall support examination schedules and subject-level marks. | Shall | Examination management | 5 | Not Started | - | - |
| FR-EXAM-004 | The system shall validate marks against configured maximum and minimum values. | Shall | Examination management | 5 | Not Started | - | - |
| FR-EXAM-005 | The system shall allow authorized teachers or exam administrators to enter marks. | Shall | Examination management | 5 | Not Started | - | - |
| FR-EXAM-006 | The system shall support draft, reviewed and published result states. | Shall | Examination management | 5 | Not Started | - | Result state machine (can a published result be unpublished?) undefined (G-01). |
| FR-EXAM-007 | The system shall prevent students from accessing unpublished results. | Shall | Examination management | 5 | Not Started | - | - |
| FR-EXAM-008 | The system shall allow students to view their published results. | Shall | Examination management | 5 | Not Started | - | - |
| FR-EXAM-009 | The system shall support authorized result reports. | Shall | Examination management | 5 | Not Started | - | Result calculation and grading rules undefined (G-01). |
| FR-EXAM-010 | The system shall maintain an audit trail of marks changes and result publication. | Shall | Examination management | 5 | Not Started | - | - |

### Notifications and announcements (SRS 4.11) (7)

| ID | Requirement | Level | Module | Phase | Status | Code / tests | Outstanding work |
|---|---|---|---|---|---|---|---|
| FR-NOT-001 | The system shall allow authorized administrators to publish institutional announcements. | Shall | Notifications | 6 | Not Started | - | - |
| FR-NOT-002 | The system shall support announcements targeted to permitted user groups. | Shall | Notifications | 6 | Not Started | - | - |
| FR-NOT-003 | The system shall display relevant announcements within the application. | Shall | Notifications | 6 | Not Started | - | - |
| FR-NOT-004 | The system should support push notifications through a configured provider. | Should | Notifications | Conditional | Not Started | - | FCM/push not approved (G-11). |
| FR-NOT-005 | The system shall record notification delivery or processing status where the selected provider exposes it. | Shall | Notifications | 6 | Not Started | - | - |
| FR-NOT-006 | The system shall not expose private student information through unauthorized notifications. | Shall | Notifications | 6 | Not Started | - | - |
| FR-NOT-007 | The system shall allow users to view notification history. | Shall | Notifications | 6 | Not Started | - | - |

### Parent and guardian management (SRS 4.12) (6)

| ID | Requirement | Level | Module | Phase | Status | Code / tests | Outstanding work |
|---|---|---|---|---|---|---|---|
| FR-PARENT-001 | The system shall support verified links between guardian accounts and student records. | Shall | Guardian management | 3 | Not Started | - | What counts as "verified" is undefined (G-06). |
| FR-PARENT-002 | The system shall allow a guardian to access information only for linked students. | Shall | Guardian management | 3 | Not Started | - | - |
| FR-PARENT-003 | The system shall support multiple authorized guardians per student where institutional policy permits. | Shall | Guardian management | 3 | Not Started | - | - |
| FR-PARENT-004 | The system shall support a guardian linked to multiple students. | Shall | Guardian management | 3 | Not Started | - | - |
| FR-PARENT-005 | The system shall allow authorized administrators to create, verify and revoke guardian relationships. | Shall | Guardian management | 3 | Not Started | - | - |
| FR-PARENT-006 | The system shall audit changes to guardian relationships. | Shall | Guardian management | 3 | Not Started | - | - |

### Reports and dashboard (SRS 4.13) (9)

| ID | Requirement | Level | Module | Phase | Status | Code / tests | Outstanding work |
|---|---|---|---|---|---|---|---|
| FR-REP-001 | The system shall provide role-appropriate dashboards. | Shall | Reporting and dashboards | 7 | Not Started | - | - |
| FR-REP-002 | The administrator dashboard shall display authorized summaries of students, staff, classes and attendance. | Shall | Reporting and dashboards | 7 | Not Started | - | - |
| FR-REP-003 | The teacher dashboard shall display assigned classes, timetable information and permitted attendance tasks. | Shall | Reporting and dashboards | 7 | Not Started | - | - |
| FR-REP-004 | The student dashboard shall display relevant academic information and announcements. | Shall | Reporting and dashboards | 7 | Not Started | - | - |
| FR-REP-005 | The guardian dashboard shall display information for linked students. | Shall | Reporting and dashboards | 7 | Not Started | - | - |
| FR-REP-006 | The accountant dashboard shall display authorized invoice, collection and outstanding-balance summaries. | Shall | Reporting and dashboards | 7 | Not Started | - | - |
| FR-REP-007 | The system shall support date-range and class-based filtering for applicable reports. | Shall | Reporting and dashboards | 7 | Not Started | - | - |
| FR-REP-008 | The system shall ensure reports respect the user's institutional scope and permissions. | Shall | Reporting and dashboards | 7 | Not Started | - | - |
| FR-REP-009 | The system should support CSV or PDF report export in a subsequent MVP increment. | Should | Reporting and dashboards | 7 (if required) | Not Started | - | SRS "should"; JSON: exports only where required (C-09). |

### Search and data access (SRS 4.14) (5)

| ID | Requirement | Level | Module | Phase | Status | Code / tests | Outstanding work |
|---|---|---|---|---|---|---|---|
| FR-SEARCH-001 | The system shall provide search for supported entities. | Shall | Search and data access | 3 | Not Started | - | - |
| FR-SEARCH-002 | Search results shall be restricted to records the user is authorized to access. | Shall | Search and data access | 3 | Not Started | - | - |
| FR-SEARCH-003 | The system shall support pagination for large result sets. | Shall | Search and data access | 3 | Not Started | - | - |
| FR-SEARCH-004 | The system shall validate sorting and filtering parameters. | Shall | Search and data access | 3 | Not Started | - | - |
| FR-SEARCH-005 | The system should support configurable page sizes and sensible maximum limits. | Should | Search and data access | 3 | Not Started | - | Default and maximum page sizes to define (G-24). |

### Cloudinary file management (SRS 4.15) (12)

| ID | Requirement | Level | Module | Phase | Status | Code / tests | Outstanding work |
|---|---|---|---|---|---|---|---|
| FR-FILE-001 | The system shall use Cloudinary to store and deliver supported uploaded media and files. | Shall | Cloudinary file management | 6 | Not Started | - | - |
| FR-FILE-002 | The system shall support profile images and institution branding assets. | Shall | Cloudinary file management | 6 | Not Started | - | - |
| FR-FILE-003 | The system may support documents and other institutional files subject to the configured Cloudinary account capabilities and file policies. | May | Cloudinary file management | 6 | Not Started | - | Depends on Cloudinary plan and private-delivery verification (G-12). |
| FR-FILE-004 | The system shall validate file type, size and upload authorization. | Shall | Cloudinary file management | 6 | Not Started | - | - |
| FR-FILE-005 | The system shall store relevant asset identifiers, ownership, institution association and metadata in the SQL database. | Shall | Cloudinary file management | 6 | Not Started | - | - |
| FR-FILE-006 | The system shall not store Cloudinary API secrets in the Flutter client. | Shall | Cloudinary file management | 6 | Not Started | - | - |
| FR-FILE-007 | The backend shall authorize uploads and issue appropriately restricted upload instructions or signed upload parameters. | Shall | Cloudinary file management | 6 | Not Started | - | - |
| FR-FILE-008 | The system shall restrict access to private files through supported access controls and authorized delivery mechanisms. | Shall | Cloudinary file management | 6 | Not Started | - | - |
| FR-FILE-009 | The system shall support updating or removing file references when authorized users replace or delete assets. | Shall | Cloudinary file management | 6 | Not Started | - | - |
| FR-FILE-010 | The system shall handle failed uploads without creating invalid or orphaned database references. | Shall | Cloudinary file management | 6 | Not Started | - | - |
| FR-FILE-011 | The system shall record relevant upload and deletion events. | Shall | Cloudinary file management | 6 | Not Started | - | - |
| FR-FILE-012 | The system shall not assume that Cloudinary provides the same access controls for every file type or delivery configuration; private document handling shall be verified before production use. | Shall | Cloudinary file management | 6 | Not Started | - | Must be verified with a real account before production (G-12). |

### Audit and administrative logging (SRS 4.16) (5)

| ID | Requirement | Level | Module | Phase | Status | Code / tests | Outstanding work |
|---|---|---|---|---|---|---|---|
| FR-AUDIT-001 | The system shall record significant security and administrative events. | Shall | Audit logging | 2 | Not Started | - | - |
| FR-AUDIT-002 | Audit records shall identify the actor, institution where applicable, action, affected entity and timestamp. | Shall | Audit logging | 2 | Not Started | - | - |
| FR-AUDIT-003 | Audit logs shall not store plaintext passwords, session secrets or unnecessary sensitive information. | Shall | Audit logging | 2 | Not Started | - | - |
| FR-AUDIT-004 | Access to audit logs shall be restricted to authorized personnel. | Shall | Audit logging | 2 | Not Started | - | - |
| FR-AUDIT-005 | Sensitive audit records shall be protected against unauthorized modification. | Shall | Audit logging | 2 | Not Started | - | Immutability mechanism undefined (G-22). |

### User interface requirements (SRS 5.1) (10)

| ID | Requirement | Level | Module | Phase | Status | Code / tests | Outstanding work |
|---|---|---|---|---|---|---|---|
| UIR-001 | The system shall provide a consistent interface across supported platforms. | Shall | Frontend UX (cross-cutting) | 1+ | Not Started | - | - |
| UIR-002 | The mobile interface shall be optimized for smartphone and tablet screens. | Shall | Frontend UX (cross-cutting) | 1+ | Not Started | - | - |
| UIR-003 | The web interface shall provide responsive layouts suitable for desktop browsers. | Shall | Frontend UX (cross-cutting) | 1+ | Not Started | - | - |
| UIR-004 | Navigation and available features shall reflect the user's role and permissions. | Shall | Frontend UX (cross-cutting) | 1+ | Not Started | - | - |
| UIR-005 | The system shall provide clear loading, success, empty, validation and error states. | Shall | Frontend UX (cross-cutting) | 1+ | Not Started | - | - |
| UIR-006 | The system shall provide understandable form validation messages. | Shall | Frontend UX (cross-cutting) | 1+ | Not Started | - | - |
| UIR-007 | The interface should support keyboard navigation and accessible labels. | Should | Frontend UX (cross-cutting) | 1+ | Not Started | - | - |
| UIR-008 | Important actions such as deleting records, publishing results and reversing financial transactions shall require appropriate confirmation or authorization. | Shall | Frontend UX (cross-cutting) | 1+ | Not Started | - | - |
| UIR-009 | The system should provide consistent date, time and currency formatting according to institutional settings. | Should | Frontend UX (cross-cutting) | 1+ | Not Started | - | - |
| UIR-010 | The system should target WCAG 2.2 Level AA for the web interface where applicable. | Should | Frontend UX (cross-cutting) | 1+ | Not Started | - | WCAG 2.2 AA verification approach for Flutter web TBD. |

### Communication interfaces (SRS 5.3) (6)

| ID | Requirement | Level | Module | Phase | Status | Code / tests | Outstanding work |
|---|---|---|---|---|---|---|---|
| COM-001 | Production API communication shall use HTTPS. | Shall | API platform | 8 | Not Started | - | Production HTTPS depends on hosting choice (G-21). |
| COM-002 | API requests and responses shall use UTF-8 encoded JSON unless another documented content type is required. | Shall | API platform | 1 | Not Started | - | - |
| COM-003 | The server shall reject unsupported content types on endpoints that accept structured JSON. | Shall | API platform | 1 | Not Started | - | - |
| COM-004 | The server shall enforce request-size limits. | Shall | API platform | 1 | Not Started | - | - |
| COM-005 | The backend shall validate permitted browser origins for cross-origin requests. | Shall | API platform | 1 | Not Started | - | - |
| COM-006 | The system shall handle network failures without falsely reporting successful data operations. | Shall | API platform | 1 | Not Started | - | - |

### Database requirements: tenant data integrity (SRS 6.4) (10)

| ID | Requirement | Level | Module | Phase | Status | Code / tests | Outstanding work |
|---|---|---|---|---|---|---|---|
| DR-001 | Every institution-owned entity shall include or be traceably associated with an institution identifier. | Shall | Database | 2+ | Not Started | - | - |
| DR-002 | The database shall enforce referential integrity using foreign keys. | Shall | Database | 2+ | Not Started | - | - |
| DR-003 | Institution-scoped unique constraints shall be used where identifiers are unique only within an institution. | Shall | Database | 2+ | Not Started | - | - |
| DR-004 | Queries shall enforce tenant context and applicable authorization rules. | Shall | Database | 2+ | Not Started | - | - |
| DR-005 | Relationships between records from different institutions shall be rejected unless an explicitly authorized platform-level operation permits them. | Shall | Database | 2+ | Not Started | - | - |
| DR-006 | Critical financial and attendance updates shall use database transactions. | Shall | Database | 4-5 | Not Started | - | - |
| DR-007 | Database migrations shall be version-controlled and tested before production deployment. | Shall | Database | 1 | Not Started | - | - |
| DR-008 | The database shall use indexes for frequently searched and joined fields. | Shall | Database | 1+ | Not Started | - | - |
| DR-009 | Backups shall be created and restoration procedures tested. | Shall | Database | 8 | Not Started | - | Backup provider undecided (G-21). |
| DR-010 | Sensitive personal information shall be limited to what the system needs. | Shall | Database | 3 | Not Started | - | - |

### Performance (SRS 7.1) (5)

| ID | Requirement | Level | Module | Phase | Status | Code / tests | Outstanding work |
|---|---|---|---|---|---|---|---|
| NFR-PERF-001 | Under the defined normal operating load, at least 95% of ordinary API read requests should complete within 2 seconds, excluding external service latency and large report generation. | Should | Performance | 8 | Not Started | - | "Normal operating load" undefined (G-26). |
| NFR-PERF-002 | Standard interactive operations should provide visible feedback promptly. | Should | Performance | 8 | Not Started | - | - |
| NFR-PERF-003 | The system shall use pagination for potentially large collections. | Shall | Performance | 3 | Not Started | - | - |
| NFR-PERF-004 | The system shall optimize common queries using appropriate indexes and query analysis. | Shall | Performance | 8 | Not Started | - | - |
| NFR-PERF-005 | Performance testing shall use a documented workload and test environment. | Shall | Performance | 8 | Not Started | - | - |

### Scalability (SRS 7.2) (5)

| ID | Requirement | Level | Module | Phase | Status | Code / tests | Outstanding work |
|---|---|---|---|---|---|---|---|
| NFR-SCAL-001 | The backend shall support horizontal scaling of stateless application instances. | Shall | Scalability | 1+ | Not Started | - | - |
| NFR-SCAL-002 | The database design shall support growth in institutions, students, staff and attendance records. | Shall | Scalability | 1+ | Not Started | - | - |
| NFR-SCAL-003 | Uploaded media shall be handled through Cloudinary rather than being stored directly in the application server's local filesystem. | Shall | Scalability | 6 | Not Started | - | - |
| NFR-SCAL-004 | Expensive reporting tasks should be designed so they can be moved to background processing as usage increases. | Should | Scalability | 7 | Not Started | - | - |
| NFR-SCAL-005 | The system shall support adding new modules without requiring a complete architectural redesign. | Shall | Scalability | 1+ | Not Started | - | - |

### Availability and reliability (SRS 7.3) (5)

| ID | Requirement | Level | Module | Phase | Status | Code / tests | Outstanding work |
|---|---|---|---|---|---|---|---|
| NFR-REL-001 | The production system should target an initial service availability of 99.5% per month, excluding explicitly defined maintenance and third-party outages. | Should | Reliability | 1+ | Not Started | - | Target subject to hosting choice (G-21). |
| NFR-REL-002 | The system shall handle temporary database and network failures gracefully. | Shall | Reliability | 1+ | Not Started | - | - |
| NFR-REL-003 | Failed transactions shall not leave partially updated critical financial or attendance records. | Shall | Reliability | 4-5 | Not Started | - | - |
| NFR-REL-004 | The application shall provide useful error messages without exposing stack traces or internal secrets. | Shall | Reliability | 1+ | Not Started | - | - |
| NFR-REL-005 | Critical operations shall be safe against duplicate submissions where practical. | Shall | Reliability | 1+ | Not Started | - | - |

### Usability (SRS 7.4) (5)

| ID | Requirement | Level | Module | Phase | Status | Code / tests | Outstanding work |
|---|---|---|---|---|---|---|---|
| NFR-USE-001 | The interface shall be understandable to users with basic digital literacy. | Shall | Usability | 1+ | Not Started | - | - |
| NFR-USE-002 | Common operations such as taking attendance shall require a minimal number of unnecessary navigation steps. | Shall | Usability | 1+ | Not Started | - | - |
| NFR-USE-003 | Form fields shall have clear labels and validation messages. | Shall | Usability | 1+ | Not Started | - | - |
| NFR-USE-004 | The system should support consistent design patterns across modules. | Should | Usability | 1+ | Not Started | - | - |
| NFR-USE-005 | The system should be evaluated with representative users before production release. | Should | Usability | 8 | Not Started | - | - |

### Maintainability (SRS 7.5) (6)

| ID | Requirement | Level | Module | Phase | Status | Code / tests | Outstanding work |
|---|---|---|---|---|---|---|---|
| NFR-MAIN-001 | The backend shall use modular code organization. | Shall | Maintainability | 1 | Not Started | - | - |
| NFR-MAIN-002 | The Flutter client shall separate presentation, application state and API access. | Shall | Maintainability | 1 | Not Started | - | - |
| NFR-MAIN-003 | Public API contracts shall be documented. | Shall | Maintainability | 1 | Not Started | - | - |
| NFR-MAIN-004 | Database schema changes shall use version-controlled migrations. | Shall | Maintainability | 1 | Not Started | - | - |
| NFR-MAIN-005 | Business rules shall not be duplicated unnecessarily across client and server. | Shall | Maintainability | 1 | Not Started | - | - |
| NFR-MAIN-006 | Automated tests shall cover critical business rules and security boundaries. | Shall | Maintainability | 1 | Not Started | - | - |

### Portability (SRS 7.6) (4)

| ID | Requirement | Level | Module | Phase | Status | Code / tests | Outstanding work |
|---|---|---|---|---|---|---|---|
| NFR-PORT-001 | The Flutter application shall target Android and web in the initial release. | Shall | Portability | 1 | Not Started | - | - |
| NFR-PORT-002 | The architecture shall support adding iOS deployment after platform-specific validation. | Shall | Portability | 1 | Not Started | - | iOS cannot be built or verified without macOS/Xcode (D-007). |
| NFR-PORT-003 | The backend shall be deployable to compatible Node.js hosting environments. | Shall | Portability | 1 | Not Started | - | - |
| NFR-PORT-004 | Deployment shall not depend on undocumented manual changes to the production server. | Shall | Portability | 8 | Not Started | - | - |

### Accessibility (SRS 7.7) (4)

| ID | Requirement | Level | Module | Phase | Status | Code / tests | Outstanding work |
|---|---|---|---|---|---|---|---|
| NFR-ACC-001 | The web interface should conform to WCAG 2.2 Level AA where applicable. | Should | Accessibility | 1+ | Not Started | - | See UIR-010. |
| NFR-ACC-002 | Controls shall expose meaningful labels and focus states. | Shall | Accessibility | 1+ | Not Started | - | - |
| NFR-ACC-003 | Color shall not be the only method of communicating important status. | Shall | Accessibility | 1+ | Not Started | - | - |
| NFR-ACC-004 | Text contrast and responsive text scaling shall be considered in interface design. | Shall | Accessibility | 1+ | Not Started | - | - |

### Backup and recovery (SRS 7.8) (5)

| ID | Requirement | Level | Module | Phase | Status | Code / tests | Outstanding work |
|---|---|---|---|---|---|---|---|
| NFR-REC-001 | The production database shall be backed up using a documented schedule. | Shall | Backup and recovery | 8 | Not Started | - | - |
| NFR-REC-002 | The initial target should be an RPO of 24 hours or better, subject to the selected database service. | Should | Backup and recovery | 8 | Not Started | - | RPO subject to selected provider (G-21). |
| NFR-REC-003 | The initial target should be an RTO of 8 hours or less for a documented recovery scenario. | Should | Backup and recovery | 8 | Not Started | - | RTO subject to selected provider (G-21). |
| NFR-REC-004 | Restoration procedures shall be tested before production launch and periodically thereafter. | Shall | Backup and recovery | 8 | Not Started | - | - |
| NFR-REC-005 | Cloudinary asset recovery and deletion behavior shall be considered separately from SQL database recovery. | Shall | Backup and recovery | 8 | Not Started | - | - |

### Security requirements (SRS 8) (30)

| ID | Requirement | Level | Module | Phase | Status | Code / tests | Outstanding work |
|---|---|---|---|---|---|---|---|
| SEC-001 | Passwords shall be stored only as secure password hashes. | Shall | Security: authentication | 2 | Not Started | - | - |
| SEC-002 | Access tokens shall have a limited lifetime. | Shall | Security: authentication | 2 | Not Started | - | - |
| SEC-003 | Refresh tokens shall be rotated and revocable. | Shall | Security: authentication | 2 | Not Started | - | - |
| SEC-004 | Password recovery shall require a verified recovery mechanism. | Shall | Security: authentication | 2 | Not Started | - | - |
| SEC-005 | Sensitive authentication endpoints shall use rate limiting and appropriate abuse protection. | Shall | Security: authentication | 2 | Not Started | - | - |
| SEC-006 | The system should support multi-factor authentication for privileged accounts. | Should | Security: authentication | Later (should) | Not Started | - | SRS "should"; MFA deferred (D-013). |
| SEC-007 | Every protected backend operation shall enforce authorization. | Shall | Security: authorization and tenant isolation | 2 | Not Started | - | - |
| SEC-008 | Client-supplied institution identifiers shall not be trusted without validating the user's membership and scope. | Shall | Security: authorization and tenant isolation | 2 | Not Started | - | - |
| SEC-009 | The backend shall validate ownership and institutional relationships for referenced records. | Shall | Security: authorization and tenant isolation | 2 | Not Started | - | - |
| SEC-010 | Unauthorized access attempts shall be rejected and recorded where appropriate. | Shall | Security: authorization and tenant isolation | 2 | Not Started | - | - |
| SEC-011 | The system shall test for cross-tenant access vulnerabilities. | Shall | Security: authorization and tenant isolation | 2 | Not Started | - | - |
| SEC-012 | Platform-level privileges shall be separated from ordinary institution-level roles. | Shall | Security: authorization and tenant isolation | 2 | Not Started | - | Support-access model undefined (G-05). |
| SEC-013 | The API shall validate all untrusted input. | Shall | Security: application | 1-2 | Not Started | - | - |
| SEC-014 | The backend shall use parameterized database operations or an ORM that safely parameterizes queries. | Shall | Security: application | 1-2 | Not Started | - | - |
| SEC-015 | The server shall restrict browser origins and configure cookies securely when cookies are used. | Shall | Security: application | 1-2 | Not Started | - | Web session strategy undecided (G-15). |
| SEC-016 | The web application shall protect cookie-based authentication against CSRF where applicable. | Shall | Security: application | 1-2 | Not Started | - | Web session strategy undecided (G-15). |
| SEC-017 | Sensitive operations shall require explicit permission and appropriate audit logging. | Shall | Security: application | 1-2 | Not Started | - | - |
| SEC-018 | The server shall not return secrets, stack traces or unnecessary sensitive fields to clients. | Shall | Security: application | 1-2 | Not Started | - | - |
| SEC-019 | Cloudinary API secrets shall be stored in server-side environment configuration or a secret manager. | Shall | Security: Cloudinary | 6 | Not Started | - | - |
| SEC-020 | Upload authorization shall be enforced by the backend. | Shall | Security: Cloudinary | 6 | Not Started | - | - |
| SEC-021 | Uploaded files shall be subject to size, format and authorization checks. | Shall | Security: Cloudinary | 6 | Not Started | - | - |
| SEC-022 | Sensitive documents shall use supported private access or controlled delivery mechanisms. | Shall | Security: Cloudinary | 6 | Not Started | - | Cloudinary private delivery unverified (G-12). |
| SEC-023 | Public asset URLs shall not be treated as proof of authorization. | Shall | Security: Cloudinary | 6 | Not Started | - | - |
| SEC-024 | File deletion and replacement operations shall verify asset ownership and authorization. | Shall | Security: Cloudinary | 6 | Not Started | - | - |
| SEC-025 | The system shall restrict access to personal information to authorized users. | Shall | Security: privacy | 8 (ongoing) | Not Started | - | - |
| SEC-026 | Student and guardian data shall not be exposed through public endpoints. | Shall | Security: privacy | 8 (ongoing) | Not Started | - | - |
| SEC-027 | Production logs shall avoid unnecessary personal information. | Shall | Security: privacy | 8 (ongoing) | Not Started | - | - |
| SEC-028 | Data retention and deletion shall follow applicable legal and institutional policies. | Shall | Security: privacy | 8 (ongoing) | Not Started | - | Retention policy and legal review (G-07, G-13). |
| SEC-029 | The system shall use encrypted transport for production communication. | Shall | Security: privacy | 8 (ongoing) | Not Started | - | - |
| SEC-030 | Applicable Indian data protection requirements and other relevant regulations shall be assessed before production launch. | Shall | Security: privacy | 8 (ongoing) | Not Started | - | Legal review needed before real student data (G-13). |

## 2. Business rules (SRS section 9)

| ID | Rule | Phase | Status | Code / tests | Outstanding work |
|---|---|---|---|---|---|
| BR-001 | **Institution isolation:** A user shall only access institutional records for which the user has valid authorization. | 2 | Not Started | - | - |
| BR-002 | **Role permissions:** A user's role shall determine the operations that the user may perform, subject to resource-specific permissions. | 2 | Not Started | - | - |
| BR-003 | **Academic enrollment:** A student shall be associated with valid academic enrollment records for the relevant class and academic year. | 3 | Not Started | - | - |
| BR-004 | **Attendance uniqueness:** A student shall not have duplicate attendance entries for the same attendance session. | 4 | Not Started | - | - |
| BR-005 | **Attendance corrections:** Attendance corrections shall be limited to authorized users and recorded in the audit history. | 4 | Not Started | - | - |
| BR-006 | **Result publication:** Students and guardians shall only view results that have been published and that they are authorized to access. | 5 | Not Started | - | - |
| BR-007 | **Fee balances:** Outstanding balances shall be calculated from invoice amounts, adjustments and verified payment records. | 5 | Not Started | - | - |
| BR-008 | **Financial auditability:** Financial transactions shall not be silently overwritten or deleted. | 5 | Not Started | - | - |
| BR-009 | **Parent access:** Guardians shall only access student information through verified relationships. | 3 | Not Started | - | - |
| BR-010 | **File access:** File ownership and authorization shall be checked before private assets are made available. | 6 | Not Started | - | - |
| BR-011 | **Institutional time:** Academic dates and attendance calculations shall respect the institution's configured time zone. | 4 | Not Started | - | - |
| BR-012 | **Deactivated accounts:** Deactivated users shall not continue to access protected application resources. | 2 | Not Started | - | - |
| BR-013 | **Academic history:** Historical enrollment and published results shall be preserved according to retention policy. | 3-5 | Not Started | - | Retention policy undefined (G-07). |
| BR-014 | **Administrative separation:** A platform Super Admin and an Institution Admin shall have distinct administrative scopes. | 2 | Not Started | - | Support-access model (G-05). |

## 3. MVP acceptance criteria (SRS section 13.2)

| ID | Criterion | Phase | Status | Verified by | Outstanding work |
|---|---|---|---|---|---|
| AC-001 | **Authentication:** Active authorized users can sign in and sign out. | 2 | Not Started | - | - |
| AC-002 | **Role access:** Each supported role sees and performs only permitted operations. | 2 | Not Started | - | - |
| AC-003 | **Tenant isolation:** Users from one institution cannot access another institution's protected data. | 2 | Not Started | - | - |
| AC-004 | **Student management:** Authorized administrators can create, update, search and view student records. | 3 | Not Started | - | - |
| AC-005 | **Academic structure:** Administrators can create academic years, departments, classes and subject assignments. | 3 | Not Started | - | - |
| AC-006 | **Attendance:** Teachers can record attendance for assigned classes, and students can view their own attendance. | 4 | Not Started | - | - |
| AC-007 | **Timetable:** Authorized users can create and view applicable timetable entries. | 4 | Not Started | - | - |
| AC-008 | **Fees:** Accountants can record authorized payments, and balances remain consistent after successful transactions. | 5 | Not Started | - | - |
| AC-009 | **Examinations:** Authorized users can enter and publish results; students can view only published results. | 5 | Not Started | - | - |
| AC-010 | **Guardians:** Guardians can access records only for verified linked students. | 3 | Not Started | - | - |
| AC-011 | **Cloudinary:** Authorized file uploads create valid asset references, and unauthorized access is rejected. | 6 | Not Started | - | - |
| AC-012 | **Responsive interface:** Core workflows operate correctly on supported mobile and web layouts. | 1-8 | Not Started | - | Checked in every phase that adds UI; final in Phase 8. |
| AC-013 | **Error handling:** Invalid input and failed requests produce safe, understandable errors. | 1+ | Not Started | - | - |
| AC-014 | **Data integrity:** Database constraints and transactions prevent invalid critical relationships and partial financial updates. | 5 | Not Started | - | - |
| AC-015 | **Recovery:** Backup restoration procedures are demonstrated in a non-production environment. | 8 | Not Started | - | Needs a restore demonstration in non-production. |
| AC-016 | **Documentation:** The API contract, setup instructions, database migrations and deployment instructions are maintained. | 1+ | Not Started | - | Maintained continuously; final review in Phase 8. |

## 4. Test scenarios (SRS section 13.3)

| ID | Scenario | Expected result | Phase | Related AC | Status | Test location |
|---|---|---|---|---|---|---|
| TC001 | Login with valid credentials | Authenticated session established | 2 | AC-001 | Not Started | - |
| TC002 | Login with invalid credentials | Request rejected safely | 2 | AC-001 | Not Started | - |
| TC003 | Student attempts to open another student's record | Access denied | 3 | AC-002, AC-004 | Not Started | - |
| TC004 | Teacher accesses an unassigned class | Access denied | 4 | AC-002, AC-006 | Not Started | - |
| TC005 | Institution A requests Institution B's student | Access denied | 3 (basic isolation tests from 2) | AC-003 | Not Started | - |
| TC006 | Duplicate attendance record is submitted | Duplicate prevented or rejected | 4 | AC-006, AC-014 | Not Started | - |
| TC007 | Invalid examination mark is entered | Validation error returned | 5 | AC-009, AC-013 | Not Started | - |
| TC008 | Unauthorized user attempts to record payment | Request rejected | 5 | AC-002, AC-008 | Not Started | - |
| TC009 | Database failure occurs during payment recording | Transaction rolled back | 5 | AC-014 | Not Started | - |
| TC010 | Unsupported file is uploaded | Upload rejected | 6 | AC-011 | Not Started | - |
| TC011 | Guardian requests an unlinked student's data | Access denied | 3 | AC-010 | Not Started | - |
| TC012 | Expired session is used | Reauthentication or valid refresh required | 2 | AC-001 | Not Started | - |
| TC013 | Web page is opened at a narrow viewport | Responsive layout remains usable | 1 (checked each phase) | AC-012 | Not Started | - |
| TC014 | Database backup is restored | Data is recoverable within the agreed target | 8 | AC-015 | Not Started | - |

## 5. Items from the JSON master prompt that the SRS does not state explicitly

These come from the project specification supplied with the SRS. They do not conflict with the SRS, but they are not SRS requirements, so they are tracked separately. Items marked Conditional need approval before they are built.

| ID | Item | JSON module | Phase | Status | Notes |
|---|---|---|---|---|---|
| JX-ADM-01 | Admission applications with status tracking (application record, review, accept/reject, link to student) | Admissions and Student Management | 3 | Not Started | Minimal scope pending confirmation (C-10, G-19). |
| JX-ADM-02 | Student document management (upload/view of admission documents) | Admissions and Student Management | 6 | Not Started | Needs Cloudinary private delivery (G-12). |
| JX-ACAD-01 | Courses/programs and Sections modelled as in the JSON entity list | Academic Management | 3 | Not Started | Class vs section vs classroom vocabulary (C-06, G-17). |
| JX-TIME-01 | Room allocation when required | Timetable Management | Conditional | Not Started | Not in SRS; build only if required (C-11). |
| JX-FEE-01 | Student fee assignments | Fees and Payments | 5 | Not Started | SRS uses invoices; reconcile in Phase 5 ERD (C-07). |
| JX-FEE-02 | Receipts | Fees and Payments | 5 | Not Started | Not in SRS; generated per payment (C-07). |
| JX-FEE-03 | Waivers and discounts with permission checks | Fees and Payments | 5 | Not Started | Rules and roles undefined (G-08). |
| JX-NOT-01 | Notification preferences where required | Announcements and Notifications | Conditional | Not Started | Not in SRS (C-11). |
| JX-INS-01 | Institution-level branding and configuration where required | Institution and Tenant Management | 3 | Not Started | SRS FR-FILE-002 covers branding assets. |
| JX-AUTH-01 | User profile management (own profile view/update) | Authentication and Account Management | 2 | Not Started | SRS FR-AUTH-006 covers read; student self-update in SRS 2.3.4. |
| JX-REP-01 | Admission summaries and examination summaries on dashboards | Dashboards and Reports | 7 | Not Started | SRS FR-REP-002 lists students, staff, classes, attendance. |
| JX-OPS-01 | Health endpoints | Audit and Platform Operations | 1 | Not Started | - |
| JX-OPS-02 | Centralized error logging and structured logging without secrets | Audit and Platform Operations | 1 | Not Started | - |
| JX-OPS-03 | Operational configuration (environment variables, example env files) | Audit and Platform Operations | 1 | Not Started | - |
| JX-OPS-04 | Backup and recovery documentation | Audit and Platform Operations | 8 | Not Started | Overlaps NFR-REC. |
| JX-SEC-01 | Security headers, secure CORS, rate limiting on sensitive endpoints | Security | 1-2 | Not Started | Overlaps SEC-005, SEC-015. |
| JX-SEC-02 | Dependency vulnerability checks | Security | 1+ | Not Started | Run at each phase; review in Phase 8. |
| JX-SEC-03 | Token validation of issuer, audience, expiry and signature | Security | 2 | Not Started | - |
| JX-SEC-04 | Prevent insecure direct object references; validate related records against active tenant | Security | 2+ | Not Started | Overlaps SEC-008, SEC-009. |
| JX-SEC-05 | Connection and transaction handling must not leak tenant context between requests | Security | 2 | Not Started | Matters most if RLS is adopted (G-14). |
| JX-SEC-06 | Evaluate PostgreSQL Row-Level Security as defense in depth | Security | 2 (spike) | Not Started | Compatibility with Prisma and pooling unverified (G-14). |
| JX-DEV-01 | Static analysis and formatting (ESLint/Prettier, dart analyze/format) | Development tooling | 1 | Not Started | - |
| JX-DEV-02 | CI checks where practical | Development tooling | 1 | Not Started | - |

## 6. Phase 0 deliverables

| Deliverable | Status | Location |
|---|---|---|
| Requirements checklist | Implemented | This file |
| Requirements to modules and acceptance criteria mapping | Implemented | `docs/PHASE0_AUDIT.md` section 5 |
| Conflicts, gaps, risks, assumptions | Implemented | `docs/PHASE0_AUDIT.md` sections 3 and 4 |
| Project state | Implemented | `docs/PROJECT_STATE.md` |
| Decision log | Implemented | `docs/DECISIONS.md` |
| Implementation plan | Implemented | `docs/IMPLEMENTATION_PLAN.md` |
