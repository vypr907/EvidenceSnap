# EvidenceSnap Threat Model

> Initial lightweight threat model for the EvidenceSnap MVP.

EvidenceSnap stores compliance evidence, control mappings, review decisions, audit logs, and export packages. Even during MVP development, the system should be designed with security risks in mind.

This threat model is intentionally lightweight and should evolve as the product matures.

---

<a id="table-of-contents"></a>

<details open>
<summary><strong>📚 Table of Contents</strong></summary>

- [1. Purpose](#purpose)
- [2. Scope](#scope)
- [3. Assets](#assets)
- [4. Actors](#actors)
- [5. Trust Boundaries](#trust-boundaries)
- [6. High-Level Data Flow](#high-level-data-flow)
- [7. Key Threats](#key-threats)
- [8. Threat Scenarios](#threat-scenarios)
- [9. MVP Security Controls](#mvp-security-controls)
- [10. Risks Accepted During Prototype](#risks-accepted-during-prototype)
- [11. Security Requirements Before Customer Data](#security-requirements-before-customer-data)
- [12. Testing and Validation](#testing-and-validation)
- [13. Future Threat Model Enhancements](#future-threat-model-enhancements)
- [14. Related Documents](#related-documents)

</details>

---

<a id="purpose"></a>

## 1. Purpose

This document identifies early security threats for EvidenceSnap and defines initial mitigation strategies.

The goal is not to create a perfect enterprise threat model.

The goal is to make sure the MVP is designed with obvious security risks in mind.

[Back to top](#table-of-contents)

---

<a id="scope"></a>

## 2. Scope

This threat model covers the initial SaaS-native MVP.

In scope:

- Next.js application
- Supabase Auth
- Supabase Postgres
- Supabase Storage
- Organization tenancy
- RBAC
- Evidence uploads
- Signed URLs
- Audit logging
- Export packages
- GitHub/Vercel development workflow

Out of scope for now:

- enterprise SSO
- deep scanner integrations
- AI processing pipelines
- billing workflows
- full OSCAL package exchange
- third-party customer integrations
- mobile app

[Back to top](#table-of-contents)

---

<a id="assets"></a>

## 3. Assets

Important assets include:

| Asset | Sensitivity | Notes |
|---|---|---|
| User accounts | High | Authentication and access control |
| Organization records | Medium | Tenant metadata |
| Membership records | High | Determines authorization |
| Evidence Requirements | Medium/High | Compliance mapping and expectations |
| Evidence Requests | High | Operational compliance state |
| Evidence Items | High | Uploaded proof, screenshots, reports, exports |
| Export Packages | High | Bundled evidence and audit data |
| Audit Logs | High | Security and compliance traceability |
| Supabase service role key | Critical | Bypasses RLS |
| Storage signed URLs | High | Temporary evidence access |
| Environment variables | High/Critical | Secrets and configuration |

[Back to top](#table-of-contents)

---

<a id="actors"></a>

## 4. Actors

### Legitimate Actors

| Actor | Description |
|---|---|
| Admin | Manages workspace/users/settings |
| Compliance Manager | Operates evidence workflow |
| Evidence Owner | Submits assigned evidence |
| Reviewer | Reviews and approves/rejects evidence |
| Auditor / Read-only | Views evidence and exports |
| Developer | Maintains application and infrastructure |

### Threat Actors

| Actor | Description |
|---|---|
| Unauthenticated attacker | Attempts public access or auth bypass |
| Malicious tenant user | Authorized user attempting unauthorized actions |
| Cross-tenant attacker | User from one org attempting to access another org |
| Compromised user | Legitimate account controlled by attacker |
| Accidental insider | User accidentally exposing data |
| Supply chain attacker | Malicious dependency or compromised package |
| Secret thief | Attacker obtaining API keys or service role keys |

[Back to top](#table-of-contents)

---

<a id="trust-boundaries"></a>

## 5. Trust Boundaries

Primary trust boundaries:

```text
User Browser
    ↕
Next.js Application
    ↕
Supabase Auth / Postgres / Storage
    ↕
External Services
```

Important boundaries:

- browser to server
- authenticated to unauthenticated
- one Organization to another Organization
- application code to database
- application code to private storage
- signed URL user to storage object
- development secrets to deployed secrets
- preview environment to production environment

[Back to top](#table-of-contents)

---

<a id="high-level-data-flow"></a>

## 6. High-Level Data Flow

```text
User authenticates
    ↓
Supabase Auth returns session
    ↓
App resolves user profile and memberships
    ↓
User selects or resolves current Organization
    ↓
App queries tenant-scoped records by org_id
    ↓
RLS enforces membership-based access
    ↓
User uploads evidence
    ↓
File stored in private Supabase Storage path
    ↓
Evidence Item metadata stored in Postgres
    ↓
Reviewer approves/rejects request
    ↓
Audit Log records action
    ↓
Export Package generated from tenant-scoped data
```

[Back to top](#table-of-contents)

---

<a id="key-threats"></a>

## 7. Key Threats

| Threat | Impact | Initial Mitigation |
|---|---|---|
| Cross-tenant data exposure | Critical | `org_id` filtering + RLS + tests |
| Public evidence files | Critical | Private buckets + signed URLs |
| Service role key exposure | Critical | Server-only key handling |
| Broken role checks | High | RBAC helpers + server-side checks |
| Unauthorized export access | High | Tenant filtering + signed URL checks |
| Audit log tampering | High | Append-only behavior later; restricted updates |
| Malicious file upload | High | File type/size limits; scanning later |
| Stale or misleading evidence | Medium | Validation metadata and reviewer workflow |
| Dependency compromise | High | Dependabot, CodeQL, minimal dependencies |
| Secret committed to repo | Critical | `.gitignore`, secret scanning, rotation |
| Weak environment separation | Medium/High | Separate production before customer data |
| Accidental deletion | Medium | Soft deletes and backups later |

[Back to top](#table-of-contents)

---

<a id="threat-scenarios"></a>

## 8. Threat Scenarios

### Scenario 1: Cross-Tenant Evidence Access

A user from Organization A attempts to access Evidence Requests or Evidence Items from Organization B.

Mitigations:

- tenant-scoped queries
- RLS policies
- membership checks
- signed URL authorization checks
- tenant isolation tests

### Scenario 2: Public Evidence Bucket

Evidence files are accidentally stored in a public bucket.

Mitigations:

- private Supabase Storage bucket
- signed URLs only
- storage configuration review
- no public object URLs
- production checklist

### Scenario 3: Service Role Key Exposed to Browser

The Supabase service role key is accidentally used in client-side code.

Mitigations:

- server-only environment variable naming
- code review
- documentation
- secret scanning
- no service role key in client components
- rotation if exposed

### Scenario 4: Unauthorized Approval

An Evidence Owner approves their own evidence without reviewer permissions.

Mitigations:

- RBAC helper functions
- server-side role checks
- reviewer assignment checks
- audit logs
- tests

### Scenario 5: Export Package Includes Wrong Tenant Data

Export query does not filter by Organization ID and includes another tenant’s evidence.

Mitigations:

- central export builder
- required `org_id` filter
- export tests
- audit logs
- RLS

### Scenario 6: Signed URL Shared Outside Organization

A valid signed URL is shared externally.

Mitigations:

- short-lived signed URLs
- signed URL generated only after authorization
- eventual download audit logs
- avoid long-lived URLs

### Scenario 7: Malicious Upload

A user uploads a malicious file.

Mitigations for MVP:

- file size limits
- file type allowlist
- do not execute uploaded files
- private storage

Future mitigations:

- malware scanning
- content disarm/reconstruction
- stricter file validation

### Scenario 8: Secret Committed to GitHub

A secret is committed to the repo.

Mitigations:

- `.gitignore`
- `.env.example`
- GitHub secret scanning
- rotation procedure
- avoid screenshots containing secrets

[Back to top](#table-of-contents)

---

<a id="mvp-security-controls"></a>

## 9. MVP Security Controls

MVP security controls should include:

- Supabase Auth
- Organization Memberships
- role-based access checks
- `org_id` on tenant-scoped records
- RLS on tenant-scoped tables where feasible
- private storage bucket
- signed URLs
- audit logging for key actions
- `.env.local` ignored
- `.env.example` committed
- GitHub secret scanning
- Dependabot
- CodeQL
- TypeScript strict mode
- basic tenant isolation tests

[Back to top](#table-of-contents)

---

<a id="risks-accepted-during-prototype"></a>

## 10. Risks Accepted During Prototype

During early prototype development, these risks are accepted only because the system should contain demo/test data.

| Accepted Risk | Reason |
|---|---|
| Single Supabase project may be used | Zero-cost constraint |
| Preview and dev may share data | Simpler early setup |
| Email reminders may be mocked | Avoid paid provider early |
| Background jobs may be manual | Reduce complexity |
| Backups may be limited | Demo data only |
| File malware scanning absent | MVP scope, no real customer evidence |
| Observability limited to platform logs | Free-first constraint |

These risks must be revisited before storing real customer evidence.

[Back to top](#table-of-contents)

---

<a id="security-requirements-before-customer-data"></a>

## 11. Security Requirements Before Customer Data

Before storing real customer evidence:

- [ ] Separate production Supabase project exists.
- [ ] Production storage bucket is private.
- [ ] Production env vars are separate.
- [ ] RLS policies are enabled and reviewed.
- [ ] Tenant isolation tests pass.
- [ ] Service role key is server-side only.
- [ ] Signed URLs are short-lived.
- [ ] Evidence exports are tenant-filtered.
- [ ] Audit logs capture key actions.
- [ ] Backup expectations are documented.
- [ ] Retention expectations are documented.
- [ ] Terms/disclaimer language exists.
- [ ] Demo data is not mixed with production data.
- [ ] Threat model is reviewed.

[Back to top](#table-of-contents)

---

<a id="testing-and-validation"></a>

## 12. Testing and Validation

Security-critical test cases:

```text
Unauthenticated users cannot access dashboard data.
User without membership cannot access an organization.
User from Org A cannot read Org B data.
User from Org A cannot generate signed URL for Org B file.
Evidence Owner cannot approve evidence without reviewer role.
Auditor cannot modify evidence records.
Export package only includes records from selected org.
Rejected evidence requires a comment.
Waived evidence requires a reason.
```

Validation should include:

- unit tests for tenant helpers
- integration tests for RLS policies
- role permission tests
- export filtering tests
- storage path tests
- signed URL authorization tests

[Back to top](#table-of-contents)

---

<a id="future-threat-model-enhancements"></a>

## 13. Future Threat Model Enhancements

Future versions should expand this document with:

- STRIDE analysis
- data classification model
- abuse cases
- file upload security design
- incident response process
- vulnerability disclosure policy
- AI processing risks
- third-party integration risks
- billing/payment threat model
- MSP multi-client threat model
- auditor access model
- OSCAL/export package integrity model

[Back to top](#table-of-contents)

---

<a id="related-documents"></a>

## 14. Related Documents

- [`docs/security/tenant-isolation.md`](./tenant-isolation.md)
- [`docs/security/secrets-management.md`](./secrets-management.md)
- [`docs/setup/environment-variables.md`](../setup/environment-variables.md)
- [`docs/architecture/technical-stack.md`](../architecture/technical-stack.md)
- [`docs/architecture/data-model.md`](../architecture/data-model.md)
- [`docs/decisions/ADR-0002-technical-stack.md`](../decisions/ADR-0002-technical-stack.md)

[Back to top](#table-of-contents)