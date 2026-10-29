# Functional Requirements (FR) — Tekstil360

**Version:** Draft v1.0 · 22 October 2026 · Author: Samil (11-314a)
**Traces from:** `BRD.md` §5.1 (BRQ-01 … BRQ-13) · **Related:** `PID.md`, `NFR.md`, `WBS.md`

Priorities: **MUST** = Phase 1. **WON'T** = explicitly excluded. Acceptance criteria (AC) use Given/When/Then. All lists in this document are closed: a value not listed is invalid.

---

## 1. Actors

| Actor | Description |
|-------|-------------|
| Visitor | Not logged in. |
| Firm | Account with role FIRM. Publishes listings, receives bids. |
| Workshop | Account with role WORKSHOP. Browses listings, submits bids. |
| Admin | Account with role ADMIN. Created by seed script only, never through registration. |
| System | Background and automatic actions (emails, deadline closing, audit records). |

"Approved" means account status APPROVED (§4.1).

---

## 2. Requirements

### F1 — Registration and authentication (BRQ-01)

| ID | Priority | Requirement |
|----|----------|-------------|
| F1.1 | MUST | A visitor can register with email, password and role (FIRM or WORKSHOP). |
| F1.2 | MUST | Email must be unique (case-insensitive). Password: 8–72 characters, at least one letter and one digit. |
| F1.3 | MUST | After registration the system sends a verification email with a link valid for 24 hours. |
| F1.4 | MUST | A user can log in only after email verification. |
| F1.5 | MUST | A user can log out; the session ends immediately. |
| F1.6 | MUST | A user can request a password reset by email; the link is valid for 1 hour and works once. |
| F1.7 | MUST | A session expires after 7 days of inactivity. |
| F1.8 | WON'T | Social login, two-factor authentication, multiple users per company. |

**AC F1.2** — Given an account with email `a@x.com` exists, When a visitor registers with `A@X.com`, Then the system rejects the request with the message "email already registered" and creates no account.
**AC F1.4** — Given a registered user has not verified the email, When they submit correct credentials, Then login is refused with the message "verify your email first".

### F2 — Corporate application (BRQ-02)

| ID | Priority | Requirement |
|----|----------|-------------|
| F2.1 | MUST | After email verification the user must submit a corporate application containing the fields listed in §3.1 before any other action except logout. |
| F2.2 | MUST | On submission, account status becomes PENDING_REVIEW. |
| F2.3 | MUST | The user can see the current status and, if REJECTED, the admin's reason. |
| F2.4 | MUST | A REJECTED user can edit the application and resubmit; status returns to PENDING_REVIEW. |
| F2.5 | MUST | VKN must be exactly 10 digits. The system checks the format only. |
| F2.6 | WON'T | Automatic verification against any official registry. |

**AC F2.1** — Given a verified user with status PENDING_APPLICATION, When they open the listings page, Then they are redirected to the application form.

### F3 — Listings (BRQ-03)

| ID | Priority | Requirement |
|----|----------|-------------|
| F3.1 | MUST | An approved Firm can create a listing with the fields in §3.2; it is saved as DRAFT. |
| F3.2 | MUST | The Firm can edit a DRAFT freely. |
| F3.3 | MUST | The Firm can publish a DRAFT; status becomes OPEN. Publishing requires all mandatory fields valid and `bid_deadline` in the future. |
| F3.4 | MUST | While OPEN, the Firm can edit `title`, `description`, `bid_deadline` (only later than the current value) and attachments. Quantity, category and service type cannot be changed once the listing has ≥1 bid (RULE-09). |
| F3.5 | MUST | The Firm can cancel a DRAFT or OPEN listing; status becomes CANCELLED. All SUBMITTED bids become NOT_SELECTED and their authors are notified. |
| F3.6 | MUST | The Firm can close an OPEN listing without awarding; status becomes CLOSED_NO_AWARD. |
| F3.7 | MUST | When `bid_deadline` passes and the listing is still OPEN, the System sets it to EXPIRED_REVIEW (§4.2): bids stop being accepted and the Firm can still award within 14 days. |
| F3.8 | MUST | The Firm sees a list of its own listings with status, bid count and deadline, filterable by status. |
| F3.9 | MUST | A listing may have up to 3 attachments (PDF, JPG, PNG; ≤5 MB each). |
| F3.10 | WON'T | Deleting listings (cancelling is the only removal), duplicating listings, listing templates. |

