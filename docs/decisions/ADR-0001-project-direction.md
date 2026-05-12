# ADR-0001: Project Direction and SaaS-Native Prototype

> Decision record for the initial EvidenceSnap product and architecture direction.

| Field | Value |
|---|---|
| ADR | ADR-0001 |
| Status | Accepted |
| Date | 2026-05-12 |
| Project | EvidenceSnap |
| Decision Type | Product / Architecture |

---

<a id="table-of-contents"></a>

<details open>
<summary><strong>📚 Table of Contents</strong></summary>

- [1. Context](#context)
- [2. Decision](#decision)
- [3. Rationale](#rationale)
- [4. Alternatives Considered](#alternatives-considered)
  - [Option A: AppSheet Prototype](#option-a-appsheet-prototype)
  - [Option B: SaaS-Native Prototype](#option-b-saas-native-prototype)
  - [Option C: Documentation-Only Validation](#option-c-documentation-only-validation)
- [5. Consequences](#consequences)
  - [Positive Consequences](#positive-consequences)
  - [Negative Consequences](#negative-consequences)
- [6. Product Scope Decision](#product-scope-decision)
- [7. Compliance-as-Code Direction](#compliance-as-code-direction)
- [8. Initial Technical Direction](#initial-technical-direction)
- [9. Security Direction](#security-direction)
- [10. Business Direction](#business-direction)
- [11. Implementation Guidance](#implementation-guidance)
- [12. Decision Review Triggers](#decision-review-triggers)
- [13. Related Documents](#related-documents)

</details>

---

<a id="context"></a>

## 1. Context

EvidenceSnap is a planned compliance evidence tracking platform.

The product is intended to help small regulated teams request, collect, review, organize, and export audit-ready evidence by framework, control, system, owner, and reporting period.

The initial product idea emerged from the need for a practical system that helps teams avoid:

- Chasing evidence through email
- Tracking compliance proof in scattered spreadsheets
- Losing control-to-evidence mapping
- Manually assembling audit packages
- Forgetting recurring evidence requests
- Accepting stale or incomplete evidence
- Overbuying large enterprise GRC platforms before they need them

The founder has experience with:

- DevSecOps
- Compliance operations
- ISSO-style workflows
- Cloud/infrastructure concepts
- Python
- AppSheet
- Data modeling
- Automation design
- Documentation-heavy system design

Because of this background, there are two plausible implementation paths:

1. Build a quick workflow prototype in AppSheet.
2. Build the first prototype directly on a SaaS-native stack.

This ADR records the decision to build the first prototype as a SaaS-native application.

[Back to top](#table-of-contents)

---

<a id="decision"></a>

## 2. Decision

EvidenceSnap will begin as a **SaaS-native prototype**, not an AppSheet prototype.

The initial product will focus on the narrow evidence operations workflow:

```text
Define Evidence Requirement
        ↓
Generate Evidence Request
        ↓
Collect Evidence
        ↓
Review Evidence
        ↓
Track Missing / Overdue Items
        ↓
Export Evidence Package
```

EvidenceSnap will not start as a full GRC replacement.

EvidenceSnap will build toward evidence-as-code and later compliance-as-code capabilities, but full OSCAL support, automated compliance scoring, and deep integrations are not MVP requirements.

[Back to top](#table-of-contents)

---

<a id="rationale"></a>

## 3. Rationale

The SaaS-native prototype is preferred because the project should validate the actual long-term product architecture and demonstrate coding/DevOps capability.

Primary reasons:

- The product is intended to become a real SaaS platform.
- Tenant isolation needs to be designed and tested early.
- Authentication and RBAC need to be designed from the beginning.
- File storage, signed URLs, and audit logging are core product requirements.
- Background jobs and email reminders are part of the core workflow.
- Export package generation needs to be implemented in the target architecture.
- The founder wants to showcase software engineering, cloud, and DevOps skills.
- AppSheet expertise is already established and is not the skill area this project needs to demonstrate.
- Building in the target stack will expose real implementation bugs earlier.

The purpose of the first prototype is not only workflow validation. It is also architecture validation.

[Back to top](#table-of-contents)

---

<a id="alternatives-considered"></a>

## 4. Alternatives Considered

<a id="option-a-appsheet-prototype"></a>

### Option A: AppSheet Prototype

Build the first prototype in AppSheet using tables, forms, views, bots, and Google Drive-based evidence storage.

#### Benefits

- Fast workflow prototyping
- Familiar development environment
- Quick form and table creation
- Easy automation experimentation
- Low initial code burden

#### Drawbacks

- Does not validate the long-term SaaS architecture
- Does not showcase coding/DevOps skills as strongly
- Multi-tenant SaaS behavior would be artificial
- File storage and signed URL patterns would differ from final product
- Auth/RBAC model would not match final product
- Background job and export architecture would differ
- Risk of building AppSheet-specific workflow assumptions

#### Decision

Rejected for first prototype.

AppSheet may still be useful as a mental model for workflow design, but it is not the selected implementation platform.

[Back to top](#table-of-contents)

---

<a id="option-b-saas-native-prototype"></a>

### Option B: SaaS-Native Prototype

Build the first prototype using a real SaaS stack.

Potential stack:

- Next.js frontend
- Next.js API routes or FastAPI backend
- Postgres database
- Supabase Auth, Clerk, Auth0, or similar
- S3, Cloudflare R2, or Supabase Storage
- Background jobs through Inngest, Trigger.dev, Celery, or scheduled workers
- Email through Resend, Postmark, or SendGrid
- Stripe for future billing
- Export generation through Node or Python worker

#### Benefits

- Validates long-term architecture
- Builds useful portfolio-quality code
- Tests tenant isolation early
- Tests auth/RBAC early
- Tests real object storage and file metadata patterns
- Tests background jobs and email reminders
- Tests export generation in the actual architecture
- Supports future open-source or public repo credibility
- Better foundation for customer pilots

#### Drawbacks

- Slower than AppSheet for initial workflow prototyping
- Requires more architecture decisions early
- Requires more setup and infrastructure work
- More potential for implementation bugs
- May delay first visual demo if scope is not controlled

#### Decision

Accepted.

[Back to top](#table-of-contents)

---

<a id="option-c-documentation-only-validation"></a>

### Option C: Documentation-Only Validation

Validate the product with only docs, mockups, landing pages, and outreach before building.

#### Benefits

- Fastest market validation
- No premature engineering
- Good for discovery conversations
- Can clarify positioning and pricing

#### Drawbacks

- Does not validate implementation risks
- Does not produce a portfolio-quality technical artifact
- Does not test tenant isolation, storage, auth, jobs, or exports
- Less satisfying for technical product development
- Harder to demonstrate working value to early users

#### Decision

Partially accepted.

Documentation and discovery should happen alongside the SaaS-native prototype, but they are not a substitute for building the first working system.

[Back to top](#table-of-contents)

---

<a id="consequences"></a>

## 5. Consequences

<a id="positive-consequences"></a>

### Positive Consequences

This decision means EvidenceSnap can:

- Build on the real future architecture from day one.
- Demonstrate practical SaaS development skill.
- Expose real security and tenancy issues early.
- Create reusable database migrations and deployment workflows.
- Establish audit logging and file handling correctly from the beginning.
- Support realistic customer pilots.
- Avoid replatforming from a no-code prototype later.
- Build credibility as a serious compliance/security product.

<a id="negative-consequences"></a>

### Negative Consequences

This decision also means:

- Development will be slower at first.
- The project needs disciplined MVP scope control.
- More tooling decisions are required early.
- Infrastructure and deployment complexity will appear sooner.
- The founder must avoid overengineering before the workflow is validated.
- The first demo may take longer than an AppSheet demo.

### Mitigation

To reduce risk:

- Keep the MVP narrow.
- Build only the core evidence workflow first.
- Use managed services where practical.
- Avoid enterprise features early.
- Avoid full OSCAL support early.
- Document decisions in ADRs.
- Build iteratively with working demos.

[Back to top](#table-of-contents)

---

<a id="product-scope-decision"></a>

## 6. Product Scope Decision

EvidenceSnap will start as an evidence operations platform, not a broad GRC platform.

### MVP Must Focus On

- Organizations
- Users and roles
- Systems
- Frameworks
- Controls
- Evidence Requirements
- Evidence Periods
- Evidence Requests
- Evidence Items
- Review workflow
- Reminder emails
- Dashboard
- Export package
- Audit log

### MVP Must Avoid

- Full risk register
- Full policy management
- Vendor risk management
- Complex workflow builder
- Deep scanner integrations
- Public API
- Mobile app
- Full OSCAL SSP generation
- Automated compliance scoring
- AI compliance certification
- Full FedRAMP package generation
- Enterprise SSO unless required by an early paying customer

The guiding question for MVP scope is:

> Does this help define, request, collect, review, track, or export compliance evidence?

If yes, it may belong in the MVP.

If no, it likely belongs later.

[Back to top](#table-of-contents)

---

<a id="compliance-as-code-direction"></a>

## 7. Compliance-as-Code Direction

EvidenceSnap should support evidence-as-code before attempting full compliance-as-code.

### Evidence-as-Code Means

- Evidence requirements are structured.
- Requirements can be imported/exported as YAML or JSON.
- Templates can be versioned.
- Validation rules can be defined.
- Evidence configuration can eventually live in Git.
- Template packs can be maintained for common use cases.

### Compliance-as-Code Later Means

- Automated checks
- Policy-as-code
- Drift detection
- Continuous compliance integrations
- OSCAL mapping
- Scanner/cloud/import workflows

### Decision

Evidence-as-code is part of the product direction.

Full compliance-as-code is future scope.

Full OSCAL support is desirable later but not required for MVP.

[Back to top](#table-of-contents)

---

<a id="initial-technical-direction"></a>

## 8. Initial Technical Direction

The exact stack is not finalized, but the preferred direction is:

| Layer | Preferred Direction |
|---|---|
| Frontend | Next.js |
| Backend | Next.js API routes or FastAPI |
| Database | Postgres |
| Auth | Supabase Auth, Clerk, Auth0, or similar |
| Storage | S3, Cloudflare R2, or Supabase Storage |
| Background Jobs | Inngest, Trigger.dev, Celery, or scheduled workers |
| Email | Resend, Postmark, or SendGrid |
| Billing | Stripe |
| Export Generation | Node or Python worker |
| Deployment | Vercel, Render, Fly.io, Railway, AWS, or similar |

### Technical Priorities

The first technical priorities are:

1. Authentication
2. Organization workspace
3. Tenant isolation
4. RBAC
5. Database migrations
6. Evidence model
7. Private file storage
8. Audit logging
9. Reminder emails
10. Export package generation

[Back to top](#table-of-contents)

---

<a id="security-direction"></a>

## 9. Security Direction

Security is part of the product value.

EvidenceSnap stores compliance evidence, so basic security architecture must be present from the beginning.

### Required Security Principles

- Tenant isolation by Organization ID
- Role-based access control
- Private file storage
- Signed download URLs
- Encryption in transit
- Encryption at rest
- File hashing on upload
- Strong audit logging
- Least privilege access
- Soft deletes for important records
- Export access logging
- Regular backups
- Clear retention controls

### Critical Rule

Every tenant-scoped query must include Organization ID filtering.

Example:

```sql
SELECT *
FROM evidence_requests
WHERE org_id = :current_org_id;
```

Frontend filtering is not sufficient for tenant isolation.

[Back to top](#table-of-contents)

---

<a id="business-direction"></a>

## 10. Business Direction

EvidenceSnap should initially use a service-assisted SaaS model.

### Target Customers

Initial customer targets:

- Small government contractors
- CMMC-prep organizations
- MSPs / MSSPs managing compliance evidence
- Small SaaS companies pursuing SOC 2
- Small regulated teams with recurring evidence needs

### Initial Revenue Goal

The initial business goal is:

> Reach $800-$1,000/month recurring revenue with 3-5 early customers.

### Possible Pricing

| Plan | Price | Intended Use |
|---|---:|---|
| Starter | $99/month | One system, basic evidence tracking |
| Team | $199/month | Multiple systems, reviewers, export packages |
| Compliance Ops | $399/month | Advanced workflows, priority support |

### Possible Setup Fees

| Setup Package | Price |
|---|---:|
| Starter setup | $299 |
| Guided evidence mapping | $750 |
| Custom framework import | $1,500 |

[Back to top](#table-of-contents)

---

<a id="implementation-guidance"></a>

## 11. Implementation Guidance

### Phase 1: Repo and Documentation

- Create `README.md`
- Create `AI_CONTEXT.md`
- Create product vision documentation
- Create data model documentation
- Create evidence-as-code schema documentation
- Create ADRs for major decisions

### Phase 2: SaaS Foundation

- Create Next.js app
- Configure database
- Configure authentication
- Create Organization and Membership model
- Implement RBAC
- Implement tenant isolation patterns

### Phase 3: Evidence Workflow

- Build Systems CRUD
- Build Frameworks/Controls CRUD or seed import
- Build Evidence Requirements CRUD
- Build Evidence Period creation
- Build Evidence Request generation
- Build Evidence Item upload/link submission
- Build reviewer approve/reject workflow

### Phase 4: Trust and Operations

- Implement Audit Log
- Implement reminder emails
- Implement private file storage
- Implement file hashing
- Implement signed URLs
- Implement export package generation

### Phase 5: Pilot Readiness

- Create demo workspace
- Create generic monthly evidence template
- Create sample export package
- Add dashboard
- Add seed data
- Prepare discovery/demo materials

[Back to top](#table-of-contents)

---

<a id="decision-review-triggers"></a>

## 12. Decision Review Triggers

This decision should be reviewed if:

- The SaaS-native prototype becomes too slow to produce a demo.
- An early paying customer explicitly requires a different architecture.
- The project pivots away from SaaS and toward consulting-only delivery.
- The evidence workflow proves invalid during discovery.
- Multi-tenant architecture becomes unnecessary.
- A no-code/low-code tool becomes clearly better for the selected business model.

Until one of these triggers occurs, the SaaS-native direction remains accepted.

[Back to top](#table-of-contents)

---

<a id="related-documents"></a>

## 13. Related Documents

- [`README.md`](../../README.md)
- [`AI_CONTEXT.md`](../../AI_CONTEXT.md)
- [`docs/product/vision.md`](../product/vision.md)
- [`docs/architecture/data-model.md`](../architecture/data-model.md)
- [`docs/evidence-as-code/schema.md`](../evidence-as-code/schema.md)

[Back to top](#table-of-contents)