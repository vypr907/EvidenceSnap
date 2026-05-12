# AI_CONTEXT.md

This file exists to keep AI assistants aligned with the EvidenceSnap project.

When helping with this project, treat this file as the primary project context. Use it to preserve product direction, architecture decisions, naming conventions, roadmap state, and open questions.

---

## Project Name

EvidenceSnap

---

## Short Description

EvidenceSnap is a lightweight compliance evidence tracking platform with evidence-as-code foundations.

It helps small regulated teams request, collect, review, organize, and export audit-ready evidence by framework, control, system, owner, and reporting period.

---

## Core Product Idea

EvidenceSnap is not intended to start as a full enterprise GRC replacement.

The first version should solve a narrower and more concrete problem:

> Compliance evidence collection is scattered across email, spreadsheets, screenshots, shared drives, and one-off reminders. EvidenceSnap creates a repeatable workflow for defining what evidence is needed, assigning owners, tracking due dates, reviewing submissions, and exporting clean evidence packages.

---

## Product Positioning

Preferred positioning:

> Lightweight compliance evidence collection without spreadsheet chaos.

Alternative positioning:

> Evidence tracking for small regulated teams.

Longer positioning:

> EvidenceSnap helps small regulated teams request, collect, review, and export compliance evidence by control, system, owner, and reporting period.

Avoid positioning the product as:

- Full GRC replacement
- Automated compliance certification
- AI compliance guarantee
- One-click audit readiness
- Replacement for assessors, auditors, legal counsel, or compliance professionals

---

## Founder Context

The founder/user has experience with:

- DevSecOps
- Compliance operations
- ISSO-style workflows
- Cloud and infrastructure
- Terraform
- VMware
- Python
- AppSheet
- Automation design
- Debug logging
- Data modeling
- Personal finance / ledger-style systems
- Documentation-heavy app design

The founder is comfortable with moderate programming and DevOps work but wants practical, realistic, incremental build plans.

Assume the founder values:

- Strong documentation
- Clear data models
- Maintainable architecture
- Auditability
- Debuggability
- Practical automation
- Phased implementation
- Avoiding overbuilt MVPs
- Useful recurring revenue

---

## Target Customer

Initial target customers should be small teams with recurring compliance evidence needs but without mature enterprise GRC tooling.

Best initial niches:

1. Small government contractors preparing for CMMC or NIST 800-171-style evidence work
2. MSPs / MSSPs managing compliance evidence for clients
3. Small SaaS companies preparing for SOC 2
4. Small regulated businesses with lightweight audit evidence needs
5. Internal security/compliance teams that need evidence discipline

Do not initially target large enterprises.

Large enterprises likely already use or evaluate tools such as ServiceNow GRC, Archer, Drata, Vanta, Hyperproof, OneTrust, or similar platforms.

---

## Business Model

Preferred initial model:

> Service-assisted micro-SaaS.

The product should be sold with setup help, especially in the early stages.

Possible pricing:

- Starter: $99/month
- Team: $199/month
- Compliance Ops: $399/month

Possible setup services:

- Starter setup: $299
- Guided evidence mapping: $750
- Custom framework import: $1,500

Initial revenue goal:

> $800-$1,000/month recurring revenue with 3-5 early customers.

---

## MVP Philosophy

Build the smallest useful evidence workflow first.

Do not build a full GRC system before validating the evidence tracking workflow.

The MVP should answer:

1. What evidence is needed?
2. Who owns it?
3. When is it due?
4. What control does it support?
5. Has it been submitted?
6. Has it been reviewed?
7. What is missing?
8. Can we export a clean package?

---

## MVP Must-Have Features

The first useful version should include:

- Authentication
- Organization workspace
- User roles
- Systems
- Frameworks
- Controls
- Evidence Requirements
- Evidence Periods
- Evidence Requests
- Evidence Items
- Evidence upload or external link submission
- Reviewer approve/reject workflow
- Reminder emails
- Dashboard
- Export package
- Audit log

