# Non-Functional Requirements (NFR) — Tekstil360

**Version:** Draft v1.0 · 22 October 2026 · Author: Samil (11-314a)
**Traces from:** `BRD.md` §5.2 · **Related:** `PID.md`, `FR.md`, `WBS.md`

Every requirement has a target and a verification method. A requirement with no verification method is a wish, not a requirement. Where the specification of load matters, the assumed volumes in §N10 apply.

---

## N1 — Performance

| ID | Requirement | Target | Verification |
|----|-------------|--------|--------------|
| N1.1 | Page load (first meaningful content) on a mobile browser over a simulated 4G connection | ≤3 s | Lighthouse mobile run on S1, S3, S5, S6 |
| N1.2 | API response time for list and detail requests with N10 data volumes | p95 ≤500 ms | Load script, 30 concurrent users, 5 min |
| N1.3 | Listing browse (F4) with all filters applied | ≤1 s server time | Timed test with 2,000 seeded listings |
| N1.4 | Bids table (F6.1) with 100 bids | ≤1 s server time | Timed test with seeded data |
| N1.5 | Attachment upload of 5 MB | ≤10 s on 10 Mbit/s | Manual timing, 3 runs |

## N2 — Reliability and data integrity

| ID | Requirement | Target | Verification |
|----|-------------|--------|--------------|
| N2.1 | Award is atomic (F6.5) | 0 partial awards | Fault injection between the writes; check all-or-nothing |
| N2.2 | At most one ACCEPTED bid per listing | 0 violations | Database constraint plus a concurrent double-award test |
| N2.3 | At most one active bid per workshop per listing | 0 violations | Database constraint plus a concurrent double-submit test |
| N2.4 | Deadline handling (F3.7) runs even if the server restarts | Listing set to EXPIRED_REVIEW within 5 min of the deadline | Restart test around a deadline |
| N2.5 | Email failure does not block the triggering action (F7.11) | 0 blocked actions | Fault injection on the email provider |
| N2.6 | Data survives restarts and redeploys | 0 lost records | Restart and redeploy with seeded data; compare counts |
| N2.7 | Backups | Daily database backup, restore tested once | Restore into a clean environment |
| N2.8 | Invalid state transitions are rejected (`FR.md` §4) | 100% | One automated test per forbidden transition |

## N3 — Security and access control

| ID | Requirement | Target | Verification |
|----|-------------|--------|--------------|
| N3.1 | Transport encryption | HTTPS only; HTTP redirects | TLS scan of the public URL |
| N3.2 | Password storage | Salted hash with a modern algorithm (argon2id or bcrypt); no plain text in database or logs | Code review; database inspection |
| N3.3 | Session cookies | `HttpOnly`, `Secure`, `SameSite=Lax` | Browser inspection |
| N3.4 | Role-based access: each endpoint allows only its listed roles | 100% of endpoints | Automated matrix: every endpoint × every role (including Visitor) |
| N3.5 | Ownership checks: a Firm reads only its own listings' bids; a Workshop reads only its own bids (RULE-03) | 0 leaks | Automated test with 2 firms and 3 workshops; also direct-URL and API tests |
| N3.6 | Status gates: unapproved or suspended accounts cannot publish or bid (RULE-01) | 100% | Automated test for each non-APPROVED status |
| N3.7 | Contact data (F6.6) is absent from every response before award, including API payloads | 0 occurrences | Response-body scan in tests |
| N3.8 | Input validation on every field in `FR.md` §3, server-side | 100% of fields | One invalid-input test per field |
| N3.9 | Protection against SQL injection, XSS and CSRF | 0 findings | Parameterised queries only; output escaping; CSRF token or same-site protection; manual test of each form |
| N3.10 | Login and reset-request throttling | ≤10 attempts per IP per 15 min | Scripted attempts |
| N3.11 | Uploaded files: type checked by content, size limited, stored outside the web root, served only to authorised users | 100% | Test uploads of a renamed `.exe`, an oversized file, and a direct URL without login |
| N3.12 | Secrets (database URL, email key) outside the repository | 0 secrets in git history | Repository scan |

## N4 — Personal data and compliance