**AC F3.3** — Given a DRAFT with `bid_deadline` in the past, When the Firm publishes, Then the system refuses with "deadline must be in the future" and the status stays DRAFT.
**AC F3.4** — Given an OPEN listing with one bid, When the Firm changes `quantity`, Then the system refuses and the quantity is unchanged.

### F4 — Browse listings (BRQ-04)

| ID | Priority | Requirement |
|----|----------|-------------|
| F4.1 | MUST | An approved Workshop sees a paginated list (20 per page) of OPEN listings, newest first. |
| F4.2 | MUST | Filters: `garment_category`, `service_type`, `city`, quantity range (min/max), bid deadline before a given date. Filters combine with AND. |
| F4.3 | MUST | Sort options: newest, nearest bid deadline, largest quantity. |
| F4.4 | MUST | A Workshop can open a listing's detail page, including attachments. The detail page does not show other workshops' bids and does not show the Firm's phone or email. |
| F4.5 | MUST | A Workshop cannot see DRAFT, CANCELLED or REMOVED listings. |
| F4.6 | WON'T | Free-text search, saved filters, recommendations. |

**AC F4.4** — Given a Workshop views an OPEN listing that has 3 bids from others, When the detail page loads, Then no bid data from other workshops and no Firm contact data appears in the page or in the API response.

### F5 — Bids (BRQ-05, BRQ-06)

| ID | Priority | Requirement |
|----|----------|-------------|
| F5.1 | MUST | An approved Workshop can submit one bid on an OPEN listing before its `bid_deadline`, with the fields in §3.3. |
| F5.2 | MUST | A second bid by the same Workshop on the same listing is refused. |
| F5.3 | MUST | While the listing is OPEN and before the deadline, the Workshop can revise its SUBMITTED bid. Each revision overwrites the values and writes an audit record with the old values. |
| F5.4 | MUST | The Workshop can withdraw its SUBMITTED bid; status becomes WITHDRAWN. After withdrawal it may submit a new bid (the withdrawn one stays in the audit log). |
| F5.5 | MUST | The Workshop sees a list of its own bids with listing title, bid status and listing status. |
| F5.6 | MUST | The Firm is notified (F7) when a bid is submitted. |
| F5.7 | MUST | A Workshop can only read bids it authored (RULE-03). |
| F5.8 | WON'T | Bidding on behalf of another company, partial awards, counter-offers. |

**AC F5.2** — Given Workshop W has a SUBMITTED bid on listing L, When W submits another bid on L, Then the system refuses with "you already bid on this listing".
**AC F5.1** — Given listing L has `bid_deadline` 10:00 and the current time is 10:01, When a Workshop submits a bid, Then the system refuses with "bidding is closed".

### F6 — Compare and award (BRQ-07, BRQ-08, BRQ-09)

| ID | Priority | Requirement |
|----|----------|-------------|
| F6.1 | MUST | The Firm sees, for its own listing, a table of all SUBMITTED bids with columns: workshop name, city, unit price, total price (unit price × offered quantity), lead time (days), offered quantity, note. |
| F6.2 | MUST | The table can be sorted by unit price, total price, lead time; default sort is unit price ascending. |
| F6.3 | MUST | The table can be filtered to bids where `offered_quantity` equals the listing quantity. |
| F6.4 | MUST | The Firm can award exactly one SUBMITTED bid while the listing is OPEN or EXPIRED_REVIEW. A confirmation step is required. |
| F6.5 | MUST | On award: the chosen bid becomes ACCEPTED, all other SUBMITTED bids become NOT_SELECTED, and the listing becomes AWARDED. These changes happen together or not at all. |
| F6.6 | MUST | After award, the Firm sees the winner's phone and email, and the winner sees the Firm's phone and email. No other workshop sees them. |
| F6.7 | MUST | An award cannot be undone in Phase 1 (RULE-04). |
| F6.8 | MUST | A Firm cannot read bids on listings it does not own. |
| F6.9 | WON'T | Ranking scores, automatic award, negotiation. |

**AC F6.5** — Given listing L has bids B1, B2, B3 (all SUBMITTED), When the Firm awards B2, Then B2 is ACCEPTED, B1 and B3 are NOT_SELECTED, L is AWARDED, and if any of these writes fails none of them is persisted.
**AC F6.6** — Given L is AWARDED to workshop W1, When workshop W2 (bid NOT_SELECTED) requests L's Firm contact data, Then the system returns 403.