---

## MVP Non-Goals

Do not prioritize these in the MVP:

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
- Enterprise SSO unless required by a paying early customer

---

## Product Layers

Think of the product in three layers.

### Layer 1 — Human Workflow

This is the MVP.

Capabilities:

- Evidence requests
- Owners
- Due dates
- Uploads
- Review
- Reminders
- Exports

### Layer 2 — Evidence-as-Code

This is the next major differentiator.

Capabilities:

- YAML/JSON import/export
- Versioned evidence requirements
- Template packs
- Validation rules
- Compliance repository
- GitHub-backed configuration

### Layer 3 — Automation / Continuous Compliance

This is later.

Capabilities:

- API integrations
- Automated evidence collection
- Scanner imports
- Cloud checks
- AI evidence summaries
- Evidence relevance checks
- Drift detection
- OSCAL bridge

---

## Evidence-as-Code Direction

EvidenceSnap should support evidence-as-code before trying to support full compliance-as-code.

Evidence-as-code means:

- Evidence requirements are structured
- Requirements are importable/exportable
- Templates can be versioned
- Validation rules can be defined
- Evidence can be tied to controls, systems, periods, owners, and reviewers
- Configuration can eventually live in Git

Example evidence-as-code object:

```yaml
id: evidence.mfa-status.monthly
framework: cmmc-level-2
control_code: IA.L2-3.5.3
system: corporate-it
name: Monthly MFA Status Export
description: Export the current user MFA status report from the identity provider.
frequency: monthly
owner_role: IT Administrator
reviewer_role: Compliance Manager
due:
  day_of_month: 5
evidence_type:
  - csv
  - pdf
validation:
  max_age_days: 35
  required_fields:
    - user
    - mfa_status
    - account_status
approval:
  required: true
tags:
  - identity
  - access-control
  - monthly
```
## OSCAL Direction

OSCAL support is desirable later but should not block the MVP.

### Future mapping ideas:

| EvidenceSnap Concept	| OSCAL-like Concept |
| --------------------- | ------------------ |
|Framework	|Catalog / Profile
|Control	|Control
|System	|System Security Plan system
|Evidence Requirement	|Assessment objective / custom property
|Evidence Item	|Assessment result observation / back-matter resource
|Review Decision	|Assessment result / finding status
|Implementation Statement	|SSP control implementation
|Export Package	|OSCAL JSON/YAML package

### Important guidance:

- Avoid promising full OSCAL compliance early.
- Design the data model so future OSCAL import/export is possible.
- Start with simplified internal models.
- Add OSCAL bridge only after real evidence workflows are stable.

## Core Data Model

The initial data model should include these entities.

### Organizations
Represents a customer tenant.

Suggested fields:

- OrgID
- OrgName
- Plan
- PrimaryContact
- TimeZone
- DefaultFramework
- StorageRegion
- CreatedAt
- Active

### Users
Suggested fields:

- UserID
- OrgID
- Name
- Email
- Role
- Status
- LastLoginAt
- CreatedAt

Suggested roles:

- Admin
- Compliance Manager
- Evidence Owner
- Reviewer
- Auditor / Read-only

### Systems
Suggested fields:

- SystemID
- OrgID
- SystemName
- SystemOwner
- Description
- Environment
- Criticality
- Active

### Frameworks
Suggested fields:

- FrameworkID
- Name
- Version
- Source
- Active

### Controls
Suggested fields:

- ControlID
- FrameworkID
- ControlCode
- ControlName
- ControlText
- ControlFamily
- ControlObjective
- ImplementationGuidance
- SortOrder
- Active

### Evidence Requirements
This is one of the most important entities.

Suggested fields:

- EvidenceRequirementID
- OrgID
- ControlID
- SystemID
- RequirementName
- RequirementDescription
- EvidenceType
- Frequency
- OwnerUserID
- ReviewerUserID
- DueDay
- GracePeriod
- Required
- Active
- Version
- Tags

