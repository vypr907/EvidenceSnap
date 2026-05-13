# EvidenceSnap Technical Stack

> Free-first SaaS-native technical stack for the EvidenceSnap MVP.

EvidenceSnap is a SaaS-native compliance evidence tracking platform. The initial technical stack is designed to support real product development while staying within a zero-cost startup budget.

This stack prioritizes:

- Free-tier services
- SaaS-native architecture
- Browser-based development through GitHub Codespaces
- MacBook-friendly local development
- Strong documentation
- CI/CD and DevSecOps practices
- Tenant isolation
- Evidence storage
- Auditability
- Future upgrade paths

This document expands on the decision recorded in [`ADR-0002: Free-First Technical Stack and DevSecOps Direction`](../decisions/ADR-0002-technical-stack.md).

---

<a id="table-of-contents"></a>

<details open>
<summary><strong>📚 Table of Contents</strong></summary>

- [1. Stack Summary](#stack-summary)
- [2. Stack Goals](#stack-goals)
- [3. Architecture Overview](#architecture-overview)
- [4. Selected Tools](#selected-tools)
  - [GitHub](#github)
  - [GitHub Codespaces](#github-codespaces)
  - [Next.js](#nextjs)
  - [TypeScript](#typescript)
  - [Supabase](#supabase)
  - [Vercel](#vercel)
  - [GitHub Actions](#github-actions)
- [5. Application Architecture](#application-architecture)
- [6. Repository Structure](#repository-structure)
- [7. Environment Strategy](#environment-strategy)
- [8. Authentication and Authorization](#authentication-and-authorization)
- [9. Tenant Isolation](#tenant-isolation)
- [10. Database Strategy](#database-strategy)
- [11. Storage Strategy](#storage-strategy)
- [12. CI/CD Strategy](#cicd-strategy)
- [13. DevSecOps Strategy](#devsecops-strategy)
- [14. Testing Strategy](#testing-strategy)
- [15. Observability Strategy](#observability-strategy)
- [16. Deferred Services](#deferred-services)
- [17. Upgrade Paths](#upgrade-paths)
- [18. Risks and Mitigations](#risks-and-mitigations)
- [19. Initial Implementation Checklist](#initial-implementation-checklist)
- [20. Related Documents](#related-documents)

</details>

---

<a id="stack-summary"></a>

## 1. Stack Summary

The selected initial stack is:

```text
Next.js + TypeScript + Supabase + Vercel + GitHub Codespaces + GitHub Actions
```

| Layer | Selected Tool | Purpose |
|---|---|---|
| Source Control | GitHub | Repository, issues, docs, pull requests |
| Cloud Dev Environment | GitHub Codespaces | Browser-based development |
| Local Dev Environment | MacBook Pro | Home development |
| Frontend | Next.js | React-based application UI |
| Backend | Next.js API routes / server actions | Server-side application logic |
| Language | TypeScript | Type safety and maintainability |
| Database | Supabase Postgres | Managed Postgres database |
| Authentication | Supabase Auth | User authentication |
| Authorization | Supabase + app-layer RBAC | Organization membership and roles |
| Storage | Supabase Storage | Private evidence file storage |
| Deployment | Vercel Hobby | Free Next.js hosting |
| CI | GitHub Actions | Automated validation |
| Security Automation | Dependabot, CodeQL, secret scanning | Basic DevSecOps checks |
| Email | Deferred; likely Resend later | Reminder emails after MVP foundation |
| Billing | Deferred; likely Stripe later | Paid plans after pilot readiness |
| Background Jobs | Deferred | Scheduled workflows after core app works |

[Back to top](#table-of-contents)

---

<a id="stack-goals"></a>

## 2. Stack Goals

The technical stack should support EvidenceSnap’s product and project goals.

### Product Goals

The stack must support:

- Multi-tenant organizations
- Authentication
- Role-based access control
- Evidence requirements
- Evidence requests
- Evidence uploads
- Review workflow
- Audit logging
- Export package generation
- Future evidence-as-code import/export
- Future reminder emails
- Future billing

### Project Goals

The stack must also support:

- Zero-cost initial development
- Browser-based work from restricted machines
- Local development from a MacBook Pro
- Clean documentation
- Repeatable setup
- CI/CD automation
- DevSecOps credibility
- Portfolio-quality engineering practices

### Guiding Principle

The stack should be simple enough to build quickly but real enough to support the long-term SaaS product.

[Back to top](#table-of-contents)

---

<a id="architecture-overview"></a>

## 3. Architecture Overview

Initial architecture:

```text
User Browser
    ↓
Next.js App on Vercel
    ↓
Supabase Auth
Supabase Postgres
Supabase Storage
    ↓
GitHub Actions / Vercel Deployments
```

Detailed flow:

```text
User signs in
    ↓
Supabase Auth verifies identity
    ↓
Next.js resolves user profile and organization membership
    ↓
Application queries tenant-scoped data by org_id
    ↓
Supabase RLS provides database-layer tenant protection
    ↓
Evidence files are stored privately in Supabase Storage
    ↓
Downloads use signed URLs after authorization checks
    ↓
Important actions are written to Audit Logs
```

The first engineering milestone should prove:

```text
Authenticated user
    ↓
Organization membership
    ↓
Tenant-scoped dashboard
    ↓
Audit logged action
```

[Back to top](#table-of-contents)

---

<a id="selected-tools"></a>

## 4. Selected Tools

<a id="github"></a>

### GitHub

GitHub is the source of truth for the project.

It stores:

- application code
- documentation
- ADRs
- database migrations
- evidence-as-code schemas
- setup guides
- CI/CD workflows
- issue tracking
- release notes

Recommended branch patterns:

```text
main
feature/*
docs/*
fix/*
security/*
```

The `main` branch should represent the stable deployable state.

[Back to top](#table-of-contents)

---

<a id="github-codespaces"></a>

### GitHub Codespaces

GitHub Codespaces provides a browser-based development environment.

This is important because the founder may be limited in what can be installed on a work machine.

Codespaces should support:

- editing docs
- running the Next.js app
- installing dependencies
- running tests
- committing changes
- opening pull requests
- validating setup instructions

A future setup doc should explain how to use Codespaces:

```text
docs/setup/codespaces.md
```

A future `.devcontainer` configuration may be added to standardize the environment.

[Back to top](#table-of-contents)

---

<a id="nextjs"></a>

### Next.js

Next.js is the selected application framework.

It will provide:

- React UI
- routing
- layouts
- server components where useful
- API routes or server actions
- Vercel deployment compatibility
- TypeScript support

### Initial Backend Approach

Use Next.js API routes and/or server actions for MVP backend logic.

A separate backend service is deferred until there is a clear need.

Potential future backend options:

- FastAPI
- dedicated Node service
- serverless workers
- queue workers

[Back to top](#table-of-contents)

---

<a id="typescript"></a>

### TypeScript

TypeScript is the selected language for application code.

It supports:

- safer refactoring
- clearer interfaces
- typed data models
- better editor support
- improved maintainability
- stronger portfolio signal

Recommended TypeScript posture:

- enable strict mode
- type shared data structures
- avoid `any` where practical
- generate Supabase types later
- define role/status enums clearly

[Back to top](#table-of-contents)

---

<a id="supabase"></a>

### Supabase

Supabase is selected for the initial backend platform.

It provides:

- Postgres database
- authentication
- object storage
- Row Level Security
- dashboard tools
- API access
- SQL migrations

Supabase is selected because it reduces the number of services required in the zero-cost MVP.

### Supabase Responsibilities

| Supabase Feature | EvidenceSnap Use |
|---|---|
| Auth | User authentication |
| Postgres | Application database |
| RLS | Tenant isolation |
| Storage | Evidence files and export packages |
| SQL Editor / CLI | Migrations and setup |
| Dashboard | Early development/admin visibility |

### Supabase Caution

The service role key must never be exposed to browser code.

Only the anon key should be public.

[Back to top](#table-of-contents)

---

<a id="vercel"></a>

### Vercel

Vercel is selected for initial hosting.

It provides:

- free hobby deployment
- GitHub integration
- automatic deployments
- preview deployments
- environment variable configuration
- strong Next.js support

### Deployment Model

| Event | Deployment |
|---|---|
| Pull request | Preview deployment |
| Merge to `main` | Production deployment |
| Local/Codespaces | Development server |

Before real customer data is stored, production and preview environment separation must be reviewed.

[Back to top](#table-of-contents)

---

<a id="github-actions"></a>

### GitHub Actions

GitHub Actions will provide CI checks.

Initial CI should run:

```bash
npm ci
npm run lint
npm run typecheck
npm test
npm run build
```

The exact scripts may be added incrementally as the project is scaffolded.

GitHub Actions should eventually include:

- linting
- type checking
- tests
- build validation
- dependency checks
- CodeQL scanning

[Back to top](#table-of-contents)

---

<a id="application-architecture"></a>

## 5. Application Architecture

Initial application structure should keep boundaries clear.

Recommended internal modules:

```text
lib/
├── supabase/
├── auth/
├── tenant/
├── audit/
├── rbac/
├── evidence/
└── storage/
```

### Suggested Responsibilities

| Module | Responsibility |
|---|---|
| `lib/supabase` | Supabase client helpers |
| `lib/auth` | Current user/session helpers |
| `lib/tenant` | Organization resolution and tenant filtering |
| `lib/rbac` | Role checks and permissions |
| `lib/audit` | Audit log writing |
| `lib/evidence` | Evidence workflow business logic |
| `lib/storage` | Upload/download/signed URL helpers |

### Core Rule

Business logic should not be scattered directly across UI components.

Tenant resolution, RBAC, audit logging, and storage access should have reusable helper functions.

[Back to top](#table-of-contents)

---

<a id="repository-structure"></a>

## 6. Repository Structure

Recommended initial repo structure:

```text
evidencesnap/
├── README.md
├── AI_CONTEXT.md
├── CHANGELOG.md
├── .env.example
├── package.json
├── next.config.ts
├── tsconfig.json
├── app/
│   ├── layout.tsx
│   ├── page.tsx
│   ├── dashboard/
│   └── auth/
├── components/
│   ├── ui/
│   └── layout/
├── lib/
│   ├── supabase/
│   ├── auth/
│   ├── tenant/
│   ├── rbac/
│   ├── audit/
│   ├── evidence/
│   └── storage/
├── docs/
│   ├── product/
│   ├── architecture/
│   ├── evidence-as-code/
│   ├── decisions/
│   ├── setup/
│   └── security/
├── supabase/
│   ├── migrations/
│   └── seed.sql
├── scripts/
├── tests/
└── .github/
    └── workflows/
```

### Documentation Folders

| Folder | Purpose |
|---|---|
| `docs/product` | Product vision, roadmap, target customers |
| `docs/architecture` | Data model, stack, system architecture |
| `docs/evidence-as-code` | Schema and examples |
| `docs/decisions` | ADRs |
| `docs/setup` | Local, Codespaces, env var setup |
| `docs/security` | Tenant isolation, secrets, threat model |

[Back to top](#table-of-contents)

---

<a id="environment-strategy"></a>

## 7. Environment Strategy

EvidenceSnap should support three environment concepts:

```text
local / codespaces
preview
production
```

### Local / Codespaces

Used for active development.

May connect to the hosted development Supabase project.

### Preview

Vercel preview deployments for branches and pull requests.

Initially, preview deployments may share the development Supabase project.

### Production

Deployment from `main`.

Before real customer data is stored, production should use a separate Supabase project.

### Free-First Compromise

The earliest prototype may use one Supabase project for development/demo data.

This is acceptable only while:

- no real customer evidence is stored
- the limitation is documented
- demo data is clearly non-sensitive
- production separation is planned before pilot use

### Required Before Real Customer Evidence

Before storing real customer evidence:

- create separate development and production Supabase projects
- separate environment variables
- review RLS policies
- verify private storage bucket behavior
- verify signed URL behavior
- document backup and retention expectations

[Back to top](#table-of-contents)

---

<a id="authentication-and-authorization"></a>

## 8. Authentication and Authorization

### Authentication

Supabase Auth will provide authentication.

Initial supported methods may include:

- email/password
- magic link, if useful

### Authorization

Authorization will be based on organization membership and role.

Core roles:

```text
Admin
Compliance Manager
Evidence Owner
Reviewer
Auditor / Read-only
```

### Membership Pattern

A user may belong to one or more organizations through a membership table.

```text
users/profiles
    ↓
memberships
    ↓
organizations
```

### Authorization Rule

A user’s access is determined by:

```text
authenticated user
        +
organization membership
        +
role
```

[Back to top](#table-of-contents)

---

<a id="tenant-isolation"></a>

## 9. Tenant Isolation

Tenant isolation is a critical security requirement.

EvidenceSnap should use defense in depth:

1. application-layer organization filtering
2. database-layer Row Level Security
3. private storage paths
4. signed URLs after authorization checks
5. audit logging for sensitive actions

### Required Query Pattern

Every tenant-scoped query must include `org_id`.

Example:

```sql
SELECT *
FROM evidence_requests
WHERE org_id = :current_org_id;
```

### RLS Direction

Supabase RLS should be enabled on tenant-scoped tables.

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

<a id="database-strategy"></a>

## 10. Database Strategy

EvidenceSnap will use Supabase Postgres.

### Initial Database Priorities

The first database milestone should include:

```text
organizations
profiles
memberships
audit_logs
```

The next milestone should include:

```text
systems
frameworks
controls
evidence_requirements
```

Then:

```text
evidence_periods
evidence_requests
evidence_items
review_comments
export_packages
```

### Migration Strategy

Schema changes should be version-controlled.

Preferred location:

```text
supabase/migrations/
```

### Seed Data

Demo seed data should live separately from schema migrations.

Preferred location:

```text
supabase/seed.sql
```

### ORM Strategy

No ORM is selected at this stage.

Initial implementation may use:

- Supabase client queries
- SQL migrations
- generated TypeScript types later

Potential future ORM options:

- Drizzle
- Prisma

The ORM decision is deferred until the schema stabilizes.

[Back to top](#table-of-contents)

---

<a id="storage-strategy"></a>

## 11. Storage Strategy

Supabase Storage will store evidence files and export packages during the MVP.

### Storage Requirements

Evidence files must be:

- private
- tenant-scoped
- associated with Evidence Items
- accessed only after authorization
- downloaded using signed URLs
- tracked with metadata
- hashed for integrity

### Evidence Storage Path

```text
/orgs/{org_id}/evidence/{period_id}/{request_id}/{evidence_item_id}/{filename}
```

### Export Package Storage Path

```text
/orgs/{org_id}/exports/{period_id}/{export_package_id}/package.zip
```

### Evidence Item Metadata

The database should store:

- original filename
- file type
- file size
- storage path
- SHA-256 hash
- uploaded by
- uploaded at
- evidence date
- source system

### Future Storage Options

Potential future alternatives:

- S3
- Cloudflare R2
- Azure Blob Storage

Supabase Storage is acceptable for MVP.

[Back to top](#table-of-contents)

---

<a id="cicd-strategy"></a>

## 12. CI/CD Strategy

The CI/CD strategy should be simple but present from the beginning.

### CI

GitHub Actions should validate code before merge.

Initial workflow:

```bash
npm ci
npm run lint
npm run typecheck
npm test
npm run build
```

If some scripts do not exist yet, they should be added incrementally.

### CD

Vercel handles deployment.

| Event | Result |
|---|---|
| Pull request opened | Preview deployment |
| Pull request updated | Preview redeployment |
| Merge to `main` | Production deployment |

### CI/CD Goals

The pipeline should prove:

- dependencies install cleanly
- code passes linting
- TypeScript compiles
- tests pass
- production build succeeds

[Back to top](#table-of-contents)

---

<a id="devsecops-strategy"></a>

## 13. DevSecOps Strategy

EvidenceSnap should demonstrate practical DevSecOps discipline from the start.

### Initial DevSecOps Controls

| Control | Tool / Practice |
|---|---|
| Dependency updates | Dependabot |
| Secret scanning | GitHub secret scanning |
| Static analysis | CodeQL |
| Type safety | TypeScript strict mode |
| Linting | ESLint |
| Build validation | GitHub Actions |
| Tenant isolation | App-layer filtering + Supabase RLS |
| Storage security | Private buckets + signed URLs |
| Auditability | Application Audit Log |
| Migration traceability | Version-controlled SQL migrations |

### Security Documents

The repo should include:

```text
docs/security/tenant-isolation.md
docs/security/secrets-management.md
docs/security/threat-model.md
```

### Supply Chain Guidance

- Use a lockfile.
- Use `npm ci` in CI.
- Avoid unnecessary dependencies.
- Review packages before adding them.
- Keep dependencies updated.
- Treat high/critical advisories seriously.

[Back to top](#table-of-contents)

---

<a id="testing-strategy"></a>

## 14. Testing Strategy

Testing should begin with the highest-risk logic.

### Priority Test Areas

- tenant isolation
- membership resolution
- role checks
- evidence request generation
- duplicate request prevention
- status transitions
- evidence validation rules
- export manifest generation

### Critical Test Cases

```text
User from Org A cannot see Org B records.
User without membership cannot access organization records.
Only reviewers can approve evidence.
Rejected evidence requires a comment.
Waived evidence requires a reason.
Evidence Request generation does not create duplicates.
Export packages only include selected org records.
```

### Tooling

Recommended initial testing tools:

- Vitest
- React Testing Library
- Playwright later for end-to-end tests

### Testing Guidance

Do not overbuild tests before the application exists, but add tests as soon as tenant helpers and evidence generation logic are created.

[Back to top](#table-of-contents)

---

<a id="observability-strategy"></a>

## 15. Observability Strategy

Initial observability should rely on platform logs and application audit logs.

### MVP Observability

Use:

- Vercel logs
- Supabase logs
- application audit logs
- basic structured server logging
- UI error boundaries

### Future Observability

Potential future tools:

- Sentry
- OpenTelemetry
- uptime monitoring
- job failure alerts
- storage access logging

### Guidance

Do not add paid observability services during the free-first prototype unless there is a clear need.

[Back to top](#table-of-contents)

---

<a id="deferred-services"></a>

## 16. Deferred Services

Some services are intentionally deferred.

| Service / Decision | Deferred Until |
|---|---|
| Production email provider | Reminder emails are needed for beta |
| Stripe billing | Someone is ready to pay |
| Background job platform | Scheduled workflows become necessary |
| ORM | Schema stabilizes |
| Dedicated backend | Next.js backend becomes insufficient |
| AI provider | Core evidence workflow works |
| External object storage | Supabase Storage becomes limiting |
| Full staging environment | Before real customer data |
| Enterprise SSO | Paying customer requires it |
| Full OSCAL support | Evidence workflow is validated |

[Back to top](#table-of-contents)

---

<a id="upgrade-paths"></a>

## 17. Upgrade Paths

The initial stack should not trap the project.

### Auth Upgrade Paths

Current:

```text
Supabase Auth
```

Potential future:

```text
Clerk
Auth0
Enterprise SSO
SAML/OIDC provider support
```

### Database Upgrade Paths

Current:

```text
Supabase Postgres
```

Potential future:

```text
Dedicated Postgres
Neon
AWS RDS
Supabase paid tier
```

### Storage Upgrade Paths

Current:

```text
Supabase Storage
```

Potential future:

```text
Cloudflare R2
AWS S3
Azure Blob Storage
```

### Background Job Upgrade Paths

Current:

```text
manual/admin-triggered workflows
```

Potential future:

```text
Supabase scheduled functions
Vercel Cron
GitHub Actions scheduled jobs
Inngest
Trigger.dev
Celery
```

### Backend Upgrade Paths

Current:

```text
Next.js API routes / server actions
```

Potential future:

```text
FastAPI
dedicated Node API
serverless workers
background worker service
```

[Back to top](#table-of-contents)

---

<a id="risks-and-mitigations"></a>

## 18. Risks and Mitigations

| Risk | Impact | Mitigation |
|---|---|---|
| Supabase coupling | Harder migration later | Keep business logic in app modules, not scattered queries |
| Free-tier limits | Prototype may hit limits | Use only demo data; upgrade when justified |
| RLS complexity | Tenant isolation bugs | Keep policies simple; test tenant isolation early |
| Shared dev/preview DB | Environment confusion | Use demo data only; separate before customer use |
| Service role key exposure | Severe security issue | Keep server-only; document secrets clearly |
| Deferred email | No automated reminders early | Mock/log reminder events |
| Deferred jobs | Manual workflows initially | Add scheduled jobs after core workflow |
| Too much documentation | Slower coding | Generate implementation docs, then scaffold app |
| Too little documentation | Hard to distribute | Treat setup docs as MVP assets |

[Back to top](#table-of-contents)

---

<a id="initial-implementation-checklist"></a>

## 19. Initial Implementation Checklist

### Documentation

- [ ] `docs/architecture/technical-stack.md`
- [ ] `docs/setup/codespaces.md`
- [ ] `docs/setup/local-macos.md`
- [ ] `docs/setup/environment-variables.md`
- [ ] `docs/security/tenant-isolation.md`
- [ ] `docs/security/secrets-management.md`
- [ ] `docs/security/threat-model.md`

### App Scaffold

- [ ] Create Next.js app
- [ ] Enable TypeScript
- [ ] Configure ESLint
- [ ] Add `.env.example`
- [ ] Create base app layout
- [ ] Create basic dashboard route

### Supabase

- [ ] Create Supabase project
- [ ] Configure Supabase Auth
- [ ] Create initial storage bucket
- [ ] Create initial migrations
- [ ] Add RLS policies
- [ ] Add demo seed data

### SaaS Foundation

- [ ] User can sign in
- [ ] User profile can be resolved
- [ ] Organization can be created
- [ ] Membership can be resolved
- [ ] Dashboard shows current organization
- [ ] Tenant-scoped query helper exists
- [ ] Audit log helper exists

### CI/CD

- [ ] Add GitHub Actions workflow
- [ ] Add lint check
- [ ] Add typecheck
- [ ] Add test script
- [ ] Add build validation
- [ ] Connect Vercel deployment

[Back to top](#table-of-contents)

---

<a id="related-documents"></a>

## 20. Related Documents

- [`README.md`](../../README.md)
- [`AI_CONTEXT.md`](../../AI_CONTEXT.md)
- [`docs/product/vision.md`](../product/vision.md)
- [`docs/architecture/data-model.md`](./data-model.md)
- [`docs/evidence-as-code/schema.md`](../evidence-as-code/schema.md)
- [`docs/decisions/ADR-0001-project-direction.md`](../decisions/ADR-0001-project-direction.md)
- [`docs/decisions/ADR-0002-technical-stack.md`](../decisions/ADR-0002-technical-stack.md)

Planned related documents:

- [`docs/setup/codespaces.md`](../setup/codespaces.md)
- [`docs/setup/local-macos.md`](../setup/local-macos.md)
- [`docs/setup/environment-variables.md`](../setup/environment-variables.md)
- [`docs/security/tenant-isolation.md`](../security/tenant-isolation.md)
- [`docs/security/secrets-management.md`](../security/secrets-management.md)
- [`docs/security/threat-model.md`](../security/threat-model.md)

[Back to top](#table-of-contents)