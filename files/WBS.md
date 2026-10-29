# Work Breakdown Structure and Project Plan (WBS) — Tekstil360

**Version:** Draft v1.0 · 22 October 2026 · Author: Samil (11-314a)
**Traces from:** `PID.md` (objective, constraints), `BRD.md`, `FR.md`, `NFR.md`
**Calendar:** the 15-week course structure. Weeks are numbered W1–W15; calendar dates are not fixed yet `[OI-2]`.

---

## 1. Planning basis

| Item | Value |
|------|-------|
| Executor | Samil (one person) |
| Total effort | ≈195 h |
| Average load | ≈13 h/week; heavier in W13–W15 `[OI-3]` |
| Weekly rhythm | Session A: review and new task. Between sessions: work at home, submitted as a pull request before session B. Session B: defence and cross review. |
| Course rules applied | Late work loses 30% per week; a review given is required for points; every decision must be explainable by the author; the product is judged against its specification. |

---

## 2. Work Breakdown Structure

Effort is in hours. "Done when" is the check that closes the work package.

### 1. Task (W1–W4) — 24 h

| ID | Work package | Effort | Week | Done when |
|----|--------------|--------|------|-----------|
| 1.1 | Repository from course template, docs folder, local environment with AI agent, trial pull request | 4 | W1 | Trial PR opened and reviewed |
| 1.2 | Problem validation: ≥3 interviews with firms/workshops, baseline M3 (OI-1) | 8 | W2–W3 | Notes stored in the repo; `PID.md` P1–P6 confirmed or changed |
| 1.3 | PID finalised | 3 | W3 | `PID.md` reviewed; open issues have owners |
| 1.4 | BRD finalised | 4 | W3–W4 | `BRD.md` IDs consistent with `PID.md` |
| 1.5 | End-to-end user scenarios (firm, workshop, admin) written as numbered steps | 5 | W4 | Every scenario step maps to an FR ID |

### 2. Requirements (W5–W6) — 18 h

| ID | Work package | Effort | Week | Done when |
|----|--------------|--------|------|-----------|
| 2.1 | FR finalised, each MUST with an acceptance criterion | 8 | W5 | Every FR ID traces to a BRQ |
| 2.2 | NFR finalised, each with target and verification | 5 | W5–W6 | No NFR without a verification method |
| 2.3 | Constraints, assumptions and open issues reviewed; OI-4, OI-5, OI-7 closed | 2 | W6 | Open-issue table updated |
| 2.4 | **Checkpoint 1** package and defence | 3 | W6 | Checkpoint passed |

### 3. Solution (W7–W11) — 63 h

| ID | Work package | Effort | Week | Done when |
|----|--------------|--------|------|-----------|
| 3.1 | Data model: entities, fields, constraints (one active bid per workshop per listing; one ACCEPTED bid per listing), indexes | 10 | W7–W8 | Every field in `FR.md` §3 appears in the model |
| 3.2 | Screen map and states for S1–S12, including loading, empty, error | 10 | W8–W9 | Each FR ID is covered by a screen |
| 3.3 | Architecture and ADRs: stack, hosting, database, email provider, file storage, authentication approach | 10 | W9–W10 | One ADR per decision; each satisfies N3, N8 |
| 3.4 | API contracts: endpoint, role, request, response, errors | 8 | W10 | Endpoint × role matrix ready for N3.4 test |
| 3.5 | Security and error handling design (N3, N4) | 6 | W10–W11 | Each N3 item has a design answer |
| 3.6 | Acceptance test plan: one test per MUST acceptance criterion and per NFR check | 8 | W11 | AC → test table complete |
| 3.7 | Generation plan: slices S0–S10 (section 5), order, guardrails, automated checks per slice | 8 | W11 | Each slice lists inputs, outputs and checks |
| 3.8 | **Checkpoint 2** package and defence; scope freeze | 3 | W11 | Checkpoint passed |

### 4. Exchange (W12–W13) — 26 h