### Evidence Periods
Suggested fields:

- EvidencePeriodID
- OrgID
- PeriodName
- PeriodStart
- PeriodEnd
- Status
- CreatedAt
- ClosedAt

Suggested statuses:

- Open
- In Review
- Complete
- Locked
- Archived

### Evidence Requests
Generated from Evidence Requirements for a specific Evidence Period.

Suggested fields:

- EvidenceRequestID
- OrgID
- EvidenceRequirementID
- EvidencePeriodID
- ControlID
- SystemID
- OwnerUserID
- ReviewerUserID
- DueDate
- Status
- RequestedAt
- SubmittedAt
- ReviewedAt
- ReminderCount
- LastReminderAt

Suggested statuses:

- Draft
- Requested
- Submitted
- Needs Changes
- Approved
- Waived
- Overdue
- Not Applicable

### Evidence Items
Suggested fields:

- EvidenceItemID
- OrgID
- EvidenceRequestID
- FileName
- FileType
- FileSize
- StoragePath
- ExternalURL
- UploadedBy
- UploadedAt
- SHA256Hash
- Description
- EvidenceDate
- SourceSystem
- Approved
- ReviewerNotes

### Review Comments
Suggested fields:

- CommentID
- OrgID
- EvidenceRequestID
- UserID
- Comment
- CreatedAt
- Visibility

### Export Packages
Suggested fields:

- PackageID
- OrgID
- EvidencePeriodID
- FrameworkID
- SystemID
- PackageName
- Status
- GeneratedAt
- DownloadURL
- IncludedEvidenceCount
- MissingEvidenceCount

### Audit Logs
Audit logging is mandatory.

Suggested fields:

- AuditLogID
- OrgID
- ActorUserID
- EntityType
- EntityID
- Action
- BeforeJSON
- AfterJSON
- IPAddress
- UserAgent
- CreatedAt

Suggested actions:

- Created
- Updated
- Deleted
- Uploaded
- Approved
- Rejected
- Waived
- Exported
- Login
- PermissionChanged

---

## Status Logic

Evidence Request status should be deterministic where possible.

Suggested status rules:

- If request exists and has not been submitted: Requested
- If due date has passed and status is Requested: Overdue
- If evidence is uploaded: Submitted
- If reviewer approves: Approved
- If reviewer rejects: Needs Changes
- If owner resubmits after rejection: Submitted
- If compliance manager waives: Waived
- If requirement is not relevant: Not Applicable

Package readiness formula:

```Readiness % = Approved Required Requests / Total Required Requests```

Alternative completion formula:

```Acceptable Completion % = (Approved + Waived + Not Applicable) / Total Required Requests```

Waived and Not Applicable items should remain visible and auditable.

---

## Export Package Requirements

Export packages are a key product differentiator.

A package should include:

- Index file
- Control matrix
- Evidence inventory
- Missing evidence report
- Review notes
- Audit log
- Evidence files organized by control

Example package structure:
```
Evidence Package - May 2026
├── index.pdf
├── control-matrix.csv
├── evidence-inventory.csv
├── missing-evidence.csv
├── review-notes.csv
├── audit-log.csv
├── AC-2/
│   ├── user-access-review.pdf
│   └── reviewer-notes.txt
├── IA-2/
│   ├── mfa-status-export.csv
│   └── mfa-status-screenshot.png
└── RA-5/
    └── vulnerability-scan-summary.pdf
```

Control matrix fields:

```csv
Framework,Control Code,Control Name,System,Evidence Requirement,Owner,Due Date,Status,Evidence File,Reviewer,Reviewed At,Notes
```

Evidence inventory fields:

```
Evidence ID,File Name,Control Code,Requirement,Uploaded By,Uploaded At,SHA256 Hash,Status,Reviewer
```

Missing evidence fields:

```
Control Code,Requirement,System,Owner,Due Date,Days Overdue,Last Reminder,Required
```

---

## Security Principles
Security must be included from the beginning because the product stores sensitive compliance evidence.

Core requirements:

- Tenant isolation by OrgID
- Role-based access control
- MFA support eventually
- Strong audit logging
- Private object storage
- Signed URLs for file download
- Encryption in transit
- Encryption at rest
- File hashing on upload
- Least privilege access
- Secure defaults
- Soft delete for important records
- Export access logging
- Regular backups
- Clear retention controls

Important architectural rule:

```Every tenant-scoped query must include OrgID filtering. Never rely only on frontend filtering.```

Example:

```sql
SELECT *
FROM evidence_requests
WHERE org_id = current_user.org_id;
```

Suggested storage path pattern:

```/orgs/{org_id}/evidence/{period_id}/{request_id}/{evidence_item_id}/{filename}```

---

## Recommended Tech Stack

Preferred serious SaaS stack:

- Frontend: Next.js
- Backend: Next.js API routes or FastAPI
- Database: Postgres
- Auth: Supabase Auth, Clerk, Auth0, or similar
- Storage: S3, Cloudflare R2, or Supabase Storage
- Background jobs: Inngest, Trigger.dev, Celery, or scheduled workers
- Email: Resend, Postmark, or SendGrid
- Hosting: Vercel, Render, Fly.io, Railway, AWS, or similar
- Billing: Stripe
- Export generation: Node or Python worker

Possible prototype stack:

- AppSheet
- Google Sheets or AppSheet Database
- Google Drive
- Apps Script

Guidance:

- AppSheet may be useful for workflow validation.
- A proper SaaS stack is preferred for a public multi-tenant product.
- Do not over-engineer the infrastructure before the core evidence workflow is validated.

---

## Suggested Repository Structure
```
evidencesnap/
├── README.md
├── AI_CONTEXT.md
├── docs/
│   ├── product/
│   │   ├── vision.md
│   │   ├── roadmap.md
│   │   ├── pricing.md
│   │   └── target-customers.md
│   ├── architecture/
│   │   ├── overview.md
│   │   ├── data-model.md
│   │   ├── auth-and-rbac.md
│   │   ├── storage.md
│   │   └── audit-logging.md
│   ├── evidence-as-code/
│   │   ├── schema.md
│   │   ├── examples.md
│   │   └── oscal-mapping-notes.md
│   ├── security/
│   │   ├── security-principles.md
│   │   ├── tenant-isolation.md
│   │   └── threat-model.md
│   └── decisions/
│       └── ADR-0001-project-direction.md
├── templates/
│   ├── generic-monthly-evidence/
│   ├── cmmc-level-1/
│   ├── cmmc-level-2/
│   ├── nist-800-171/
│   └── soc2-security/
├── app/
├── api/
├── db/
│   ├── migrations/
│   └── seed/
├── scripts/
├── tests/
└── .github/
    └── workflows/
```
---

## Roadmap
### Phase 0 — Validate

Goals:

- Landing page
- Demo dashboard
- Sample evidence package
- Pricing hypothesis
- Outreach list
- Discovery interviews

Success criteria:

- At least 10 useful discovery conversations
- At least 3 demo requests
- At least 1 pilot candidate
- Phase 1 — Internal Prototype

Goals:

- Basic CRUD for controls, systems, requirements, requests
- Upload evidence
- Review evidence
- Generate basic export

Success criteria:

- Run a fake monthly evidence cycle end-to-end

### Phase 2 — Private Beta

Goals:

- Auth
- Tenant isolation
- Email reminders
- Audit logs
- Export package
- Basic dashboard

Success criteria:

- One real customer or pilot uses it for a real evidence cycle

### Phase 3 — Paid MVP

Goals:

- Billing
- CSV import/export
- Template packs
- Better onboarding
- Support workflow
- Basic admin settings