### F7 — Email notifications (BRQ-10)

The list is closed. No other emails are sent.

| ID | Priority | Event | Recipient |
|----|----------|-------|-----------|
| F7.1 | MUST | Email verification requested | The registering user |
| F7.2 | MUST | Password reset requested | The user |
| F7.3 | MUST | Corporate application approved | The applicant |
| F7.4 | MUST | Corporate application rejected (includes the reason) | The applicant |
| F7.5 | MUST | New bid submitted on a listing | The Firm |
| F7.6 | MUST | Bid awarded (ACCEPTED) | The winning Workshop |
| F7.7 | MUST | Bid NOT_SELECTED because of award or cancellation | Each affected Workshop |
| F7.8 | MUST | Listing expired and awaiting decision (F3.7) | The Firm |
| F7.9 | MUST | Account suspended | The user |

| ID | Priority | Requirement |
|----|----------|-------------|
| F7.10 | MUST | Emails are in Turkish and contain no phone number or password. |
| F7.11 | MUST | A failed send is retried up to 3 times; the failure is logged and never blocks the triggering action. |

### F8 — Administration (BRQ-02, BRQ-11)

| ID | Priority | Requirement |
|----|----------|-------------|
| F8.1 | MUST | An Admin sees a list of applications with status PENDING_REVIEW, oldest first. |
| F8.2 | MUST | The Admin can open an application and approve it (status APPROVED) or reject it with a reason of 10–500 characters. |
| F8.3 | MUST | The Admin can suspend an APPROVED account with a reason; status becomes SUSPENDED. A suspended user cannot log in. |
| F8.4 | MUST | The Admin can reinstate a SUSPENDED account; status becomes APPROVED. |
| F8.5 | MUST | When an account is suspended, its OPEN listings become CANCELLED (F3.5 effects apply) and its SUBMITTED bids become WITHDRAWN. |
| F8.6 | MUST | The Admin can remove an OPEN listing with a reason; status becomes REMOVED and F3.5 effects apply to its bids. |
| F8.7 | MUST | The Admin can read all accounts, listings and bids. |
| F8.8 | WON'T | Admin roles and permissions, bulk actions, admin-created listings. |

### F9 — Audit log (BRQ-12)

| ID | Priority | Requirement |
|----|----------|-------------|
| F9.1 | MUST | The System writes an audit record for every account, listing and bid status change, and for every bid revision (F5.3). |
| F9.2 | MUST | A record contains: entity type, entity id, actor id (or SYSTEM), old value, new value, reason (if any), UTC timestamp. |
| F9.3 | MUST | Audit records cannot be edited or deleted through the application. |
| F9.4 | MUST | The Admin can view the audit records of any entity, newest first. |
| F9.5 | WON'T | Export, search, retention policy. |

---

## 3. Data Fields (closed lists)

### 3.1 Corporate application

| Field | Rule |
|-------|------|
| `legal_name` | Text, 2–150 characters, mandatory |
| `vkn` | Exactly 10 digits, mandatory, unique among accounts |
| `city` | Text, 2–60 characters, mandatory |
| `phone` | Digits with optional leading `+`, 10–15 digits, mandatory |
| `contact_person` | Text, 2–100 characters, mandatory |
| `monthly_capacity` | Integer ≥1 pieces per month, mandatory for Workshop, absent for Firm |
| `services` | One or more of `CMT`, `FULL_PACKAGE`; mandatory for Workshop, absent for Firm |

### 3.2 Listing

| Field | Rule |
|-------|------|
| `title` | Text, 5–120 characters, mandatory |
| `garment_category` | One of `T_SHIRT`, `SHIRT`, `TROUSERS`, `JACKET`, `DRESS`, `KNITWEAR`, `HOME_TEXTILE`, mandatory |
| `service_type` | One of `CMT` (cut-make-trim), `FULL_PACKAGE`, mandatory |
| `quantity` | Integer 1–1,000,000 pieces, mandatory |
| `delivery_date` | Date, later than `bid_deadline`, mandatory |
| `bid_deadline` | Date and time in Europe/Istanbul, mandatory |
| `city` | Text, 2–60 characters, mandatory (delivery or production location preference) |
| `target_unit_price` | TRY, >0, up to 2 decimals, optional; visible to workshops if set |
| `description` | Text, 0–2000 characters, optional |
| `attachments` | 0–3 files, PDF/JPG/PNG, ≤5 MB each |