General orientation for planning, not legal advice. The law applicable to the real deployment (Turkey's personal-data law, KVKK) should be confirmed with a qualified adviser before real use `[OI-6]`.

| ID | Requirement | Target | Verification |
|----|-------------|--------|--------------|
| N4.1 | Store only the fields listed in `FR.md` §3 and account fields in `BRD.md` §5.3 | 0 extra personal fields | Schema review against the lists |
| N4.2 | Privacy notice shown at registration, with explicit acceptance recorded (timestamp) | 100% of accounts | Registration test |
| N4.3 | Personal data never written to application logs | 0 occurrences | Log scan after the acceptance test run |
| N4.4 | Declared data location (hosting region) documented in the README | Present | Document check |
| N4.5 | Test and demo data are synthetic | 100% | Seed-script review |

## N5 — Usability and localisation

| ID | Requirement | Target | Verification |
|----|-------------|--------|--------------|
| N5.1 | All user-visible text, emails and validation messages are in Turkish | 100% | Screen-by-screen review of S1–S12; email review |
| N5.2 | Dates and times shown in Europe/Istanbul, format DD.MM.YYYY HH:mm; prices as TRY with 2 decimals | 100% | Review of all screens with prices and dates |
| N5.3 | Task success without help (PID M4) | ≥80% of ≥5 participants | Observed tests |
| N5.4 | Every screen has loading, empty and error states | 12 of 12 screens | Checklist per screen |
| N5.5 | Form errors shown next to the field and keep user input | 100% of forms | Manual test with invalid input |
| N5.6 | Destructive or final actions require confirmation (cancel, close, award, suspend, remove) | 100% | Manual test of each action |
| N5.7 | Basic accessibility: text contrast ≥4.5:1, all form fields labelled, full keyboard navigation of S1 and S7 | Lighthouse accessibility score ≥90 | Lighthouse run on S1, S5, S7 |

## N6 — Compatibility

| ID | Requirement | Target | Verification |
|----|-------------|--------|--------------|
| N6.1 | Browsers: latest two versions of Chrome, Firefox, Safari, Edge | All S1–S12 usable | Manual smoke test per browser |
| N6.2 | Viewports: 360 px to 1920 px wide; no horizontal scrolling except inside tables | 100% | Browser device emulation at 360, 768, 1280, 1920 |
| N6.3 | The bids table (S5) remains usable at 360 px | Readable and sortable | Manual test |
| N6.4 | Works without installing anything (no plugin, no app) | Yes | Test on a clean browser profile |

## N7 — Maintainability and testing

| ID | Requirement | Target | Verification |
|----|-------------|--------|--------------|
| N7.1 | Every `MUST` acceptance criterion in `FR.md` has an automated test | 100% | Traceability table AC → test, in the README |
| N7.2 | Continuous integration runs build, lint and tests on each pull request | Green before merge | CI history |
| N7.3 | Each significant technical decision recorded as an ADR | ≥1 ADR per decision in `WBS.md` §5 | Repository check |
| N7.4 | README lets a new person run the project from a clean machine | ≤15 min, ≤10 commands | Dry run on a clean machine |
| N7.5 | Database changes through versioned migrations only | 100% | Repository check |

## N8 — Operability

| ID | Requirement | Target | Verification |
|----|-------------|--------|--------------|
| N8.1 | Public URL available during the defence week | ≥99% | Uptime check on the URL |
| N8.2 | Structured logs with request id and error level, without personal data (N4.3) | Present | Log sample |
| N8.3 | Health endpoint returns status of the application and the database | Present | Request test |
| N8.4 | One command (or one pipeline) deploys the application | 1 step | Redeploy test |
| N8.5 | A seed script creates the admin account and a synthetic demo dataset | Present | Run on an empty database |

## N9 — Capacity limits

| ID | Requirement | Target | Verification |
|----|-------------|--------|--------------|
| N9.1 | Listing pages and bid tables are paginated or limited | 20 listings per page; bids table up to 200 rows | Test with seeded data |
| N9.2 | Attachment storage | ≤ 100 MB total | Storage usage check after seeding |

## N10 — Assumed volumes (basis for N1–N9)

| Item | Assumed maximum |
|------|-----------------|
| Accounts | 500 |
| Listings | 2,000 |
| Bids | 20,000 |
| Concurrent users | 30 |
| Emails per day | 500 |

---

## Traceability

| NFR group | Supports |
|-----------|----------|
| N1, N2 | BRQ-03 … BRQ-08, R-06 |
| N3 | BRQ-01, BRQ-02, BRQ-06, BRQ-09, BRQ-11; RULE-01, RULE-03, RULE-05 |
| N4 | R-07 |
| N5, N6 | BRQ-13 |
| N7, N8 | R-05, G-06 |

## Open issues

| # | Issue | Owner | Target |
|---|-------|-------|--------|
| OI-6 | Confirm which data-protection law and hosting region apply to the real deployment | Samil | Week 6 |
| OI-7 | Confirm the volumes in N10 against what real users expect | Samil | Week 4 |
