# Project Passport (PID) — Tekstil360

**Project:** Tekstil360 — B2B textile production marketplace, Phase 1 (MVP)
**Document:** PID — Project Passport
**Version:** Draft v1.0 · 22 October 2026
**Author:** Samil (11-314a)
**Language:** English (application UI: Turkish)
**Related documents:** `BRD.md`, `FR.md`, `NFR.md`, `WBS.md`

> Items marked `[OI-n]` are open issues listed at the end of this document. Everything described here beyond the one-line project description is a working proposal to be confirmed with real users (OI-1).

---

## 1. Prerequisites (the problem)

Problems as currently understood. Each one is a hypothesis to be validated in interviews before Checkpoint 1 (OI-1).

| # | Problem | Who suffers |
|---|---------|-------------|
| P1 | A firm that needs garments produced finds workshops (atölye) through personal contacts, phone calls and messaging groups. Each request is retyped for every workshop. | Firm |
| P2 | Quotes arrive in different formats (voice message, photo, free text), so offers cannot be compared on the same criteria (unit price, lead time, quantity). | Firm |
| P3 | There is no durable record of who was asked, who answered, and why a workshop was chosen. | Firm |
| P4 | Workshops with free capacity cannot see demand outside their personal network. | Workshop |
| P5 | Neither side can tell quickly whether the other is a real, registered company. | Firm, Workshop |
| P6 | A workshop that is not chosen usually gets no answer. | Workshop |

---

## 2. Objective (SMART)

**By the Week 15 defence, deliver a responsive Turkish-language web application on a public URL where a verified firm publishes a production listing (ilan), verified workshops submit bids, and the firm compares the bids and awards exactly one — with ≥5 test participants completing this scenario end to end, and every requirement in `FR.md` and `NFR.md` passing its stated check.**

| SMART | Content |
|-------|---------|
| **Specific** | Three roles (Firm, Workshop, Admin). Corporate application and admin verification, listings, one bid per workshop per listing, sealed bids, side-by-side comparison, award, email notifications, audit log. Full list in section 5. |
| **Measurable** | Metrics 1–7 in section 4, each with a target and a verification method. |
| **Achievable** | One executor, ~13 h/week `[OI-3]`, standard web stack, free-tier hosting and email. Payments, chat, logistics and native apps are excluded. |
| **Relevant** | Addresses P1–P6 (traceability in `BRD.md` §3.2). |
| **Time-bound** | Specification complete in Week 12; generated release in Weeks 14–15; defence in Week 15 (calendar dates `[OI-2]`). |

---

## 3. Success Criteria

| # | Criterion | Check |
|---|-----------|-------|
| SC-1 | Complete scenario works on the public URL: register → corporate application → admin approval → publish listing → bid → compare → award. | Live demo at defence without fallbacks |
| SC-2 | A workshop never sees another workshop's bid, and an unapproved account cannot publish or bid. | Automated access-control tests (`NFR.md` N3.4, N3.5) |
| SC-3 | Every listing, bid and account status change is stored with actor and timestamp. | Audit-log test (`FR.md` F9) |
| SC-4 | Every `MUST` requirement in `FR.md` passes its acceptance criterion. | Acceptance test run, 0 failures |
| SC-5 | The specification is complete enough that another student can generate a prototype from it without asking questions. | Ring exchange report (Week 13): gaps found ≤5 critical |

---

## 4. Metrics

| # | Metric | Baseline | Target | Verification |
|---|--------|----------|--------|--------------|
| M1 | Time for a firm to publish a listing | Not measured | ≤5 min median | Timed test with ≥5 participants |
| M2 | Time for a workshop to submit a bid | Not measured | ≤3 min median | Timed test with ≥5 participants |
| M3 | Time for a firm to gather comparable offers today (phone/messaging) | To be measured from 3–5 interviews `[OI-1]` | Reference value only | Interview notes |
| M4 | Participants completing the full scenario without help | n/a | ≥80% (≥4 of 5) | Observed test sessions |
| M5 | Access-control test cases passing | n/a | 100% | Automated tests |
| M6 | Status changes with an audit record | n/a | 100% | Audit-log test |
| M7 | `MUST` acceptance criteria passing | 0 | 100% | Acceptance test run |

