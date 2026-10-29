# Business Requirements Document (BRD) — Tekstil360

**Version:** Draft v1.0 · 22 October 2026 · Author: Samil (11-314a)
**Traces from:** `PID.md` · **Traces to:** `FR.md`, `NFR.md`, `WBS.md`

---

# 1. General Information

## 1.1 Purpose

This document states what the business needs from Tekstil360 and why, in a form that each requirement can be traced to a goal and forward to functional and non-functional requirements. It is written for the course reviewer and for the AI agent that will generate the product.

## 1.2 Project Information

| Field | Value |
|-------|-------|
| Project | Tekstil360 — B2B textile production marketplace, Phase 1 (MVP) |
| Executor / owner | Samil |
| Reviewer | Course teacher |
| Platform | Responsive web application, Turkish UI |
| Phase | Specification (Weeks 1–12), then generation (Weeks 14–15) |

## 1.3 Change History

| Version | Date | Change |
|---------|------|--------|
| 1.0 | 22.10.2026 | First draft |

## 1.4 Glossary

| Term | Meaning |
|------|---------|
| Firma (Firm) | A company that needs garments or textile products produced and publishes listings. |
| Atölye (Workshop) | A producer that submits bids for listings. |
| İlan (Listing) | A structured production request published by a firm. |
| Teklif (Bid) | A workshop's offer for one listing. |
| Kurumsal başvuru (Corporate application) | The company data a user submits for admin verification. |
| Award | The firm's selection of exactly one bid for a listing. |
| Sealed bid | A bid visible only to its author and to the listing's firm. |
| Admin | Platform operator who verifies accounts and moderates. |
| TRY | Turkish lira, the only currency. |
| VKN | 10-digit Turkish tax number. |

---

# 2. Executive Summary

Turkish textile firms that need production capacity collect quotes from workshops one by one through phone and messaging apps. Offers arrive in different formats, cannot be compared, and leave no record. Workshops, in turn, only see demand inside their personal networks.

Tekstil360 gives both sides one structured channel: a verified firm publishes a listing, verified workshops submit bids in a fixed format, the firm compares the bids side by side and awards one. Phase 1 delivers this core loop only; payments, chat and logistics are excluded.

---

# 3. Business Goals and Scope

## 3.1 Project Goals

| ID | Goal | Solves |
|----|------|--------|
| G-01 | Replace one-by-one quote collection with one listing that many workshops can answer. | P1, P4 |
| G-02 | Make offers comparable by forcing one bid format (unit price, lead time, offered quantity). | P2 |
| G-03 | Let only verified companies publish and bid. | P5 |
| G-04 | Keep a durable record of listings, bids and award decisions. | P3 |
| G-05 | Give every bidder a visible outcome (accepted or not selected). | P6 |
| G-06 | Deliver a specification complete enough to generate the product from it within the course. | Course requirement |

## 3.2 Success Criteria

Defined in `PID.md` §3 (SC-1 … SC-5) and §4 (M1 … M7).

## 3.3 Project Boundaries

**In scope and out of scope:** see `PID.md` §5. This list is closed: anything not named there is out of scope.

## 3.4 Business Risks

| ID | Risk | Probability | Impact | Mitigation |
|----|------|-------------|--------|------------|
| R-01 | The assumed problems (P1–P6) do not match reality. | Medium | High | Interview ≥3 real users by Week 3 (OI-1); revise `PID.md` before Checkpoint 1. |
| R-02 | Scope grows after Checkpoint 2. | High | High | Scope freeze at Checkpoint 2; additions go to a Phase 2 list. |
| R-03 | Users bypass the platform after seeing each other's contacts. | Medium | Low (outside Phase 1 metrics) | Contacts revealed only after award (RULE-05); accepted residual risk. |
| R-04 | Fake companies pass verification. | Medium | Medium | Manual review with required fields; suspension by admin (FR F8). |
| R-05 | The generated code contradicts the specification. | Medium | High | Acceptance test per `FR.md`; generation in small slices (`WBS.md` §5). |
| R-06 | Too little time for the build weeks. | Medium | High | Cut list in `WBS.md` §7; critical path defined. |
| R-07 | Personal-data handling errors (company contacts). | Low | Medium | `NFR.md` N4; store only the fields listed in `FR.md` §3. |