### 3.3 Bid

| Field | Rule |
|-------|------|
| `unit_price` | TRY, >0, up to 2 decimals, mandatory |
| `lead_time_days` | Integer 1–365, mandatory |
| `offered_quantity` | Integer 1 to the listing's `quantity`, mandatory |
| `note` | Text, 0–1000 characters, optional |

Values in §3.2 and §3.3 (categories, limits) are proposals to be confirmed in OI-4.

---

## 4. State Machines

### 4.1 Account status

| From | To | Trigger |
|------|----|---------|
| (new) | PENDING_EMAIL | Registration (F1.1) |
| PENDING_EMAIL | PENDING_APPLICATION | Email verified (F1.4) |
| PENDING_APPLICATION | PENDING_REVIEW | Application submitted (F2.2) |
| PENDING_REVIEW | APPROVED | Admin approves (F8.2) |
| PENDING_REVIEW | REJECTED | Admin rejects (F8.2) |
| REJECTED | PENDING_REVIEW | User resubmits (F2.4) |
| APPROVED | SUSPENDED | Admin suspends (F8.3) |
| SUSPENDED | APPROVED | Admin reinstates (F8.4) |

No other transitions exist.

### 4.2 Listing status

| From | To | Trigger |
|------|----|---------|
| (new) | DRAFT | Created (F3.1) |
| DRAFT | OPEN | Published (F3.3) |
| DRAFT | CANCELLED | Firm cancels (F3.5) |
| OPEN | CANCELLED | Firm cancels (F3.5) or account suspended (F8.5) |
| OPEN | CLOSED_NO_AWARD | Firm closes (F3.6) |
| OPEN | EXPIRED_REVIEW | Deadline passes (F3.7) |
| OPEN | AWARDED | Firm awards (F6.5) |
| OPEN | REMOVED | Admin removes (F8.6) |
| EXPIRED_REVIEW | AWARDED | Firm awards within 14 days (F6.4) |
| EXPIRED_REVIEW | CLOSED_NO_AWARD | Firm closes, or 14 days pass |

No other transitions exist.

### 4.3 Bid status

| From | To | Trigger |
|------|----|---------|
| (new) | SUBMITTED | Workshop submits (F5.1) |
| SUBMITTED | WITHDRAWN | Workshop withdraws (F5.4) or account suspended (F8.5) |
| SUBMITTED | ACCEPTED | Firm awards (F6.5) |
| SUBMITTED | NOT_SELECTED | Another bid awarded, or listing cancelled/removed (F3.5, F6.5, F8.6) |

No other transitions exist. When a listing becomes CLOSED_NO_AWARD, its SUBMITTED bids become NOT_SELECTED.

---

## 5. Screens (closed list)

| # | Screen | Actor | Covers |
|---|--------|-------|--------|
| S1 | Register / Login / Password reset | Visitor | F1 |
| S2 | Corporate application and status | Firm, Workshop | F2 |
| S3 | My listings | Firm | F3.8 |
| S4 | Listing form (create/edit) | Firm | F3 |
| S5 | Listing detail with bids table and award | Firm | F6 |
| S6 | Open listings (filters, sort, pagination) | Workshop | F4 |
| S7 | Listing detail with bid form | Workshop | F4.4, F5 |
| S8 | My bids | Workshop | F5.5 |
| S9 | Application review list and detail | Admin | F8.1, F8.2 |
| S10 | Accounts list (suspend/reinstate) | Admin | F8.3, F8.4 |
| S11 | Listings moderation | Admin | F8.6, F8.7 |
| S12 | Audit log viewer | Admin | F9.4 |

Every screen has loading, empty and error states (`NFR.md` N5.4).

---

## 6. Traceability

| BRQ | FR groups |
|-----|-----------|
| BRQ-01 | F1 |
| BRQ-02 | F2, F8.1–F8.2 |
| BRQ-03 | F3 |
| BRQ-04 | F4 |
| BRQ-05 | F5 |
| BRQ-06 | F4.4, F5.7, F6.8 |
| BRQ-07 | F6.1–F6.3 |
| BRQ-08 | F6.4, F6.5, F6.7 |
| BRQ-09 | F6.6 |
| BRQ-10 | F7 |
| BRQ-11 | F8.3–F8.6 |
| BRQ-12 | F9 |
| BRQ-13 | `NFR.md` N5, N6 |
