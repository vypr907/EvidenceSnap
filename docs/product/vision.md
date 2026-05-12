# EvidenceSnap Product Vision

> Lightweight compliance evidence collection without spreadsheet chaos.

EvidenceSnap is a SaaS-native compliance evidence tracking platform designed for small regulated teams that need a practical way to request, collect, review, organize, and export audit-ready evidence without adopting a full enterprise GRC platform.

The product starts with a narrow, high-value workflow:

> Define what evidence is needed, assign owners, collect proof, review submissions, track what is missing, and export a clean evidence package.

---

# Table of Contents


<details open>
<summary><strong>📚 Table of Contents</strong></summary>

<a id="table-of-contents"></a>

- [1. Vision Statement](#vision-statement)
- [2. Problem](#problem)
- [3. Target Users](#target-users)
  - [Primary Early Customers](#primary-early-customers)
  - [Not Initial Target Customers](#not-initial-target-customers)
- [4. Product Positioning](#product-positioning)
  - [What EvidenceSnap Is](#what-evidencesnap-is)
  - [What EvidenceSnap Is Not](#what-evidencesnap-is-not)
- [5. Core Product Promise](#core-product-promise)
- [6. MVP Product Loop](#mvp-product-loop)
- [7. MVP Scope](#mvp-scope)
  - [MVP Must-Haves](#mvp-must-haves)
  - [MVP Non-Goals](#mvp-non-goals)
- [8. SaaS-Native Prototype Decision](#saas-native-prototype-decision)
  - [Rationale](#saas-native-rationale)
  - [Consequence](#saas-native-consequence)
- [9. Product Layers](#product-layers)
  - [Layer 1 — Human Evidence Workflow](#layer-1-human-evidence-workflow)
  - [Layer 2 — Evidence-as-Code](#layer-2-evidence-as-code)
  - [Layer 3 — Automation and Continuous Compliance](#layer-3-automation-and-continuous-compliance)
- [10. Core Concepts](#core-concepts)
  - [Organization](#organization)
  - [User](#user)
  - [System](#system)
  - [Framework](#framework)
  - [Control](#control)
  - [Evidence Requirement](#evidence-requirement)
  - [Evidence Period](#evidence-period)
  - [Evidence Request](#evidence-request)
  - [Evidence Item](#evidence-item)
  - [Export Package](#export-package)
- [11. Primary User Roles](#primary-user-roles)
  - [Admin](#role-admin)
  - [Compliance Manager](#role-compliance-manager)
  - [Evidence Owner](#role-evidence-owner)
  - [Reviewer](#role-reviewer)
  - [Auditor / Read-only](#role-auditor-read-only)
- [12. Key Differentiators](#key-differentiators)
  - [Evidence-First Design](#evidence-first-design)
  - [Exportable Evidence Packages](#exportable-evidence-packages)
  - [Service-Assisted Setup](#service-assisted-setup)
  - [Evidence-as-Code Foundation](#evidence-as-code-foundation)
  - [Security and Auditability](#security-and-auditability)
- [13. Security Principles](#security-principles)
- [14. Initial Business Model](#initial-business-model)
  - [Example Pricing](#example-pricing)
  - [Example Setup Fees](#example-setup-fees)
- [15. Early Success Criteria](#early-success-criteria)
  - [Prototype Success Criteria](#prototype-success-criteria)
  - [Business Success Criteria](#business-success-criteria)
- [16. Design Principles](#design-principles)
  - [Keep the MVP Narrow](#keep-the-mvp-narrow)
  - [Prefer Practical Workflows Over Compliance Theater](#prefer-practical-workflows-over-compliance-theater)
  - [Make Evidence Portable](#make-evidence-portable)
  - [Preserve History](#preserve-history)
  - [Build for Trust](#build-for-trust)
  - [Avoid Overpromising](#avoid-overpromising)
  - [Make the Technical Foundation Visible](#make-the-technical-foundation-visible)
- [17. Future Vision](#future-vision)
- [18. Current Priority](#current-priority)
- [19. Guiding Rule](#guiding-rule)

</details>

---
<a id="vision-statement"></a>

## 1. Vision Statement


EvidenceSnap helps small regulated teams turn compliance evidence collection from an ad hoc scramble into a structured, repeatable, auditable workflow.

The long-term vision is to become a lightweight evidence operations platform that supports:

- Human-driven evidence collection
- Recurring evidence workflows
- Evidence package exports
- Evidence-as-code templates
- Versioned compliance configuration
- AI-assisted evidence review
- Future compliance-as-code and OSCAL-aligned workflows

EvidenceSnap should not begin as a broad GRC platform. It should begin as a focused solution for compliance evidence operations.

[Back to top](#table-of-contents)

---

<a id="problem"></a>

## 2. Problem

Many small teams have compliance obligations but lack clean evidence operations.

Evidence is often scattered across:

- Email threads
- Shared drives
- Screenshots
- CSV exports
- Spreadsheets
- Ticketing systems
- Chat messages
- One-off folders
- Manually assembled audit packages

This creates several problems:

| Problem | Impact |
|---|---|
| Evidence is hard to find | Audit prep takes too long |
| Owners are unclear | Tasks get missed |
| Due dates are informal | Evidence becomes stale |
| Controls are not mapped | Proof is disconnected from requirements |
| Review is inconsistent | Bad evidence may be accepted |
| Exports are manual | Packages are slow and error-prone |
| History is weak | Teams cannot easily prove what changed and when |

The core pain is not that teams lack another dashboard.

The core pain is that they lack a repeatable evidence workflow.

[Back to top](#table-of-contents)

---

<a id="target-users"></a>

## 3. Target Users

EvidenceSnap is intended for small and mid-sized teams that need compliance evidence discipline but are not ready for heavy enterprise GRC tooling.

<a id="primary-early-customers"></a>

### Primary Early Customers

1. **Small government contractors**
   - Especially teams preparing for CMMC, NIST 800-171, or similar evidence expectations.

2. **MSPs / MSSPs**
   - Teams that collect compliance evidence from clients and need repeatable client-facing workflows.

3. **Small SaaS companies**
   - Especially teams preparing for SOC 2 or internal security reviews.

4. **Small regulated businesses**
   - Teams with recurring evidence needs but limited compliance staff.

5. **Internal security/compliance teams**
   - Teams that need better evidence tracking without a large tool rollout.

<a id="not-initial-target-customers"></a>

### Not Initial Target Customers

EvidenceSnap is not initially aimed at large enterprises that already have mature GRC platforms such as ServiceNow GRC, Archer, Drata, Vanta, Hyperproof, OneTrust, or similar systems.

[Back to top](#table-of-contents)

---

<a id="product-positioning"></a>

## 4. Product Positioning

Preferred positioning:

> Lightweight compliance evidence collection without spreadsheet chaos.

Alternative positioning:

> Evidence tracking for small regulated teams.

Longer positioning:

> EvidenceSnap helps small regulated teams request, collect, review, and export compliance evidence by control, system, owner, and reporting period.

<a id="what-evidencesnap-is"></a>

### What EvidenceSnap Is

EvidenceSnap is:

- An evidence tracking platform
- A recurring evidence request system
- A review and approval workflow
- An evidence package generator
- A future evidence-as-code platform
- A practical tool for compliance operations

<a id="what-evidencesnap-is-not"></a>

### What EvidenceSnap Is Not

EvidenceSnap is not:

- A full enterprise GRC suite
- A guarantee of compliance
- A certification tool
- A replacement for auditors or assessors
- A legal/compliance authority
- A one-click audit solution
- An AI compliance oracle

[Back to top](#table-of-contents)

---

<a id="core-product-promise"></a>

## 5. Core Product Promise

EvidenceSnap should help users answer these questions quickly:

1. What evidence is required?
2. Which control does it support?
3. Which system does it apply to?
4. Who owns it?
5. When is it due?
6. Has it been submitted?
7. Has it been reviewed?
8. What is missing?
9. What is overdue?
10. Can we export an organized evidence package?

If the product does this well, it creates immediate value.

[Back to top](#table-of-contents)

---

<a id="mvp-product-loop"></a>

## 6. MVP Product Loop

The MVP should focus on the smallest useful evidence workflow.

```text
Define Evidence Requirement
        ↓
Generate Evidence Request
        ↓
Notify Evidence Owner
        ↓
Submit Evidence
        ↓
Review Evidence
        ↓
Approve / Reject / Waive
        ↓
Track Readiness
        ↓
Export Evidence Package
```

Everything in the MVP should support this loop.

[Back to top](#table-of-contents)

---

<a id="mvp-scope"></a>

## 7. MVP Scope

<a id="mvp-must-haves"></a>

### MVP Must-Haves

The first SaaS-native prototype should include:

- Authentication
- Organization workspace
- Role-based access control
- Systems
- Frameworks
- Controls
- Evidence Requirements
- Evidence Periods
- Evidence Requests
- Evidence Items
- File upload or external evidence link submission
- Reviewer approve/reject workflow
- Reminder emails
- Basic dashboard
- Evidence package export
- Audit log

<a id="mvp-non-goals"></a>

### MVP Non-Goals

The first version should intentionally avoid:

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

The first product should be narrow, useful, and shippable.

[Back to top](#table-of-contents)

---

<a id="saas-native-prototype-decision"></a>

## 8. SaaS-Native Prototype Decision

The first prototype should be SaaS-native.

This is an intentional project decision.

<a id="saas-native-rationale"></a>

### Rationale

The founder wants to:

- Build on the actual systems that can support the long-term product
- Work through real architecture, security, tenancy, and deployment issues early
- Showcase coding, DevOps, and SaaS development skills
- Avoid validating the product only through AppSheet-specific implementation patterns
- Create a portfolio-quality project that demonstrates software engineering capability

<a id="saas-native-consequence"></a>

### Consequence

AppSheet may still be useful as a mental model for workflow design, but it is not the selected prototype platform for EvidenceSnap.

The prototype should use a real SaaS stack with:

- Application code
- Database migrations
- Authentication
- Tenant isolation
- Object storage
- Background jobs
- Email delivery
- Deployment automation
- Source-controlled documentation

[Back to top](#table-of-contents)

---

<a id="product-layers"></a>

## 9. Product Layers

EvidenceSnap should evolve through three layers.

<a id="layer-1-human-evidence-workflow"></a>

### Layer 1 — Human Evidence Workflow

This is the MVP.

#### Capabilities

- Define evidence requirements
- Assign evidence owners
- Generate evidence requests
- Track due dates
- Submit evidence
- Review evidence
- Approve, reject, waive, or mark not applicable
- Export evidence packages

This is the foundation customers can understand and pay for immediately.

<a id="layer-2-evidence-as-code"></a>

### Layer 2 — Evidence-as-Code

This is the first major differentiator after the MVP.

#### Capabilities

- YAML/JSON evidence templates
- Versioned evidence requirements
- Import/export of evidence configuration
- Template packs
- Validation rules
- Compliance repository concept
- GitHub-backed configuration

Evidence-as-code should allow structured definitions like:

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

Users should not be required to write YAML manually, but the product should eventually be able to export and import it.

<a id="layer-3-automation-and-continuous-compliance"></a>

### Layer 3 — Automation and Continuous Compliance

This comes later.

#### Capabilities

- API-based evidence collection
- Scanner imports
- Cloud configuration checks
- Identity provider exports
- Ticketing integrations
- AI evidence summaries
- Evidence relevance checks
- Drift detection
- OSCAL bridge research
- Compliance-as-code expansion

This layer should only be built after the core evidence workflow is validated.

[Back to top](#table-of-contents)

---

<a id="core-concepts"></a>

## 10. Core Concepts

<a id="organization"></a>

### Organization

A customer tenant or workspace.

<a id="user"></a>

### User

A person with access to an Organization.

Suggested roles:

- Admin
- Compliance Manager
- Evidence Owner
- Reviewer
- Auditor / Read-only

<a id="system"></a>

### System

An application, infrastructure environment, platform, business system, or compliance boundary.

Examples:

- Corporate IT
- AWS Production
- Microsoft 365 Tenant
- Google Workspace
- Internal SaaS Platform

<a id="framework"></a>

### Framework

A security or compliance framework.

Examples:

- CMMC Level 1
- CMMC Level 2
- NIST 800-171
- SOC 2 Security
- Custom Internal Controls

<a id="control"></a>

### Control

A specific requirement from a framework.

Examples:

- AC-2
- IA-2
- CM-8
- RA-5

<a id="evidence-requirement"></a>

### Evidence Requirement

A recurring or one-time definition of what evidence must be collected.

Example:

> Monthly MFA status export for all active users.

<a id="evidence-period"></a>

### Evidence Period

A reporting window.

Examples:

- May 2026
- Q2 2026
- FY2026
- Assessment Window 1

<a id="evidence-request"></a>

### Evidence Request

A generated instance of an Evidence Requirement for a specific Evidence Period.

Example:

> May 2026 MFA Status Export due June 5.

<a id="evidence-item"></a>

### Evidence Item

A submitted file, URL, attestation, screenshot, report, export, or document.

<a id="export-package"></a>

### Export Package

An organized package containing evidence, control mappings, review notes, missing evidence reports, and audit logs.

[Back to top](#table-of-contents)

---

<a id="primary-user-roles"></a>

## 11. Primary User Roles

<a id="role-admin"></a>

### Admin

Responsible for workspace configuration.

Can:

- Manage users
- Manage systems
- Configure frameworks
- Manage settings
- View audit logs
- Generate exports

<a id="role-compliance-manager"></a>

### Compliance Manager

Responsible for operating the evidence workflow.

Can:

- Define evidence requirements
- Generate evidence periods
- Assign owners
- Review evidence status
- Export packages

<a id="role-evidence-owner"></a>

### Evidence Owner

Responsible for submitting assigned evidence.

Can:

- View assigned evidence requests
- Upload evidence
- Add notes
- Respond to reviewer feedback

<a id="role-reviewer"></a>

### Reviewer

Responsible for reviewing evidence quality.

Can:

- Approve evidence
- Reject evidence
- Request changes
- Add review comments

<a id="role-auditor-read-only"></a>

### Auditor / Read-only

Responsible for viewing evidence without modifying records.

Can:

- View approved evidence
- View export packages
- View control mappings
- View audit history, if permitted

[Back to top](#table-of-contents)

---

<a id="key-differentiators"></a>

## 12. Key Differentiators

<a id="evidence-first-design"></a>

### 1. Evidence-First Design

EvidenceSnap starts with the practical evidence workflow, not broad GRC theory.

The primary unit of value is:

> What evidence do we need, and is it ready?

<a id="exportable-evidence-packages"></a>

### 2. Exportable Evidence Packages

Export packages should be one of the main “wow” features.

A package may include:

```text
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

<a id="service-assisted-setup"></a>

### 3. Service-Assisted Setup

Early customers may need help mapping their first evidence requirements.

EvidenceSnap can be sold as a service-assisted micro-SaaS:

- Setup fee
- Monthly subscription
- Guided evidence mapping
- Template import
- Workflow configuration

<a id="evidence-as-code-foundation"></a>

### 4. Evidence-as-Code Foundation

EvidenceSnap should eventually make evidence requirements portable, versionable, and structured.

This creates value for technical users and differentiates the product from basic spreadsheet trackers.

<a id="security-and-auditability"></a>

### 5. Security and Auditability

Because the product stores compliance evidence, it must be built with security and auditability from the beginning.

[Back to top](#table-of-contents)

---

<a id="security-principles"></a>

## 13. Security Principles

Security is part of the product value.

Core principles:

- Tenant isolation by Organization ID
- Role-based access control
- Private object storage
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

Important architectural rule:

> Every tenant-scoped query must include Organization ID filtering.

Do not rely only on frontend filtering for tenant separation.

[Back to top](#table-of-contents)

---

<a id="initial-business-model"></a>

## 14. Initial Business Model

The preferred initial business model is service-assisted SaaS.

<a id="example-pricing"></a>

### Example Pricing

| Plan | Price | Intended Use |
|---|---:|---|
| Starter | $99/month | One system, basic evidence tracking |
| Team | $199/month | Multiple systems, reviewers, export packages |
| Compliance Ops | $399/month | Advanced workflows, priority support |

<a id="example-setup-fees"></a>

### Example Setup Fees

| Setup Package | Price |
|---|---:|
| Starter setup | $299 |
| Guided evidence mapping | $750 |
| Custom framework import | $1,500 |

Initial business goal:

> Reach $800-$1,000/month recurring revenue with 3-5 early customers.

[Back to top](#table-of-contents)

---

<a id="early-success-criteria"></a>

## 15. Early Success Criteria

The first product milestone is not “build a complete GRC platform.”

The first success milestone is:

> A real team uses EvidenceSnap to complete one evidence period from request generation through export package.

<a id="prototype-success-criteria"></a>

### Prototype Success Criteria

- A demo organization can be created.
- Controls can be added or imported.
- Evidence requirements can be defined.
- Evidence requests can be generated for a period.
- Evidence owners can submit files or links.
- Reviewers can approve or reject evidence.
- Missing and overdue evidence can be seen.
- An evidence package can be exported.
- Key actions are audit logged.

<a id="business-success-criteria"></a>

### Business Success Criteria

- At least 10 discovery conversations
- At least 3 demo requests
- At least 1 pilot candidate
- At least 1 team willing to use the product for a real evidence cycle
- Eventually 3-5 paying customers

[Back to top](#table-of-contents)

---

<a id="design-principles"></a>

## 16. Design Principles

<a id="keep-the-mvp-narrow"></a>

### Keep the MVP Narrow

If a feature does not directly support the evidence workflow, defer it.

<a id="prefer-practical-workflows-over-compliance-theater"></a>

### Prefer Practical Workflows Over Compliance Theater

The product should make real evidence collection easier.

<a id="make-evidence-portable"></a>

### Make Evidence Portable

Users should be able to export their data and evidence.

<a id="preserve-history"></a>

### Preserve History

Evidence, reviews, waivers, and exports should be historically traceable.

<a id="build-for-trust"></a>

### Build for Trust

Security, audit logging, and clear limitations are mandatory.

<a id="avoid-overpromising"></a>

### Avoid Overpromising

EvidenceSnap helps organize and track compliance evidence. It does not guarantee compliance outcomes.

<a id="make-the-technical-foundation-visible"></a>

### Make the Technical Foundation Visible

Because this project is also intended to demonstrate coding and DevOps skill, the repository should include strong documentation, architecture notes, deployment notes, tests, and decision records.

[Back to top](#table-of-contents)

---

<a id="future-vision"></a>

## 17. Future Vision

In the long term, EvidenceSnap could become a lightweight compliance operations platform with:

- Evidence-as-code repositories
- GitHub-backed compliance templates
- Versioned control mappings
- OSCAL-aligned exports
- Automated evidence collectors
- Cloud/security tool integrations
- AI evidence summaries
- AI relevance checks
- Drift detection
- MSP client portals
- Read-only auditor access
- Compliance package generation

The product should grow into these capabilities only after the core evidence workflow proves valuable.

[Back to top](#table-of-contents)

---

<a id="current-priority"></a>

## 18. Current Priority

The current priority is to build the SaaS-native foundation for the smallest useful evidence workflow.

Immediate next steps:

1. Finalize initial repository documentation.
2. Create architecture/data model documentation.
3. Choose the initial SaaS stack.
4. Define the first database schema.
5. Build authentication and tenant isolation.
6. Build core CRUD for systems, frameworks, controls, and evidence requirements.
7. Build evidence request generation.
8. Build upload/review/export workflow.
9. Create a sample demo workspace.
10. Start discovery outreach before overbuilding.

[Back to top](#table-of-contents)

---

<a id="guiding-rule"></a>

## 19. Guiding Rule

When deciding what to build next, ask:

> Does this help define, request, collect, review, track, or export compliance evidence?

If yes, it may belong in the MVP.

If no, it probably belongs in a later phase.

[Back to top](#table-of-contents)