---

## 5. Scope

| In scope — Phase 1 | Out of scope — deferred |
|--------------------|--------------------------|
| Registration with email verification and password login | Online payment, escrow, invoicing |
| Corporate application (kurumsal başvuru) with admin approve/reject | Automatic tax-number or trade-registry verification |
| Firm: create, edit, publish, cancel, close listings | In-app chat or messaging |
| Workshop: browse and filter open listings | Logistics, shipping, customs |
| Workshop: submit, revise, withdraw one bid per listing | Ratings and reviews |
| Sealed bids (not visible to other workshops) | Native mobile apps (the web app must work on mobile browsers) |
| Firm: compare bids side by side, award exactly one | Languages other than Turkish |
| Contact details revealed to both parties after award | Currencies other than TRY |
| Email notifications for the events listed in `FR.md` F7 | Push, SMS, messenger notifications |
| Admin: review applications, suspend accounts, remove listings | Multi-user company accounts and sub-roles |
| Audit log of status changes | Analytics dashboards, reporting exports |
| | Third-party integrations (ERP, marketplaces) |

---

## 6. Impact Assessment

| Area | Impact |
|------|--------|
| Firm's sourcing process | Process — quote collection moves from messaging to a single listing |
| Workshop's sales process | Process — demand becomes visible beyond personal contacts |
| Company and contact data | Data — the system stores company names, tax numbers, phone numbers and emails; personal-data rules apply (`NFR.md` N4) |
| Email delivery | Integration — a transactional email provider becomes a dependency |
| File storage | Integration — listing attachments need storage |
| Administration | Process — someone must review applications; the executor acts as admin during the course |

---

## 7. Constraints and Assumptions

### Constraints

| # | Constraint |
|---|------------|
| C1 | Fixed course calendar: specification by Week 12, defence in Week 15. |
| C2 | One executor. |
| C3 | Budget limited to free tiers of hosting, database and email. |
| C4 | The product is judged against its specification (course rule), so scope changes after Checkpoint 2 are not allowed. |
| C5 | UI in Turkish only; documents in English. |
| C6 | Currency TRY only; time zone Europe/Istanbul. |
| C7 | The executor must be able to explain every decision and every line (course ownership rule). |

### Assumptions

| # | Assumption | Owner | Confirm by |
|---|------------|-------|------------|
| A1 | At least 3 firms or workshops can be interviewed to validate P1–P6. | Samil | Week 3 |
| A2 | The corporate application can be reviewed manually by an admin without integration to official registries. | Samil | Week 4 |
| A3 | Both firms and workshops go through the corporate application. | Samil | Week 3 |
| A4 | A test group of ≥5 people is available for the usability tests. | Samil | Week 13 |
| A5 | Free-tier email and hosting cover test volumes (≤500 emails/day). | Samil | Week 7 |

---

## Open Issues

| # | Issue | Owner | Target |
|---|-------|-------|--------|
| OI-1 | Validate problems P1–P6 and measure baseline M3 with ≥3 real firms/workshops. | Samil | Week 3 |
| OI-2 | Fix exact calendar dates for Weeks 12–15 (spec complete, release, defence). | Samil + teacher | Week 1 |
| OI-3 | Confirm weekly hours available. | Samil | Week 1 |
| OI-4 | Confirm that the closed lists in `FR.md` §3 (garment categories, service types) match real use. | Samil | Week 4 |
| OI-5 | Confirm whether the corporate application applies to both roles (A3). | Samil | Week 3 |
| OI-6 | Confirm which data-protection law and hosting region apply to the real deployment (`NFR.md` N4) | Samil | Week 6 |
| OI-7 | Confirm the volumes in `NFR.md` N10 against real expectations | Samil | Week 4 |
