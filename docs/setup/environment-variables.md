# Environment Variables

> Environment variable reference for EvidenceSnap.

This document explains the environment variables used by the EvidenceSnap prototype.

It should be kept in sync with:

```text
.env.example
```

Do not commit real secrets.

---

<a id="table-of-contents"></a>

<details open>
<summary><strong>📚 Table of Contents</strong></summary>

- [1. Purpose](#purpose)
- [2. Environment Files](#environment-files)
- [3. Public vs Server-Side Variables](#public-vs-server-side-variables)
- [4. Required Variables](#required-variables)
- [5. Optional / Future Variables](#optional--future-variables)
- [6. Example `.env.example`](#example-envexample)
- [7. Local Development](#local-development)
- [8. Codespaces Development](#codespaces-development)
- [9. Vercel Deployment](#vercel-deployment)
- [10. Supabase Notes](#supabase-notes)
- [11. Validation Checklist](#validation-checklist)
- [12. Security Rules](#security-rules)
- [13. Related Documents](#related-documents)

</details>

---

<a id="purpose"></a>

## 1. Purpose

Environment variables configure external services and deployment-specific values.

EvidenceSnap uses environment variables for:

- Supabase connection
- authentication redirect URLs
- server-only Supabase access
- future email provider
- future billing provider
- app base URL

[Back to top](#table-of-contents)

---

<a id="environment-files"></a>

## 2. Environment Files

Recommended files:

| File | Purpose | Commit? |
|---|---|---|
| `.env.example` | Template showing required variables | Yes |
| `.env.local` | Local/Codespaces secrets | No |
| `.env.production` | Not recommended for repo storage | No |

The repo should include:

```text
.env.example
```

The repo should ignore:

```text
.env
.env.local
.env.*.local
```

[Back to top](#table-of-contents)

---

<a id="public-vs-server-side-variables"></a>

## 3. Public vs Server-Side Variables

In Next.js, variables prefixed with `NEXT_PUBLIC_` can be exposed to browser code.

### Public Variables

Public variables may be visible to users.

Examples:

```bash
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
NEXT_PUBLIC_APP_URL=
```

### Server-Side Variables

Server-side variables must never be exposed to browser code.

Examples:

```bash
SUPABASE_SERVICE_ROLE_KEY=
RESEND_API_KEY=
STRIPE_SECRET_KEY=
STRIPE_WEBHOOK_SECRET=
```

### Critical Rule

Never use server-only secrets in client components or browser-executed code.

[Back to top](#table-of-contents)

---

<a id="required-variables"></a>

## 4. Required Variables

### `NEXT_PUBLIC_SUPABASE_URL`

Supabase project URL.

Example:

```bash
NEXT_PUBLIC_SUPABASE_URL=https://example.supabase.co
```

Safe for browser exposure.

### `NEXT_PUBLIC_SUPABASE_ANON_KEY`

Supabase anonymous public key.

Example:

```bash
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
```

Safe for browser exposure when RLS policies are correctly configured.

### `SUPABASE_SERVICE_ROLE_KEY`

Supabase service role key.

Example:

```bash
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key
```

Server-side only.

This key bypasses RLS and must be treated as highly sensitive.

### `NEXT_PUBLIC_APP_URL`

Base application URL.

Local example:

```bash
NEXT_PUBLIC_APP_URL=http://localhost:3000
```

Vercel production example:

```bash
NEXT_PUBLIC_APP_URL=https://evidencesnap.example.com
```

Safe for browser exposure.

[Back to top](#table-of-contents)

---

<a id="optional--future-variables"></a>

## 5. Optional / Future Variables

These are not required for the earliest prototype but may be added later.

### Email

```bash
RESEND_API_KEY=
EMAIL_FROM=
```

### Billing

```bash
STRIPE_SECRET_KEY=
STRIPE_WEBHOOK_SECRET=
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=
```

### App Configuration

```bash
APP_ENV=
LOG_LEVEL=
```

### Storage Alternative

If Supabase Storage is replaced later:

```bash
S3_BUCKET=
S3_REGION=
S3_ACCESS_KEY_ID=
S3_SECRET_ACCESS_KEY=
```

[Back to top](#table-of-contents)

---

<a id="example-envexample"></a>

## 6. Example `.env.example`

```bash
# -----------------------------------------------------------------------------
# EvidenceSnap Environment Variables
# -----------------------------------------------------------------------------

# App
NEXT_PUBLIC_APP_URL=http://localhost:3000

# Supabase public client configuration
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=

# Supabase server-side key
# WARNING: Never expose this to browser code.
SUPABASE_SERVICE_ROLE_KEY=

# Future email provider
# RESEND_API_KEY=
# EMAIL_FROM=

# Future billing provider
# STRIPE_SECRET_KEY=
# STRIPE_WEBHOOK_SECRET=
# NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=
```

[Back to top](#table-of-contents)

---

<a id="local-development"></a>

## 7. Local Development

Create `.env.local`:

```bash
cp .env.example .env.local
```

Edit `.env.local`:

```bash
NEXT_PUBLIC_APP_URL=http://localhost:3000
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key
```

Restart the dev server after changing variables:

```bash
npm run dev
```

[Back to top](#table-of-contents)

---

<a id="codespaces-development"></a>

## 8. Codespaces Development

Codespaces may also use `.env.local`.

Create:

```bash
cp .env.example .env.local
```

For Codespaces, `NEXT_PUBLIC_APP_URL` may need to use the forwarded URL:

```bash
NEXT_PUBLIC_APP_URL=https://<codespace-name>-3000.app.github.dev
```

This URL may change when a new Codespace is created.

For auth testing, Supabase redirect URLs may need to include the Codespaces forwarded URL.

[Back to top](#table-of-contents)

---

<a id="vercel-deployment"></a>

## 9. Vercel Deployment

In Vercel, configure environment variables in the project settings.

Recommended environment separation:

| Vercel Environment | Variable Source |
|---|---|
| Development | Local `.env.local` |
| Preview | Vercel Preview environment variables |
| Production | Vercel Production environment variables |

Before real customer data:

- preview and production may temporarily use the same demo Supabase project
- this must be documented as prototype-only

Before real customer evidence:

- production should use a separate Supabase project
- production secrets should be distinct from development secrets

[Back to top](#table-of-contents)

---

<a id="supabase-notes"></a>

## 10. Supabase Notes

Supabase provides:

- Project URL
- anon public key
- service role key

### Anon Key

The anon key is intended for client access.

It relies on RLS policies for data protection.

### Service Role Key

The service role key bypasses RLS.

Use only in:

- server-side code
- admin scripts
- controlled maintenance tasks

Never use it in:

- client components
- browser bundles
- public docs
- screenshots
- issue comments

[Back to top](#table-of-contents)

---

<a id="validation-checklist"></a>

## 11. Validation Checklist

Use this checklist when setting up environment variables.

- [ ] `.env.local` exists locally.
- [ ] `.env.local` is ignored by Git.
- [ ] `.env.example` contains no real secrets.
- [ ] `NEXT_PUBLIC_SUPABASE_URL` is set.
- [ ] `NEXT_PUBLIC_SUPABASE_ANON_KEY` is set.
- [ ] `SUPABASE_SERVICE_ROLE_KEY` is set only where server-side use is required.
- [ ] `NEXT_PUBLIC_APP_URL` matches the current environment.
- [ ] Supabase Auth redirect URLs match the app URL.
- [ ] Vercel environment variables are configured.
- [ ] No secrets are committed.

[Back to top](#table-of-contents)

---

<a id="security-rules"></a>

## 12. Security Rules

Environment variable rules:

1. Never commit real secrets.
2. Keep `.env.example` safe and empty of real values.
3. Treat service role keys as highly sensitive.
4. Do not expose server-only keys to browser code.
5. Rotate keys if exposed.
6. Use separate production secrets before real customer data.
7. Document every required variable.
8. Keep Vercel and GitHub secrets scoped to the environment where they are needed.

[Back to top](#table-of-contents)

---

<a id="related-documents"></a>

## 13. Related Documents

- [`docs/setup/codespaces.md`](./codespaces.md)
- [`docs/setup/local-macos.md`](./local-macos.md)
- [`docs/security/secrets-management.md`](../security/secrets-management.md)
- [`docs/security/tenant-isolation.md`](../security/tenant-isolation.md)
- [`docs/architecture/technical-stack.md`](../architecture/technical-stack.md)
- [`docs/decisions/ADR-0002-technical-stack.md`](../decisions/ADR-0002-technical-stack.md)

[Back to top](#table-of-contents)