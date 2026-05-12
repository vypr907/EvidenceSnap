# EvidenceSnap

> Lightweight compliance evidence tracking with evidence-as-code foundations.

EvidenceSnap is a compliance evidence operations platform designed for small regulated teams that need a practical way to request, collect, review, organize, and export audit-ready evidence without buying or maintaining a full enterprise GRC platform.

The goal is simple:

> Know what evidence is needed, who owns it, when it is due, what control it supports, what is missing, and how to package it when someone asks.

---

## 🚧 Project Status

EvidenceSnap is currently in early design / MVP planning.

This repository will contain:

- Application source code
- Architecture notes
- Product requirements
- Data model documentation
- Compliance-as-code / evidence-as-code schemas
- Design decisions
- Roadmap documents
- Security notes
- Deployment notes
- AI collaboration context

---

## 🎯 Product Vision

EvidenceSnap helps teams move from scattered compliance evidence to structured, repeatable evidence operations.

Instead of relying on email threads, spreadsheets, screenshots, and ad hoc folders, EvidenceSnap provides a system for:

- Defining compliance controls
- Mapping evidence requirements to controls
- Assigning evidence owners and reviewers
- Generating recurring evidence requests
- Uploading or linking evidence
- Reviewing and approving submissions
- Tracking missing or overdue evidence
- Exporting evidence packages by framework, control, system, and reporting period

Long term, EvidenceSnap should support **evidence-as-code** and eventually broader **compliance-as-code** workflows.

---

## 🧩 Core Concepts

### Organization

A customer workspace or tenant.

Examples:

- Small government contractor
- MSP compliance client
- SaaS startup
- Internal security team

### System

An application, infrastructure environment, business system, or compliance boundary.

Examples:

- Corporate IT
- AWS Production
- Microsoft 365 Tenant
- Google Workspace
- Internal SaaS Platform

### Framework

A compliance or security framework.

Examples:

- CMMC Level 1
- CMMC Level 2
- NIST 800-171
- SOC 2 Security
- Custom Internal Controls

### Control

A specific requirement or control from a framework.

Examples:

- AC-2
- IA-2
- CM-8
- RA-5

### Evidence Requirement

A recurring or one-time requirement that describes what proof must be collected for a control.

Example:

> Monthly MFA status export for all active users.

### Evidence Request

A generated instance of an Evidence Requirement for a specific reporting period.

Example:

> May 2026 MFA Status Export due June 5.

### Evidence Item

A submitted file, URL, attestation, screenshot, export, report, or document.

### Evidence Period

A reporting period such as a month, quarter, year, audit window, assessment period, or custom range.

### Export Package

An organized package containing approved evidence, control mappings, missing evidence reports, review notes, and audit logs.

---

## 🏗️ Initial MVP Scope

The MVP should focus on evidence tracking first.

### Must Have

- User authentication
- Organization workspace
- Role-based access
- Systems
- Frameworks
- Controls
- Evidence Requirements
- Evidence Periods
- Evidence Requests
- Evidence upload/linking
- Reviewer approve/reject workflow
- Reminder emails
- Dashboard
- Exportable evidence package
- Audit log

### Not MVP

The first version should avoid trying to become a full GRC platform.

Deferred features:

- Full risk register
- Full policy management
- Vendor risk management
- Complex workflow builder
- Deep scanner integrations
- Full OSCAL package generation
- Automated compliance scoring
- Public API
- Mobile app

---

## 🧠 Evidence-as-Code Direction

EvidenceSnap should eventually allow controls and evidence requirements to be represented as structured, versionable YAML or JSON.

Example:

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
  The product should begin with a simple internal model that can later map to standards such as OSCAL.
  ---

## 🗂️ Suggested Repository Structure
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

This structure can change as the tech stack becomes more concrete.
---

## 👥 Target Users

EvidenceSnap is intended for teams that need compliance evidence discipline but may not be ready for a large GRC platform.

Initial target users:

- Small government contractors
- CMMC-prep organizations
- MSPs / MSSPs supporting compliance clients
- Small SaaS companies pursuing SOC 2
- Small regulated businesses
- Internal security/compliance teams with lightweight needs
---

## 💰 Initial Business Model

The first commercial version should likely be a service-assisted micro-SaaS.

### Possible pricing:

|Plan	        |Price	       |Use Case|
| ------------- | ------------ | --------------------
|Starter	    | $99/month	   |1 system, basic evidence tracking
|Team    	    | $199/month   |Multiple systems, reviewers, exports
|Compliance Ops	| $399/month   |Advanced workflows, priority support

### Possible setup fees:

|Setup Package	|Price|
| ------------- | ----|
|Starter setup	|$299
|Guided evidence mapping	|$750
|Custom framework import	|$1,500

### Initial goal:

Reach $800-$1,000 MRR with 3-5 early customers.

## 🔐 Security Principles

Because EvidenceSnap stores compliance evidence, security must be designed into the product from the beginning.

### Core principles:

- Tenant isolation by Organization ID
- Role-based access control
- Private file storage
- Signed download URLs
- Encryption in transit
- Encryption at rest
- Immutable-style audit logging
- File hash tracking
- Least privilege
- Soft deletes where appropriate
- Export activity logging
- Regular backups
- Clear data retention controls


## 📦 Export Package Concept

A generated evidence package may look like this:

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

The export package is expected to be one of the main product differentiators.

# 🧭 Roadmap
## Phase 0 — Validation
- Landing page
- Demo screenshots
- Sample evidence package
- Outreach to early users
- Discovery interviews
## Phase 1 — Internal Prototype
- Basic app shell
- Evidence tracking data model
- File upload
- Status workflow
- Manual export
## Phase 2 — Private Beta
- Authentication
- Tenant isolation
- Reminder emails
- Reviewer workflow
- Audit log
- Export package generation
## Phase 3 — Paid MVP
- Subscription billing
- CSV import/export
- Template packs
- Better dashboards
- Setup workflow
- Customer onboarding
## Phase 4 — Evidence-as-Code
- YAML/JSON import/export
- Versioned evidence templates
- Validation rules
- GitHub-backed template packs
- Compliance repository concept
## Phase 5 — Continuous Compliance
- Integrations
- Automated evidence collection
- AI evidence summaries
- Evidence relevance checks
- OSCAL import/export research
- Drift detection

---

## ⚠️ Compliance Disclaimer

EvidenceSnap is intended to help organize, track, and package compliance evidence.

It does not guarantee certification, authorization, audit success, or legal compliance.

Users remain responsible for determining applicable requirements, validating evidence, and working with qualified assessors, auditors, legal counsel, or compliance professionals as needed.
---

## 📌 Current Priority

The immediate priority is to define and build the smallest useful evidence workflow:

1. Define controls.
2. Define evidence requirements.
3. Generate evidence requests.
4. Collect evidence.
5. Review evidence.
6. Export an evidence package.

Everything else should support that core loop.