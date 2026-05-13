# Tenant Isolation

> Security design notes for isolating EvidenceSnap customer data by Organization.

Tenant isolation is one of the most important security requirements in EvidenceSnap.

EvidenceSnap stores compliance evidence, review records, audit logs, and export packages. A user from one Organization must never be able to access another Organization’s data.

---

<a id="table-of-contents"></a>

<details open>
<summary><strong>📚 Table of Contents</strong></summary>

- [1. Purpose](#purpose)
- [2. Core Principle](#core-principle)
- [3. Tenant Model](#tenant-model)
- [4. Tenant-Scoped Tables](#tenant-scoped-tables)
- [5. Membership Model](#membership-model)
- [6. Application-Layer Isolation](#application-layer-isolation)
- [7. Database-Layer Isolation](#database-layer-isolation)
- [8. Storage Isolation](#storage-isolation)
- [9. Signed URL Rules](#signed-url-rules)
- [10. Audit Logging](#audit-logging)
- [11. Testing Tenant Isolation](#testing-tenant-isolation)
- [12. Common Failure Modes](#common-failure-modes)
- [13. MVP Checklist](#mvp-checklist)
- [14. Production Checklist](#production-checklist)
- [15. Related Documents](#related-documents)

</details>

---

<a id="purpose"></a>

## 1. Purpose

This document defines the tenant isolation strategy for EvidenceSnap.

It should guide:

- database design
- API design
- Supabase RLS policies
- storage path design
- signed URL generation
- tests
- security reviews

[Back to top](#table-of-contents)

---

<a id="core-principle"></a>

## 2. Core Principle

Every tenant-scoped record must be tied to an Organization.

Every tenant-scoped query must be filtered by Organization ID.

```sql
SELECT *
FROM evidence_requests
WHERE org_id = :current_org_id;
```

Frontend filtering is not sufficient.

Tenant isolation must be enforced at multiple layers.

[Back to top](#table-of-contents)

---

<a id="tenant-model"></a>

## 3. Tenant Model

The primary tenant object is:

```text
Organization
```

A User accesses an Organization through:

```text
Membership
```

Relationship:

```text
User
  ↓
Membership
  ↓
Organization
```

A user may eventually belong to multiple Organizations.

The currently selected Organization determines the tenant scope for app queries.

[Back to top](#table-of-contents)

---

<a id="tenant-scoped-tables"></a>

## 4. Tenant-Scoped Tables

The following tables must include `org_id`:

```text
systems
evidence_requirements
evidence_periods
evidence_requests
evidence_items
review_comments
export_packages
audit_logs
```

The following tables may be global, tenant-scoped, or hybrid:

```text
frameworks
controls
```

For MVP simplicity, frameworks and controls may be tenant-scoped or copied into each Organization.

[Back to top](#table-of-contents)

---

<a id="membership-model"></a>

## 5. Membership Model

Membership determines access.

Suggested fields:

| Field | Purpose |
|---|---|
| `id` | Membership ID |
| `org_id` | Organization ID |
| `user_id` | User ID |
| `role` | User role in the Organization |
| `status` | invited, active, disabled |
| `created_at` | Creation timestamp |
| `updated_at` | Last update timestamp |

Suggested roles:

```text
Admin
Compliance Manager
Evidence Owner
Reviewer
Auditor / Read-only
```

Access requires:

1. authenticated user
2. active membership
3. appropriate role
4. matching `org_id`

[Back to top](#table-of-contents)

---

<a id="application-layer-isolation"></a>

## 6. Application-Layer Isolation

Application code must resolve the current Organization before querying tenant data.

Recommended helper pattern:

```text
getCurrentUser()
getCurrentMembership()
getCurrentOrg()
requireOrgAccess(orgId)
requireRole(orgId, allowedRoles)
```

### Query Rule

Never write tenant-scoped queries without `org_id`.

Bad:

```sql
SELECT *
FROM evidence_requests;
```

Good:

```sql
SELECT *
FROM evidence_requests
WHERE org_id = :current_org_id;
```

### Server-Side Checks

Before sensitive actions:

- verify user is authenticated
- verify active membership
- verify role
- verify target record belongs to the same `org_id`

Sensitive actions include:

- evidence upload
- evidence approval
- evidence rejection
- waiver
- export generation
- user invitation
- role change
- signed URL generation

[Back to top](#table-of-contents)

---

<a id="database-layer-isolation"></a>

## 7. Database-Layer Isolation

Supabase Row Level Security should be used for defense in depth.

### RLS Direction

Enable RLS on tenant-scoped tables.

Example:

```sql
ALTER TABLE evidence_requests ENABLE ROW LEVEL SECURITY;
```

Example read policy concept:

```sql
CREATE POLICY "Members can read org evidence requests"
ON evidence_requests
FOR SELECT
USING (
  org_id IN (
    SELECT org_id
    FROM memberships
    WHERE user_id = auth.uid()
      AND status = 'active'
  )
);
```

Example insert policy concept:

```sql
CREATE POLICY "Members can insert org evidence requests"
ON evidence_requests
FOR INSERT
WITH CHECK (
  org_id IN (
    SELECT org_id
    FROM memberships
    WHERE user_id = auth.uid()
      AND status = 'active'
  )
);
```

### Role-Based Policies

Some actions require stricter role checks.

Example concepts:

- Evidence Owners can submit evidence for assigned requests.
- Reviewers can approve/reject assigned requests.
- Admins can manage users.
- Compliance Managers can manage requirements and exports.

RLS may enforce some role rules, while application logic enforces more detailed workflow rules.

[Back to top](#table-of-contents)

---

<a id="storage-isolation"></a>

## 8. Storage Isolation

Evidence files must be stored in private storage.

Suggested path pattern:

```text
/orgs/{org_id}/evidence/{period_id}/{request_id}/{evidence_item_id}/{filename}
```

Export package path pattern:

```text
/orgs/{org_id}/exports/{period_id}/{export_package_id}/package.zip
```

Storage paths must include `org_id`.

Database records must store the storage path and tenant ownership.

### Storage Rules

- Buckets should not be public.
- File paths should include Organization ID.
- App should verify access before generating signed URLs.
- Evidence Items should store file metadata.
- File download events should eventually be audit logged.

[Back to top](#table-of-contents)

---

<a id="signed-url-rules"></a>

## 9. Signed URL Rules

Signed URLs should only be generated after authorization checks.

Before generating a signed URL:

1. authenticate user
2. resolve current Organization
3. verify membership
4. verify the Evidence Item belongs to the Organization
5. verify user role/permission
6. generate short-lived signed URL
7. optionally log access

Signed URLs should:

- expire
- be scoped to one object
- not be stored permanently
- not be committed or pasted into public docs

[Back to top](#table-of-contents)

---

<a id="audit-logging"></a>

## 10. Audit Logging

Tenant-sensitive actions should be audit logged.

Examples:

- evidence uploaded
- evidence downloaded, eventually
- evidence approved
- evidence rejected
- evidence waived
- export package generated
- export package downloaded, eventually
- user invited
- user role changed
- requirement changed
- period locked

Audit logs must include:

```text
org_id
actor_user_id
action
entity_type
entity_id
created_at
```

Optional:

```text
before_json
after_json
ip_address
user_agent
```

[Back to top](#table-of-contents)

---

<a id="testing-tenant-isolation"></a>

## 11. Testing Tenant Isolation

Tenant isolation tests are high priority.

Critical test cases:

```text
User from Org A cannot read Org B Evidence Requests.
User from Org A cannot upload evidence to Org B request.
User from Org A cannot generate signed URL for Org B file.
User without membership cannot access org dashboard.
Disabled membership cannot access org records.
Auditor cannot approve evidence.
Evidence Owner cannot manage organization users.
Export package only includes records from selected org.
```

Tests should cover:

- app-layer helpers
- RLS policy behavior
- storage path generation
- signed URL authorization
- export package filtering

[Back to top](#table-of-contents)

---

<a id="common-failure-modes"></a>

## 12. Common Failure Modes

| Failure | Risk | Mitigation |
|---|---|---|
| Missing `org_id` filter | Cross-tenant data exposure | Tenant query helpers and tests |
| Public storage bucket | Evidence leak | Private buckets only |
| Service role key in browser | RLS bypass | Server-only usage |
| Signed URL generated without auth check | Unauthorized file access | Central signed URL helper |
| Export query lacks tenant filter | Cross-tenant evidence package | Export tests |
| User role checked only in UI | Privilege escalation | Server-side role checks |
| Shared dev/prod data | Accidental exposure | Separate production before customer data |

[Back to top](#table-of-contents)

---

<a id="mvp-checklist"></a>

## 13. MVP Checklist

Before MVP demo:

- [ ] Tenant-scoped tables include `org_id`.
- [ ] Application queries filter by `org_id`.
- [ ] Membership model exists.
- [ ] Role model exists.
- [ ] RLS enabled on tenant-scoped tables where feasible.
- [ ] Evidence storage paths include `org_id`.
- [ ] Buckets are private.
- [ ] Signed URLs require authorization checks.
- [ ] Audit logs include `org_id`.
- [ ] Basic tenant isolation tests exist.

[Back to top](#table-of-contents)

---

<a id="production-checklist"></a>

## 14. Production Checklist

Before real customer evidence:

- [ ] Separate production Supabase project exists.
- [ ] Production storage bucket is private.
- [ ] RLS policies reviewed.
- [ ] Service role key is server-only.
- [ ] Vercel production env vars configured.
- [ ] Demo data removed from production.
- [ ] Backup expectations documented.
- [ ] Retention expectations documented.
- [ ] Tenant isolation tests pass.
- [ ] Export package tenant filtering tested.
- [ ] Signed URL behavior tested.
- [ ] Security threat model reviewed.

[Back to top](#table-of-contents)

---

<a id="related-documents"></a>

## 15. Related Documents

- [`docs/architecture/data-model.md`](../architecture/data-model.md)
- [`docs/architecture/technical-stack.md`](../architecture/technical-stack.md)
- [`docs/security/secrets-management.md`](./secrets-management.md)
- [`docs/security/threat-model.md`](./threat-model.md)
- [`docs/setup/environment-variables.md`](../setup/environment-variables.md)
- [`docs/decisions/ADR-0002-technical-stack.md`](../decisions/ADR-0002-technical-stack.md)

[Back to top](#table-of-contents)