| ID | Work package | Effort | Week | Done when |
|----|--------------|--------|------|-----------|
| 4.1 | Final consistency review: IDs, state machines, closed lists, cross-references across all documents | 6 | W12 | No contradictions found by a second read |
| 4.2 | Hand the specification to the ring partner and receive another | 1 | W12 | Handover done |
| 4.3 | Prototype generated from the partner's specification, without asking the author | 14 | W13 | Prototype runs |
| 4.4 | Report: what had to be invented, where the text contradicts itself | 5 | W13 | Report submitted |

### 5. Product (W14–W15) — 44 h

| ID | Work package | Effort | Week | Done when |
|----|--------------|--------|------|-----------|
| 5.1 | Generate and verify slices S0–S10 (section 5) | 28 | W14–W15 | All slice checks green |
| 5.2 | Deploy on a public URL; seed script run | 4 | W14 | N8.1, N8.4, N8.5 met |
| 5.3 | Usability tests with ≥5 participants; fixes of blocking defects | 6 | W15 | M1, M2, M4 measured |
| 5.4 | Defence preparation; opposing the partner's project | 6 | W15 | Defence delivered |

### 6. Ongoing — 20 h

| ID | Work package | Effort | Week | Done when |
|----|--------------|--------|------|-----------|
| 6.1 | Weekly review of other students' artefacts (≈1.5 h/week) | 20 | W1–W13 | One review per week submitted |

**Total:** 24 + 18 + 63 + 26 + 44 + 20 = **195 h**.

---

## 3. Schedule

`■` = active work.

| Work package group | W1 | W2 | W3 | W4 | W5 | W6 | W7 | W8 | W9 | W10 | W11 | W12 | W13 | W14 | W15 |
|--------------------|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|-----|-----|
| 1 Task | ■ | ■ | ■ | ■ | | | | | | | | | | | |
| 2 Requirements | | | | | ■ | ■ | | | | | | | | | |
| 3 Solution | | | | | | | ■ | ■ | ■ | ■ | ■ | | | | |
| 4 Exchange | | | | | | | | | | | | ■ | ■ | | |
| 5 Product | | | | | | | | | | | | | | ■ | ■ |
| 6 Reviews | ■ | ■ | ■ | ■ | ■ | ■ | ■ | ■ | ■ | ■ | ■ | ■ | ■ | | |

### Weekly plan

| Week | Focus | Deliverable at week end |
|------|-------|--------------------------|
| W1 | Setup | Repository, environment, trial PR; project topic and problem statement; OI-2, OI-3 answered |
| W2 | Interviews | ≥2 interviews done; interview notes in the repo |
| W3 | Validation | Interviews complete; `PID.md` and `BRD.md` updated (OI-1, OI-5 closed) |
| W4 | Scenarios | Scenarios written; BRD final |
| W5 | Functional requirements | `FR.md` final |
| W6 | Non-functional requirements | `NFR.md` final; **Checkpoint 1** |
| W7 | Data model (part 1) | Entities and relations |
| W8 | Data model (part 2), screens (part 1) | Constraints and indexes; first screens |
| W9 | Screens, architecture (part 1) | S1–S12 with states; first ADRs |
| W10 | Architecture, contracts, security (part 1) | ADRs complete; API contracts |
| W11 | Security, test plan, generation plan | **Checkpoint 2**; scope freeze |
| W12 | Consistency review and handover | Final specification; ring draw |
| W13 | Prototype from the partner's spec | Prototype and gap report |
| W14 | Generation and release | Slices S0–S10 generated; public URL |
| W15 | Tests, defence | Usability results; defence |

---

## 4. Milestones

| # | Milestone | Week | Condition to pass |
|---|-----------|------|-------------------|
| M1 | Working repository | W1 | Trial PR walked through the whole submission path |
| M2 | Problem validated | W3 | ≥3 interviews documented |
| M3 | Checkpoint 1 | W6 | PID, BRD, FR, NFR complete and consistent |
| M4 | Checkpoint 2 / scope freeze | W11 | Data model, screens, architecture, contracts, security, test plan, generation plan complete |
| M5 | Specification handed over | W12 | Final version in the repository |
| M6 | Partner prototype and gap report | W13 | Both submitted |
| M7 | Public release | W14 | URL works; admin can be seeded; S0–S8 slices pass |
| M8 | Defence | W15 | Live demo, usability results, opposing done |