## 3.5 Constraints and Dependencies

Constraints: `PID.md` §7 (C1–C7).

| ID | Dependency | Needed by |
|----|------------|-----------|
| D-01 | Transactional email provider (free tier) | Week 14 |
| D-02 | Hosting with a public URL and a managed database (free tier) | Week 14 |
| D-03 | File storage for listing attachments | Week 14 |
| D-04 | ≥3 interview participants | Week 3 |
| D-05 | ≥5 test participants | Week 13 |

---

# 4. Source Data

## 4.1 Current State (AS-IS)

1. The firm decides what it needs: product, quantity, deadline.
2. The firm contacts workshops one by one by phone or messaging app, repeating the request.
3. Workshops answer in free form, at different times, with different information.
4. The firm collects answers by hand and compares them in memory, notes or a spreadsheet.
5. The firm chooses a workshop and informs it. Other workshops often hear nothing.
6. No record of the process is kept in one place.

**Problems and impact:** P1–P6 in `PID.md` §1. Baseline time for steps 2–4 is measured in OI-1.

## 4.2 Target Process (TO-BE)

1. Firm and workshop register; each submits a corporate application; admin approves.
2. Firm publishes a listing with fixed fields and a bid deadline.
3. Approved workshops browse open listings and submit one bid each in the fixed format.
4. Firm views all bids side by side, sorted and filtered.
5. Firm awards one bid. The listing closes; the winner and the other bidders are notified; contact details are revealed to the firm and the winner.
6. All status changes are recorded.

## 4.3 Business Rules

| ID | Rule |
|----|------|
| RULE-01 | Only accounts in status APPROVED may publish listings or submit bids. |
| RULE-02 | A workshop has at most one active bid per listing; it may revise or withdraw it while the listing is OPEN. |
| RULE-03 | A firm sees all bids on its own listings; a workshop sees only its own bids. |
| RULE-04 | Exactly one bid may be awarded per listing; an award is final in Phase 1. |
| RULE-05 | Contact details (phone, email) of the other party are visible only after the award, and only to the firm and the winning workshop. |
| RULE-06 | Bids are accepted only while the listing is OPEN and before its bid deadline. |
| RULE-07 | A firm cannot bid and a workshop cannot publish listings (one role per account). |
| RULE-08 | Prices are in TRY per unit; dates and deadlines are in Europe/Istanbul time. |
| RULE-09 | A listing with at least one bid cannot have its quantity, category or service type changed. |

## 4.4 Stakeholders

| Stakeholder | Interest | Influence |
|-------------|----------|-----------|
| Firm users | Fast, comparable offers | High |
| Workshop users | Access to demand | High |
| Admin (executor) | Trustworthy accounts, low manual effort | High |
| Course teacher | Specification quality, traceability | High |
| Ring partner (Week 13) | A specification without gaps | Medium |

---

# 5. Solution Requirements

## 5.1 Business Requirements

