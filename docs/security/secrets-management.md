# Secrets Management

> Guidance for handling secrets, keys, and sensitive configuration in EvidenceSnap.

EvidenceSnap stores compliance evidence and tenant data. Secrets management must be handled carefully from the beginning.

---

<a id="table-of-contents"></a>

<details open>
<summary><strong>📚 Table of Contents</strong></summary>

- [1. Purpose](#purpose)
- [2. Core Rules](#core-rules)
- [3. Secret Types](#secret-types)
- [4. Public vs Private Variables](#public-vs-private-variables)
- [5. Supabase Secrets](#supabase-secrets)
- [6. Local Development Secrets](#local-development-secrets)
- [7. Codespaces Secrets](#codespaces-secrets)
- [8. Vercel Secrets](#vercel-secrets)
- [9. GitHub Actions Secrets](#github-actions-secrets)
- [10. Secret Rotation](#secret-rotation)
- [11. Accidental Exposure Response](#accidental-exposure-response)
- [12. Secret Scanning](#secret-scanning)
- [13. Checklist](#checklist)
- [14. Related Documents](#related-documents)

</details>

---

<a id="purpose"></a>

## 1. Purpose

This document defines how EvidenceSnap handles secrets and sensitive configuration.

It applies to:

- local development
- GitHub Codespaces
- GitHub Actions
- Vercel deployments
- Supabase keys
- future email provider keys
- future Stripe keys
- future storage provider keys

[Back to top](#table-of-contents)

---

<a id="core-rules"></a>

## 2. Core Rules

Secrets management rules:

1. Never commit real secrets.
2. Commit `.env.example`, not `.env.local`.
3. Use environment-specific secrets.
4. Keep service role keys server-side only.
5. Do not expose privileged secrets to browser code.
6. Rotate secrets if exposed.
7. Use least privilege where possible.
8. Document every required variable.
9. Do not paste secrets into issues, pull requests, screenshots, or chat.
10. Treat compliance evidence access keys as sensitive.

[Back to top](#table-of-contents)

---

<a id="secret-types"></a>

## 3. Secret Types

Expected secret categories:

| Secret | Sensitivity | Notes |
|---|---|---|
| Supabase anon key | Public-ish | Safe only with RLS correctly configured |
| Supabase service role key | High | Bypasses RLS |
| Resend API key | High | Future email sending |
| Stripe secret key | High | Future billing |
| Stripe webhook secret | High | Future billing webhook validation |
| Storage provider secret | High | Future external object storage |
| GitHub tokens | High | CI/CD automation |
| Vercel tokens | High | Deployment automation |

[Back to top](#table-of-contents)

---

<a id="public-vs-private-variables"></a>

## 4. Public vs Private Variables

### Public Variables

In Next.js, variables prefixed with `NEXT_PUBLIC_` may be exposed to browser code.

Examples:

```bash
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
NEXT_PUBLIC_APP_URL=
```

### Private Variables

Private variables must remain server-side.

Examples:

```bash
SUPABASE_SERVICE_ROLE_KEY=
RESEND_API_KEY=
STRIPE_SECRET_KEY=
STRIPE_WEBHOOK_SECRET=
```

### Rule

Never use private variables in client components or browser-executed code.

[Back to top](#table-of-contents)

---

<a id="supabase-secrets"></a>

## 5. Supabase Secrets

### Supabase URL

```bash
NEXT_PUBLIC_SUPABASE_URL=
```

Public.

### Supabase Anon Key

```bash
NEXT_PUBLIC_SUPABASE_ANON_KEY=
```

Public client key.

This key is safe only when RLS policies are correctly configured.

### Supabase Service Role Key

```bash
SUPABASE_SERVICE_ROLE_KEY=
```

Highly sensitive.

The service role key bypasses RLS.

Use only:

- server-side admin operations
- trusted scripts
- controlled maintenance tasks

Never use:

- in browser code
- in client components
- in public docs
- in screenshots
- in issue comments

[Back to top](#table-of-contents)

---

<a id="local-development-secrets"></a>

## 6. Local Development Secrets

Local secrets should be stored in:

```text
.env.local
```

This file must be ignored by Git.

Recommended `.gitignore` entries:

```gitignore
.env
.env.local
.env.*.local
```

Local `.env.local` should contain development/demo secrets only.

Do not use production secrets locally unless necessary.

[Back to top](#table-of-contents)

---

<a id="codespaces-secrets"></a>

## 7. Codespaces Secrets

For early development, Codespaces may use `.env.local`.

For more mature workflows, use GitHub Codespaces secrets.

Recommended guidance:

- use demo/dev secrets only
- avoid production secrets in Codespaces
- do not expose secrets in terminal screenshots
- delete Codespaces that are no longer needed
- rotate keys if there is any concern of exposure

[Back to top](#table-of-contents)

---

<a id="vercel-secrets"></a>

## 8. Vercel Secrets

Vercel environment variables should be configured through Vercel project settings.

Use separate values for:

- Preview
- Production

Before real customer data:

- production should have its own Supabase project
- production secrets should be distinct
- preview should not use production service role keys unless absolutely necessary

Vercel should store:

```bash
NEXT_PUBLIC_SUPABASE_URL
NEXT_PUBLIC_SUPABASE_ANON_KEY
SUPABASE_SERVICE_ROLE_KEY
NEXT_PUBLIC_APP_URL
```

Future:

```bash
RESEND_API_KEY
STRIPE_SECRET_KEY
STRIPE_WEBHOOK_SECRET
```

[Back to top](#table-of-contents)

---

<a id="github-actions-secrets"></a>

## 9. GitHub Actions Secrets

GitHub Actions secrets should store CI-only secrets.

Do not add secrets to GitHub Actions unless the workflow needs them.

Initial CI may not require Supabase secrets if it only runs:

```bash
npm ci
npm run lint
npm run typecheck
npm test
npm run build
```

If integration tests need Supabase later, use GitHub Actions secrets carefully.

[Back to top](#table-of-contents)

---

<a id="secret-rotation"></a>

## 10. Secret Rotation

Rotate secrets when:

- a secret is committed
- a secret is pasted into an issue/PR/chat
- a secret appears in a screenshot
- a contributor leaves and had access
- suspicious access is detected
- moving from prototype to production
- switching environments

Rotation steps:

1. Revoke or regenerate the secret in the provider dashboard.
2. Update local `.env.local`.
3. Update Vercel environment variables.
4. Update GitHub Actions secrets, if used.
5. Redeploy affected environments.
6. Confirm old secret no longer works.
7. Document the incident if relevant.

[Back to top](#table-of-contents)

---

<a id="accidental-exposure-response"></a>

## 11. Accidental Exposure Response

If a secret is exposed:

1. Stop using the exposed key.
2. Rotate the key immediately.
3. Remove the secret from the file/comment/screenshot if possible.
4. Check Git history if the secret was committed.
5. Consider repository secret scanning results.
6. Review provider logs if available.
7. Document what happened.
8. Review how to prevent recurrence.

### If a Secret Was Committed

Do not assume deleting the file in a later commit is enough.

The key must be rotated.

For public repositories, treat committed secrets as compromised.

[Back to top](#table-of-contents)

---

<a id="secret-scanning"></a>

## 12. Secret Scanning

Recommended GitHub features:

- secret scanning
- push protection, if available
- Dependabot
- CodeQL

Additional options later:

- pre-commit hooks
- gitleaks
- trufflehog

For MVP, GitHub secret scanning and careful `.gitignore` hygiene are the minimum.

[Back to top](#table-of-contents)

---

<a id="checklist"></a>

## 13. Checklist

### Repo Checklist

- [ ] `.env.example` exists.
- [ ] `.env.local` is ignored.
- [ ] No real secrets are in docs.
- [ ] No real secrets are in screenshots.
- [ ] No real secrets are in code comments.

### Local Checklist

- [ ] `.env.local` exists.
- [ ] Only dev/demo secrets are used.
- [ ] Production secrets are not stored locally unless required.

### Vercel Checklist

- [ ] Preview environment variables configured.
- [ ] Production environment variables configured.
- [ ] Production uses separate secrets before customer data.
- [ ] Service role key is not exposed to browser code.

### Supabase Checklist

- [ ] Anon key is used only with RLS.
- [ ] Service role key is server-side only.
- [ ] RLS is reviewed before customer data.
- [ ] Storage buckets are private.

[Back to top](#table-of-contents)

---

<a id="related-documents"></a>

## 14. Related Documents

- [`docs/setup/environment-variables.md`](../setup/environment-variables.md)
- [`docs/security/tenant-isolation.md`](./tenant-isolation.md)
- [`docs/security/threat-model.md`](./threat-model.md)
- [`docs/architecture/technical-stack.md`](../architecture/technical-stack.md)
- [`docs/decisions/ADR-0002-technical-stack.md`](../decisions/ADR-0002-technical-stack.md)

[Back to top](#table-of-contents)