---

## 5. Generation slices (input for work package 3.7)

Each slice is generated, then verified with its checks before the next starts. Order follows dependencies. The audit-writing helper (F9.1–F9.3) is built in S0 and used by every later slice.

| Slice | Content | FR / NFR | Effort (h) | Depends on |
|-------|---------|----------|------------|------------|
| S0 | Scaffold, CI, migrations, seed script, health endpoint, audit-writing helper, deploy skeleton | N7, N8, F9.1–F9.3 | 3 | — |
| S1 | Registration, email verification, login, logout, password reset | F1, F7.1, F7.2 | 3 | S0 |
| S2 | Corporate application, admin review, account state machine | F2, F8.1, F8.2, F7.3, F7.4 | 3 | S1 |
| S3 | Listings: create, edit, publish, cancel, close, my listings, attachments | F3.1–F3.6, F3.8, F3.9 | 4 | S2 |
| S4 | Browse and filter listings | F4 | 2 | S3 |
| S5 | Bids: submit, revise, withdraw, my bids | F5, F7.5 | 2.5 | S4 |
| S6 | Compare and award, contact reveal | F6, F7.6, F7.7 | 3.5 | S5 |
| S7 | Deadline job and expired-review handling | F3.7, F7.8, N2.4 | 1 | S6 |
| S8 | Admin moderation: suspend, reinstate, remove | F8.3–F8.7, F7.9 | 2 | S6 |
| S9 | Audit viewer | F9.4 | 1 | S0 |
| S10 | Hardening: access-control matrix, load test, accessibility and browser checks, README | N1, N3, N5, N6, N7.4 | 3 | S1–S9 |

Slice effort total: 3 + 3 + 3 + 4 + 2 + 2.5 + 3.5 + 1 + 2 + 1 + 3 = **28 h** (equals work package 5.1).

Slices S7, S8 and S9 can be done in any order after their dependencies.

---

## 6. Critical path

`Validated problem (1.2) → FR/NFR (2.1, 2.2) → Data model (3.1) → Architecture and contracts (3.3, 3.4) → Test plan and generation plan (3.6, 3.7) → Final consistency review (4.1) → Slices S0 → S1 → S2 → S3 → S4 → S5 → S6 → S10 → Deployment (5.2) → Usability tests (5.3) → Defence`

Off the critical path: S7, S8, S9, the partner prototype (4.3, 4.4), reviews (6.1).

---

## 7. Risks to the plan and what to cut

| Risk | Mitigation |
|------|-----------|
| Hours per week lower than 13 | Use the cut list below before Checkpoint 2 |
| Specification contradicts itself and the generated product follows the contradiction | Work package 4.1; one automated check per cross-reference where possible |
| Free-tier limits hit during testing | Use the synthetic dataset from `NFR.md` N10; keep attachment limits (N9.2) |
| Partner prototype takes longer than 14 h | Generate by slices in the partner's order; report the gap instead of fixing it |

**Cut list.** Scope changes after Checkpoint 2 must also update the specification, otherwise the product loses points (course rule). If the plan is behind, remove from the specification before Checkpoint 2, in this order:

1. F6.3 (filter by full quantity) and the filter on bid deadline in F4.2
2. F4.3 sort options other than "newest"
3. F3.9 attachments, with N3.11 and N9.2
4. F9.4 audit viewer (S9); audit writing stays
5. F7.8 expiry email

**Never cut:** sealed bids (N3.5, N3.7), status gates (N3.6), atomic award (N2.1, N2.2), audit writing (F9.1), the access-control matrix (N3.4). These carry the product's value and the success criteria SC-2 and SC-3.

---

## 8. Open issues

| # | Issue | Owner | Target |
|---|-------|-------|--------|
| OI-2 | Calendar dates of W12–W15 | Samil + teacher | W1 |
| OI-3 | Real weekly hours versus the 13 h/week plan | Samil | W1 |
