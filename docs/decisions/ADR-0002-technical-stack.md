# ADR-0002: Free-First Technical Stack and DevSecOps Direction

> Decision record for EvidenceSnap's initial SaaS-native technical stack, development workflow, CI/CD approach, and DevSecOps posture.

| Field | Value |
|---|---|
| ADR | ADR-0002 |
| Status | Accepted |
| Date | 2026-05-12 |
| Project | EvidenceSnap |
| Decision Type | Architecture / Technical Stack / DevSecOps |
| Related ADR | [ADR-0001: Project Direction and SaaS-Native Prototype](./ADR-0001-project-direction.md) |

---

<a id="table-of-contents"></a>

<details open>
<summary><strong>📚 Table of Contents</strong></summary>

- [1. Context](#context)
- [2. Decision](#decision)
- [3. Selected Stack](#selected-stack)
- [4. Constraints](#constraints)
  - [Zero-Funds Constraint](#zero-funds-constraint)
  - [Work Machine Constraint](#work-machine-constraint)
  - [Home Development Constraint](#home-development-constraint)
  - [Documentation and Distribution Constraint](#documentation-and-distribution-constraint)
- [5. Rationale](#rationale)
- [6. Alternatives Considered](#alternatives-considered)
  - [Option A: Next.js + Supabase + Vercel + GitHub Codespaces](#option-a-nextjs--supabase--vercel--github-codespaces)
  - [Option B: Next.js + Clerk + Neon + Cloudflare R2](#option-b-nextjs--clerk--neon--cloudflare-r2)
  - [Option C: FastAPI + React + Postgres](#option-c-fastapi--react--postgres)
  - [Option D: AppSheet / No-Code Prototype](#option-d-appsheet--no-code-prototype)
- [7. CI/CD and DevSecOps Considerations](#cicd-and-devsecops-considerations)
- [8. Source Control Strategy](#source-control-strategy)
- [9. Environment Strategy](#environment-strategy)
- [10. Secrets Management](#secrets-management)
- [11. Database and Migration Strategy](#database-and-migration-strategy)
- [12. Authentication and Authorization Strategy](#authentication-and-authorization-strategy)
- [13. Tenant Isolation Strategy](#tenant-isolation-strategy)
- [14. Storage Strategy](#storage-strategy)
- [15. Email and Notification Strategy](#email-and-notification-strategy)
- [16. Background Job Strategy](#background-job-strategy)
- [17. Testing Strategy](#testing-strategy)
- [18. Security Scanning and Supply Chain Strategy](#security-scanning-and-supply-chain-strategy)
- [19. Observability Strategy](#observability-strategy)
- [20. Backup and Retention Strategy](#backup-and-retention-strategy)
- [21. Documentation Strategy](#documentation-strategy)
- [22. Deferred Decisions](#deferred-decisions)
- [23. Consequences](#consequences)
  - [Positive Consequences](#positive-consequences)
  - [Negative Consequences](#negative-consequences)
- [24. Implementation Plan](#implementation-plan)
- [25. Decision Review Triggers](#decision-review-triggers)
- [26. Related Documents](#related-documents)

</details>

---

<a id="context"></a>

## 1. Context

EvidenceSnap is intended to be a SaaS-native compliance evidence tracking platform.

ADR-0001 established that the first prototype should be SaaS-native instead of AppSheet-based so the project can validate the actual long-term systems, architecture, security posture, and DevOps workflow.

The next major decision is the initial technical stack.

This stack must support:

- Zero-cost startup
- SaaS-native development
- Multi-tenant architecture
- Authentication
- Role-based access control
- Private evidence storage
- Audit logging
- Evidence package exports
- Browser-based development when local installs are restricted
- Local development on a MacBook Pro
- Strong documentation for future users/contributors
- CI/CD and DevSecOps practices from the beginning

The stack should be realistic for a solo builder but credible enough to demonstrate professional engineering and DevSecOps thinking.

[Back to top](#table-of-contents)

---

<a id="decision"></a>

## 2. Decision

EvidenceSnap will use a **free-first SaaS-native stack**:

```text
Next.js + TypeScript + Supabase + Vercel + GitHub Codespaces + GitHub Actions
```

The initial prototype will use:

- **GitHub** for source control and documentation
- **GitHub Codespaces** for browser-based development
- **Next.js** for the frontend and backend application layer
- **TypeScript** for application code
- **Supabase Auth** for authentication
- **Supabase Postgres** for the database
- **Supabase Storage** for evidence uploads
- **Supabase Row Level Security** as part of tenant isolation
- **Vercel Hobby** for free deployment
- **GitHub Actions** for CI checks
- **Dependabot and CodeQL** for basic DevSecOps automation
- **Resend** later for production email notifications
- **Stripe** later for billing

The initial stack should avoid paid services until the project has a real pilot or paying customer.

[Back to top](#table-of-contents)

---

<a id="selected-stack"></a>

## 3. Selected Stack

| Layer | Selected Tool | Notes |
|---|---|---|
| Source Control | GitHub | Repo, docs, issues, pull requests, Actions |
| Cloud Dev Environment | GitHub Codespaces | Browser-based development when local installs are restricted |
| Local Dev Environment | MacBook Pro | Home development environment |
| Frontend | Next.js | React-based SaaS app framework |
| Backend | Next.js API routes / server actions | Avoid separate backend until needed |
| Language | TypeScript | Safer application code and better portfolio signal |
| Database | Supabase Postgres | Free-tier managed Postgres |
| Auth | Supabase Auth | Integrated with Supabase and RLS |
| Storage | Supabase Storage | Free-tier object storage for prototype evidence files |
| Deployment | Vercel Hobby | Free deployment and preview deployments |
| CI | GitHub Actions | Typecheck, lint, tests, build |
| Security Scanning | Dependabot, CodeQL, npm audit | Free basic supply chain/security checks |
| Email | Deferred; Resend later | Mock/log emails until needed |
| Background Jobs | Deferred; manual/admin-triggered first | Scheduled jobs later |
| Billing | Deferred; Stripe later | Not needed before paid beta |

[Back to top](#table-of-contents)

---

<a id="constraints"></a>

## 4. Constraints

<a id="zero-funds-constraint"></a>

### Zero-Funds Constraint

The project currently has no startup budget.

The initial stack must use free tiers or local tools wherever possible.

This means:

- No paid hosting at the start
- No paid database at the start
- No paid email service at the start
- No paid background job service at the start
- No paid AI services in the MVP
- No paid monitoring service in the MVP
- No paid design system or UI kit

The product should be designed so these services can be upgraded later without a full rewrite.

<a id="work-machine-constraint"></a>

### Work Machine Constraint

The founder is limited in what can be installed on the local machine while at work.

The development workflow should support browser-based development through GitHub Codespaces.

This allows work on:

- Documentation
- Code
- Pull requests
- CI fixes
- App scaffolding
- Tests
- Configuration

without requiring local installation of Node.js, Docker, Postgres, or other developer tools.

<a id="home-development-constraint"></a>

### Home Development Constraint

The founder has a MacBook Pro for home development.

The repo should include Mac-friendly local setup documentation.

Local development should support:

- Node.js LTS
- npm
- Git
- VS Code or equivalent editor
- Hosted Supabase project for early development
- Optional Supabase CLI later

Docker should not be required for the earliest prototype.

<a id="documentation-and-distribution-constraint"></a>

### Documentation and Distribution Constraint

The founder is used to building tools primarily for personal use.

EvidenceSnap must be documented as though another person may eventually:

- clone the repo
- understand the architecture
- run the app locally
- configure environment variables
- contribute code
- deploy a prototype
- understand security assumptions
- understand what is and is not production-ready

Documentation is therefore part of the product, not an afterthought.

[Back to top](#table-of-contents)

---

<a id="rationale"></a>

## 5. Rationale

The selected stack balances free-first development with real SaaS architecture.

### Why Next.js

Next.js supports:

- React frontend
- API routes/server actions
- Vercel deployment
- TypeScript
- modern SaaS app structure
- full-stack development in one repo

This avoids needing a separate backend service in the MVP.

### Why TypeScript

TypeScript supports:

- safer code
- better refactoring
- improved developer experience
- clearer data model typing
- stronger portfolio signal

### Why Supabase

Supabase provides several core SaaS components in one free-first platform:

- Postgres database
- Authentication
- Storage
- Row Level Security
- API access
- SQL migrations
- dashboard for early development

This reduces the number of moving parts.

### Why Vercel

Vercel integrates well with Next.js and GitHub.

It provides:

- free hobby deployment
- preview deployments for pull requests
- automatic production deploys from `main`
- simple environment variable management

### Why GitHub Codespaces

Codespaces supports browser-based development, which helps when the work machine cannot install local development tools.

It also helps standardize the development environment for future contributors.

### Why GitHub Actions

GitHub Actions supports free CI automation for public repositories and basic validation workflows.

It should be used for:

- type checking
- linting
- tests
- builds
- dependency checks
- CodeQL scanning

[Back to top](#table-of-contents)

---

<a id="alternatives-considered"></a>

## 6. Alternatives Considered

<a id="option-a-nextjs--supabase--vercel--github-codespaces"></a>

### Option A: Next.js + Supabase + Vercel + GitHub Codespaces

This is the selected option.

#### Benefits

- Free-first
- SaaS-native
- Good documentation and portfolio value
- Browser-based development through Codespaces
- MacBook-friendly local development
- Integrated auth/database/storage
- Real Postgres database
- Supports Row Level Security
- Simple Vercel deployment
- Avoids too many services early

#### Drawbacks

- Supabase coupling may increase early
- RLS design requires care
- Free-tier limits may appear later
- Background jobs are not fully solved immediately
- Email delivery is deferred

#### Decision

Accepted.

[Back to top](#table-of-contents)

---

<a id="option-b-nextjs--clerk--neon--cloudflare-r2"></a>

### Option B: Next.js + Clerk + Neon + Cloudflare R2

A more modular SaaS stack.

#### Benefits

- Strong separation of responsibilities
- Clerk is polished for auth
- Neon is strong for serverless Postgres
- Cloudflare R2 is strong object storage
- Good long-term architecture

#### Drawbacks

- More accounts and services
- More integration work
- More setup documentation
- More secrets to manage
- More complexity before validation
- May create friction under zero-funds constraints

#### Decision

Rejected for initial prototype.

This may be reconsidered if Supabase becomes limiting.

[Back to top](#table-of-contents)

---

<a id="option-c-fastapi--react--postgres"></a>

### Option C: FastAPI + React + Postgres

A more traditional split frontend/backend stack.

#### Benefits

- Strong Python backend fit
- Good API design flexibility
- Good option for future export/AI workers
- Familiar Python ecosystem

#### Drawbacks

- More infrastructure complexity
- Separate frontend/backend deployment
- More setup work
- More CI/CD complexity
- Slower MVP scaffolding
- Less convenient for Vercel-only deployment

#### Decision

Rejected for initial prototype.

FastAPI may be introduced later if the backend grows beyond what Next.js API routes/server actions should handle.

[Back to top](#table-of-contents)

---

<a id="option-d-appsheet--no-code-prototype"></a>

### Option D: AppSheet / No-Code Prototype

Use AppSheet to prototype the evidence workflow.

#### Benefits

- Fast workflow prototyping
- Familiar no-code environment
- Strong form/table automation capabilities
- Useful mental model for business workflows

#### Drawbacks

- Does not validate SaaS-native architecture
- Does not demonstrate coding/DevOps skills
- Does not match long-term tenant isolation strategy
- Does not match long-term auth/storage/deployment model
- Could encourage AppSheet-specific design assumptions

#### Decision

Rejected for EvidenceSnap prototype.

AppSheet remains useful as prior experience and workflow inspiration, but not as the implementation platform.

[Back to top](#table-of-contents)

---

<a id="cicd-and-devsecops-considerations"></a>

## 7. CI/CD and DevSecOps Considerations

The selected stack must support CI/CD and DevSecOps from the beginning.

EvidenceSnap is a compliance evidence product, so the repository and delivery pipeline should reflect security-conscious engineering practices.

The initial CI/CD and DevSecOps model should support:

- GitHub-based source control
- Pull request review workflow, even for solo development
- Vercel preview deployments
- Vercel production deployment from `main`
- GitHub Actions validation
- TypeScript type checking
- linting
- automated build validation
- basic unit tests
- dependency monitoring
- CodeQL scanning
- secret scanning
- documented secrets management
- version-controlled database migrations
- tenant isolation testing
- Supabase Row Level Security
- private evidence storage
- signed URLs for downloads
- application audit logging
- documentation-driven setup and deployment

The initial CI/CD model will use:

```text
GitHub Actions for validation
Vercel for deployment automation
Supabase for auth, database, storage, and RLS
```

[Back to top](#table-of-contents)

---

<a id="source-control-strategy"></a>

## 8. Source Control Strategy

GitHub is the source of truth for:

- application code
- documentation
- database migrations
- setup instructions
- architecture decisions
- evidence-as-code schemas
- template packs
- CI/CD workflows
- security documentation
- release notes

### Branching Strategy

For solo development, use a simple branching model:

```text
main
feature/*
docs/*
fix/*
security/*
```

### Branch Meaning

| Branch Pattern | Purpose |
|---|---|
| `main` | Stable branch, production deploy target |
| `feature/*` | Feature development |
| `docs/*` | Documentation updates |
| `fix/*` | Bug fixes |
| `security/*` | Security improvements |

### Pull Request Guidance

Even as a solo project, meaningful changes should use pull requests when practical.

This supports:

- review discipline
- CI validation
- changelog writing
- portfolio credibility
- future contributor workflows

[Back to top](#table-of-contents)

---

<a id="environment-strategy"></a>

## 9. Environment Strategy

The project should support three environment concepts:

```text
local / codespaces
preview
production
```

### Local / Codespaces

Used for development.

May connect to:

- hosted Supabase development project
- optional local Supabase later

### Preview

Used for pull request deployments through Vercel.

Initially, preview deployments may share the development Supabase project.

### Production

Used for the deployed app from `main`.

Before real customer evidence is stored, production should use a separate Supabase project.

### Free-First Environment Compromise

Because funds are limited, the earliest prototype may use a single free Supabase project.

This is acceptable only while:

- no real customer evidence is stored
- data is demo/test data
- the limitation is documented
- production separation is planned before pilot/customer use

### Required Future Change Before Customer Data

Before storing real customer evidence:

- create separate development and production Supabase projects
- separate environment variables
- review RLS policies
- document backup and retention expectations
- verify storage bucket privacy
- verify signed URL behavior

[Back to top](#table-of-contents)

---

<a id="secrets-management"></a>

## 10. Secrets Management

Secrets must be managed carefully from the beginning.

### Rules

- Never commit `.env`
- Commit `.env.example`
- Document all environment variables
- Store deployment secrets in Vercel environment variables
- Store CI-only secrets in GitHub Actions secrets
- Keep Supabase service role keys server-side only
- Never expose privileged keys to browser code
- Rotate secrets if accidentally exposed
- Use least privilege where possible

### Expected Environment Variables

Initial variables may include:

```bash
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=
NEXT_PUBLIC_APP_URL=
```

Future variables may include:

```bash
RESEND_API_KEY=
STRIPE_SECRET_KEY=
STRIPE_WEBHOOK_SECRET=
```

### Documentation Requirement

Create:

```text
.env.example
docs/setup/environment-variables.md
docs/security/secrets-management.md
```

[Back to top](#table-of-contents)

---

<a id="database-and-migration-strategy"></a>

## 11. Database and Migration Strategy

EvidenceSnap will use Supabase Postgres for the initial database.

Database schema changes should be version-controlled.

Preferred migration location:

```text
supabase/migrations/
```

### Migration Rules

- Schema changes should be committed to the repo.
- Migrations should be reviewed like application code.
- Tenant-scoped tables should include `org_id`.
- RLS policies should be defined in migrations when feasible.
- Seed/demo data should be separated from schema migrations.

### Initial Foundation Tables

The first tables should be:

```text
organizations
users / profiles
memberships
audit_logs
```

Then add:

```text
systems
frameworks
controls
evidence_requirements
evidence_periods
evidence_requests
evidence_items
review_comments
export_packages
```

### ORM Decision

No ORM is selected yet.

The initial implementation may use:

- Supabase client queries
- SQL migrations
- TypeScript types generated from Supabase later

Potential future options:

- Drizzle
- Prisma

The ORM decision is deferred until the initial schema and workflow stabilize.

[Back to top](#table-of-contents)

---

<a id="authentication-and-authorization-strategy"></a>

## 12. Authentication and Authorization Strategy

Supabase Auth will be used for initial authentication.

### Initial Auth Features

- Email/password login
- Magic link or passwordless login, if convenient
- Authenticated dashboard
- User profile record
- Organization membership mapping

### Authorization Model

Authorization will be based on Organization Membership.

Core roles:

```text
Admin
Compliance Manager
Evidence Owner
Reviewer
Auditor / Read-only
```

### Authorization Principles

- Authentication proves who the user is.
- Membership proves which organization the user belongs to.
- Role determines what the user can do.
- Tenant-scoped queries must filter by organization.
- RLS should enforce tenant separation at the database layer where practical.

[Back to top](#table-of-contents)

---

<a id="tenant-isolation-strategy"></a>

## 13. Tenant Isolation Strategy

Tenant isolation is a critical security requirement.

EvidenceSnap will use defense in depth:

1. **Application-layer filtering**
2. **Database-layer Row Level Security**
3. **Private storage paths**
4. **Signed URL generation after authorization checks**
5. **Audit logging for important tenant-scoped actions**

### Required Application Rule

Every tenant-scoped query must include `org_id`.

Example:

```sql
SELECT *
FROM evidence_requests
WHERE org_id = :current_org_id;
```

### RLS Direction

Supabase Row Level Security should be enabled on tenant-scoped tables.

Example policy concept:

```sql
CREATE POLICY "Members can read org evidence requests"
ON evidence_requests
FOR SELECT
USING (
  org_id IN (
    SELECT org_id
    FROM memberships
    WHERE user_id = auth.uid()
  )
);
```

### Tenant-Scoped Tables

Tenant-scoped tables include:

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

Potentially tenant-scoped or hybrid:

```text
frameworks
controls
```

[Back to top](#table-of-contents)

---

<a id="storage-strategy"></a>

## 14. Storage Strategy

Supabase Storage will be used for initial evidence file storage.

### Storage Requirements

Evidence files must be:

- private
- tenant-scoped
- linked to Evidence Items
- downloadable only after authorization
- accessed using signed URLs
- traceable with metadata
- hashable for evidence integrity

### Storage Path Pattern

```text
/orgs/{org_id}/evidence/{period_id}/{request_id}/{evidence_item_id}/{filename}
```

### Export Package Path Pattern

```text
/orgs/{org_id}/exports/{period_id}/{export_package_id}/package.zip
```

### File Metadata

Evidence Items should store:

- original file name
- file type
- file size
- storage path
- SHA-256 hash
- uploaded by
- uploaded at
- evidence date
- source system, if provided

### Future Storage Options

Supabase Storage may be replaced or supplemented later by:

- S3
- Cloudflare R2
- Azure Blob Storage

No storage migration is required for MVP.

[Back to top](#table-of-contents)

---

<a id="email-and-notification-strategy"></a>

## 15. Email and Notification Strategy

Email reminders are part of the product vision, but paid email service usage should be deferred until needed.

### MVP Phase

During early prototype:

- email reminders may be mocked
- reminder events may be logged
- admin/manual trigger may simulate reminders
- UI may show “would send reminder” messages

### Later Phase

When real beta users need reminders, add a free-tier transactional email provider.

Preferred future option:

```text
Resend
```

Alternative options:

```text
Postmark
SendGrid
Supabase Edge Function + email provider
```

### Email Events

Future email events may include:

- Evidence Request assigned
- Evidence Request due soon
- Evidence Request overdue
- Evidence rejected / needs changes
- Evidence approved
- Export package ready

[Back to top](#table-of-contents)

---

<a id="background-job-strategy"></a>

## 16. Background Job Strategy

Background jobs are useful but should not complicate the first prototype.

### MVP Phase

Use manual/admin-triggered actions first:

- generate Evidence Requests
- update overdue statuses
- generate export packages
- simulate reminders

### Later Phase

Add scheduled or event-driven jobs.

Potential options:

- Supabase scheduled functions
- GitHub Actions scheduled workflow
- Vercel Cron Jobs
- Inngest
- Trigger.dev

### Deferred Job Decisions

The final background job platform is deferred until the core evidence workflow is functional.

[Back to top](#table-of-contents)

---

<a id="testing-strategy"></a>

## 17. Testing Strategy

Testing should start small but focus on high-risk areas.

### Early Test Priorities

- tenant isolation helpers
- evidence request generation
- duplicate request prevention
- role/permission checks
- status transitions
- validation rule logic
- export manifest generation

### Critical Test Cases

```text
User from Org A cannot see Org B records.
Evidence Request generation does not create duplicates.
Only reviewers can approve evidence.
Rejected evidence requires a comment.
Waived evidence requires a reason.
Export package only includes selected org records.
Overdue status is calculated correctly.
```

### Test Tooling

Initial options:

- Vitest
- React Testing Library
- Playwright later

### CI Requirement

The CI pipeline should eventually run:

```bash
npm ci
npm run lint
npm run typecheck
npm test
npm run build
```

Testing should grow with the app, but tenant isolation tests should be prioritized early.

[Back to top](#table-of-contents)

---

<a id="security-scanning-and-supply-chain-strategy"></a>

## 18. Security Scanning and Supply Chain Strategy

EvidenceSnap should include basic DevSecOps automation early.

### Initial Tools

- GitHub Dependabot
- GitHub secret scanning
- GitHub CodeQL
- npm audit
- ESLint
- TypeScript strict mode

### Dependency Hygiene Rules

- Use a lockfile.
- Use `npm ci` in CI.
- Avoid unnecessary packages.
- Prefer maintained libraries.
- Review packages before adding them.
- Avoid copying unknown code into security-sensitive areas.
- Treat dependency updates as normal maintenance.

### npm Audit Guidance

`npm audit` may be noisy.

Initial approach:

- run dependency checks
- review findings
- avoid blocking every build on low-value noise
- enforce blocking only for high/critical issues later

### Future Supply Chain Enhancements

Future enhancements may include:

- SBOM generation
- container scanning, if containers are introduced
- SLSA/provenance work
- signed releases
- dependency review enforcement

[Back to top](#table-of-contents)

---

<a id="observability-strategy"></a>

## 19. Observability Strategy

Observability should be planned but not overbuilt.

### Initial Observability

Use:

- Vercel logs
- Supabase logs
- application audit logs
- structured server logs where practical
- error boundaries in the UI

### Future Observability

Potential future tools:

- Sentry
- OpenTelemetry
- uptime monitoring
- job failure alerts
- storage access logging
- audit export reporting

### MVP Guidance

Initial observability will rely on platform logs and application audit logs.

Dedicated error tracking may be added after the MVP workflow is functional.

[Back to top](#table-of-contents)

---

<a id="backup-and-retention-strategy"></a>

## 20. Backup and Retention Strategy

EvidenceSnap stores compliance evidence, so backup and retention expectations must be documented before real customer data is stored.

### Prototype Phase

During prototype:

- only demo/test data should be stored
- backup expectations are limited
- data loss is acceptable only for demo data
- this limitation must be documented

### Before Customer Data

Before storing real customer evidence:

- use a dedicated production Supabase project
- document database backup expectations
- document storage backup expectations
- document evidence retention expectations
- document deletion behavior
- document export package retention
- review Supabase backup capabilities
- define customer-facing limitations

### Retention Concepts

Future retention settings may include:

- evidence retention period
- export package retention period
- soft delete retention
- audit log retention
- organization offboarding export

[Back to top](#table-of-contents)

---

<a id="documentation-strategy"></a>

## 21. Documentation Strategy

Documentation is part of the product.

Because EvidenceSnap should be usable by others, the repo must include setup, architecture, and security documentation.

### Required Early Docs

```text
README.md
AI_CONTEXT.md
docs/product/vision.md
docs/architecture/data-model.md
docs/architecture/technical-stack.md
docs/evidence-as-code/schema.md
docs/decisions/ADR-0001-project-direction.md
docs/decisions/ADR-0002-technical-stack.md
docs/setup/codespaces.md
docs/setup/local-macos.md
docs/setup/environment-variables.md
docs/security/tenant-isolation.md
docs/security/secrets-management.md
docs/security/threat-model.md
```

### Documentation Style

Docs should use:

- Markdown
- clear headings
- collapsible tables of contents where useful
- manual anchors for stable linking
- examples
- tables
- explicit assumptions
- TODO sections for unresolved decisions
- append-friendly changelog/release notes

[Back to top](#table-of-contents)

---

<a id="deferred-decisions"></a>

## 22. Deferred Decisions

The following decisions are intentionally deferred:

| Decision | Deferred Until |
|---|---|
| ORM selection | After initial Supabase schema stabilizes |
| Dedicated backend service | When Next.js backend becomes insufficient |
| Production email provider | When beta users need real reminders |
| Background job platform | When recurring jobs are needed |
| Billing implementation | When a pilot is ready to pay |
| AI provider | After core evidence workflow works |
| External object storage | When Supabase Storage becomes limiting |
| Full staging environment | Before real customer data |
| Full OSCAL support | After evidence workflow validation |
| Enterprise SSO | Paying customer requirement |

[Back to top](#table-of-contents)

---

<a id="consequences"></a>

## 23. Consequences

<a id="positive-consequences"></a>

### Positive Consequences

This decision provides:

- zero-cost starting path
- SaaS-native architecture
- browser-based development
- MacBook local development
- integrated auth/database/storage
- clear deployment path
- DevSecOps-friendly repo structure
- portfolio-quality engineering story
- reduced service sprawl
- faster MVP compared to modular multi-service stacks
- real tenant isolation practice through Supabase/RLS

<a id="negative-consequences"></a>

### Negative Consequences

This decision also creates risks:

- early Supabase coupling
- free-tier limitations
- background jobs are not fully solved immediately
- email reminders are deferred
- preview and dev environments may share infrastructure early
- RLS policy design may be tricky
- Supabase service role key misuse could be dangerous
- Vercel/Supabase platform limits may appear later

### Mitigations

To reduce these risks:

- document environment limitations
- do not store real customer data until production separation exists
- keep service role keys server-side only
- implement RLS carefully
- test tenant isolation early
- defer paid services until needed
- keep architecture docs updated
- record major changes in ADRs

[Back to top](#table-of-contents)

---

<a id="implementation-plan"></a>

## 24. Implementation Plan

### Step 1: Documentation

Create or update:

```text
docs/decisions/ADR-0002-technical-stack.md
docs/architecture/technical-stack.md
docs/setup/codespaces.md
docs/setup/local-macos.md
docs/setup/environment-variables.md
docs/security/tenant-isolation.md
docs/security/secrets-management.md
docs/security/threat-model.md
```

### Step 2: App Scaffold

Create the Next.js app with TypeScript.

Recommended repo shape:

```text
evidencesnap/
├── app/
├── components/
├── lib/
│   ├── supabase/
│   ├── auth/
│   ├── tenant/
│   └── audit/
├── docs/
├── supabase/
│   ├── migrations/
│   └── seed.sql
├── scripts/
├── tests/
├── .github/
│   └── workflows/
├── .env.example
├── package.json
├── next.config.ts
└── tsconfig.json
```

### Step 3: Supabase Setup

Create:

- Supabase project
- auth configuration
- storage bucket
- initial tables
- RLS policies
- demo seed data

### Step 4: CI Setup

Add GitHub Actions workflow to run:

```bash
npm ci
npm run lint
npm run typecheck
npm test
npm run build
```

### Step 5: Vercel Deployment

Configure:

- Vercel project
- environment variables
- preview deployments
- production deployment from `main`

### Step 6: SaaS Foundation Milestone

Build the first working SaaS foundation:

```text
User signs in
        ↓
User has organization membership
        ↓
Dashboard resolves current organization
        ↓
Queries are tenant-scoped
        ↓
Audit log records key action
```

[Back to top](#table-of-contents)

---

<a id="decision-review-triggers"></a>

## 25. Decision Review Triggers

This decision should be reviewed if:

- Supabase free tier becomes too limiting.
- Vercel Hobby becomes too limiting.
- The app requires backend behavior not suitable for Next.js.
- Tenant isolation cannot be implemented safely with the chosen approach.
- A paying pilot requires different infrastructure.
- Real customer evidence requires stronger backup/retention guarantees.
- Background jobs become central to MVP functionality.
- Email reminders become required earlier than expected.
- The project receives funding that changes constraints.

Until one of these triggers occurs, the accepted stack remains:

```text
Next.js + TypeScript + Supabase + Vercel + GitHub Codespaces + GitHub Actions
```

[Back to top](#table-of-contents)

---

<a id="related-documents"></a>

## 26. Related Documents

- [`README.md`](../../README.md)
- [`AI_CONTEXT.md`](../../AI_CONTEXT.md)
- [`docs/product/vision.md`](../product/vision.md)
- [`docs/architecture/data-model.md`](../architecture/data-model.md)
- [`docs/evidence-as-code/schema.md`](../evidence-as-code/schema.md)
- [`docs/decisions/ADR-0001-project-direction.md`](./ADR-0001-project-direction.md)

Planned related documents:

- [`docs/architecture/technical-stack.md`](../architecture/technical-stack.md)
- [`docs/setup/codespaces.md`](../setup/codespaces.md)
- [`docs/setup/local-macos.md`](../setup/local-macos.md)
- [`docs/setup/environment-variables.md`](../setup/environment-variables.md)
- [`docs/security/tenant-isolation.md`](../security/tenant-isolation.md)
- [`docs/security/secrets-management.md`](../security/secrets-management.md)
- [`docs/security/threat-model.md`](../security/threat-model.md)

[Back to top](#table-of-contents)