Success criteria:

- 3 paying customers


### Phase 4 — Evidence-as-Code

Goals:

- YAML/JSON import/export
- Versioned requirements
- Validation rules
- Template repository
- Compliance repo concept

Success criteria:

- A customer can version and import evidence templates


### Phase 5 — Automation and AI Assistance

Goals:

- Evidence summaries
- Relevance checks
- Missing evidence explanations
- Integrations
- OSCAL bridge research

Success criteria:

- AI improves review speed without making unsupported compliance claims

---

## AI Feature Guidance

AI can be useful, but it must not overpromise.

Good AI uses:

- Summarize uploaded evidence
- Identify likely evidence type
- Check whether evidence appears relevant to requirement
- Flag stale evidence
- Explain missing evidence
- Generate monthly executive summary
- Suggest possible control mappings
- Draft reviewer notes

Avoid AI claims like:

- Certified compliant
- Guaranteed audit success
- Automatically passes CMMC
- Replaces assessor judgment
- Fully automated compliance

Preferred wording:

```AI-assisted evidence organization and review.```

---

## Documentation Style

Documentation should be clear, practical, and implementation-oriented.

Preferred style:

- Markdown-first
- Clear headers
- Examples
- Tables where useful
- Code blocks for schemas and configs
- Decision records for major architectural choices
- Keep docs append-friendly
- Prefer explicit assumptions
- Use TODO sections for unresolved decisions

For larger docs, collapsible <details></details> sections are acceptable and very preferred.

## Naming Conventions

Use consistent naming where possible.

Preferred product terms:

- Organization
- System
- Framework
- Control
- Evidence Requirement
- Evidence Period
- Evidence Request
- Evidence Item
- Review Comment
- Export Package
- Audit Log

Avoid introducing synonyms unless needed.

For example, do not randomly alternate between:

- Evidence Task
- Evidence Request
- Evidence Assignment

Use **Evidence Request**.

---

## Open Questions

These decisions are not final yet.

### Product
- Which niche should be targeted first: CMMC contractors, MSPs, or SOC 2 SaaS companies?
- Should the first prototype be AppSheet or SaaS-native?
- Should evidence packages generate PDF, ZIP, or both in v1?
- Should framework templates be bundled or imported from GitHub?
### Technical
- Next.js-only backend or FastAPI backend?
- Supabase vs Neon/Postgres + custom auth?
- S3 vs Cloudflare R2 vs Supabase Storage?
- Which background job system?
- How strict should file validation be in v1?
- How should audit logs be made tamper-resistant?
### Compliance-as-Code
- What should the first YAML schema look like?
- Should schemas be stored per workspace, per repo, or both?
- When should OSCAL mapping begin?
- Should template packs live in this repo or a separate public template repo?
- Current Recommended Next Steps
- Create repository with README.md and AI_CONTEXT.md.
- Add ```/docs/product/vision.md```.
- Add ```/docs/architecture/data-model.md```.
- Add ```/docs/evidence-as-code/schema.md```.
- Add ```/docs/decisions/ADR-0001-project-direction.md```.
- Build a sample evidence package manually.
- Create a landing page draft.
- Decide prototype stack.
- Build the smallest possible evidence workflow.
- Start discovery outreach before overbuilding.

---

## Persistent Project Guidance for AI Assistants

When assisting with EvidenceSnap:

- Keep the MVP narrow.
- Prioritize evidence workflow over broad GRC features.
- Preserve the evidence-as-code direction.
- Do not overpromise compliance outcomes.
- Keep security and tenant isolation in mind.
- Prefer practical phased implementation.
- Use clear documentation and examples.
- Track decisions in ADR-style documents.
- When generating schemas, consider future OSCAL mapping but do not force full OSCAL early.
- When proposing features, categorize them as MVP, post-MVP, or future.
- When in doubt, optimize for the core loop: define evidence, request evidence, collect evidence, review evidence, export evidence.