| ID | Requirement | Goal | Priority | FR group |
|----|-------------|------|----------|----------|
| BRQ-01 | A user can register as firm or workshop and log in securely. | G-03 | MUST | F1 |
| BRQ-02 | A user submits a corporate application; an admin approves or rejects it with a reason. | G-03 | MUST | F2, F8 |
| BRQ-03 | An approved firm can create, publish, edit, cancel and close listings in a fixed format. | G-01 | MUST | F3 |
| BRQ-04 | An approved workshop can browse and filter open listings. | G-01 | MUST | F4 |
| BRQ-05 | An approved workshop can submit, revise and withdraw one bid per listing in a fixed format. | G-01, G-02 | MUST | F5 |
| BRQ-06 | Bids are hidden from other workshops. | G-03 | MUST | F5, F6 |
| BRQ-07 | A firm can compare all bids on a listing side by side, sorted and filtered. | G-02 | MUST | F6 |
| BRQ-08 | A firm can award exactly one bid; the listing closes. | G-01, G-05 | MUST | F6 |
| BRQ-09 | After award, contact details become visible to the firm and the winner only. | G-01 | MUST | F6 |
| BRQ-10 | Users receive email for the events listed in `FR.md` F7. | G-05 | MUST | F7 |
| BRQ-11 | An admin can suspend accounts and remove listings. | G-03 | MUST | F8 |
| BRQ-12 | Every status change is recorded with actor and time. | G-04 | MUST | F9 |
| BRQ-13 | The interface is in Turkish and works on desktop and mobile browsers. | all | MUST | NFR N5, N6 |

## 5.2 Key Non-Functional Requirements

Detailed in `NFR.md`. Summary: performance (N1), reliability (N2), security and access control (N3), personal-data handling (N4), usability and localisation (N5), compatibility (N6), maintainability (N7), operability (N8).

## 5.3 Key Data Requirements

| Data | Notes |
|------|-------|
| Account (email, password hash, role, status) | Role is one of FIRM, WORKSHOP, ADMIN |
| Company profile (legal name, VKN, city, phone; workshops also monthly capacity and services) | Fields closed in `FR.md` §3 |
| Listing (fields in `FR.md` §3) | Status machine in `FR.md` §4 |
| Bid (fields in `FR.md` §3) | Status machine in `FR.md` §4 |
| Attachment (file reference, size, type) | ≤3 per listing |
| Audit record (actor, entity, old status, new status, time) | Append-only |

## 5.4 Key Integration Requirements

| ID | Integration | Purpose |
|----|-------------|---------|
| INT-01 | Transactional email provider | Notifications and email verification |
| INT-02 | File storage | Listing attachments |

No other integrations are in scope.

## 5.5 Key Process Change Requirements

| ID | Change |
|----|--------|
| PC-01 | Firms must describe a request once, in the fixed listing format. |
| PC-02 | Workshops must answer in the fixed bid format. |
| PC-03 | The admin reviews each corporate application. |

---

# 6. Solution Options and Release

## 6.1 Options

| Option | Description | Verdict |
|--------|-------------|---------|
| A | Keep phone/messaging groups | Rejected: this is the current problem. |
| B | Shared spreadsheet or form tool | Rejected: no verification, no sealed bids, no record per party. |
| C | Custom web application (chosen) | Accepted: only option that enforces the rules in section 4.3 and is the course deliverable. |

## 6.2 Scope of First Release

All BRQ-01 … BRQ-13. No later release is planned within the course.

## 6.3 Resource Plan

| Resource | Amount |
|----------|--------|
| Executor | 1 person, ~13 h/week `[OI-3]` |
| AI coding agent | Used for generation (Weeks 13–14) |
| Test participants | ≥5 |

## 6.4 Budget Estimate

| Item | Cost |
|------|------|
| Hosting, database, email, storage | 0 (free tiers) |
| Domain (optional) | Not required |

---

# Appendix — Open Questions

| ID | Question | Owner | Target |
|----|----------|-------|--------|
| OI-1 | Validate P1–P6 and baseline with ≥3 real users | Samil | Week 3 |
| OI-2 | Exact calendar dates for Weeks 12–15 | Samil + teacher | Week 1 |
| OI-3 | Weekly hours available | Samil | Week 1 |
| OI-4 | Confirm closed lists (categories, service types) | Samil | Week 4 |
| OI-5 | Does the corporate application apply to both roles? | Samil | Week 3 |
| OI-6 | Confirm which data-protection law and hosting region apply to the real deployment (`NFR.md` N4) | Samil | Week 6 |
| OI-7 | Confirm the volumes in `NFR.md` N10 against real expectations | Samil | Week 4 |
