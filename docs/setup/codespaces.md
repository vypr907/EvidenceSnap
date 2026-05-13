# GitHub Codespaces Setup

> Browser-based development setup for EvidenceSnap.

This guide explains how to run EvidenceSnap in GitHub Codespaces without installing Node.js, Git, Docker, or other development tools on the local machine.

Codespaces is the preferred development path when working from a restricted machine.

---

<a id="table-of-contents"></a>

<details open>
<summary><strong>📚 Table of Contents</strong></summary>

- [1. Purpose](#purpose)
- [2. Prerequisites](#prerequisites)
- [3. Recommended Workflow](#recommended-workflow)
- [4. Create a Codespace](#create-a-codespace)
- [5. Install Dependencies](#install-dependencies)
- [6. Configure Environment Variables](#configure-environment-variables)
- [7. Run the Development Server](#run-the-development-server)
- [8. Open the App](#open-the-app)
- [9. Working with Supabase](#working-with-supabase)
- [10. Running Checks](#running-checks)
- [11. Git Workflow](#git-workflow)
- [12. Troubleshooting](#troubleshooting)
- [13. Security Notes](#security-notes)
- [14. Related Documents](#related-documents)

</details>

---

<a id="purpose"></a>

## 1. Purpose

EvidenceSnap should be buildable from a browser-based development environment.

This supports development from machines where local installation is limited or unavailable.

Codespaces should allow a contributor to:

- edit documentation
- edit application code
- install dependencies
- run the Next.js development server
- run tests
- run lint/typecheck/build commands
- commit changes
- open pull requests

[Back to top](#table-of-contents)

---

<a id="prerequisites"></a>

## 2. Prerequisites

You need:

- GitHub account
- Access to the EvidenceSnap repository
- Browser access to GitHub Codespaces
- Supabase project credentials, if running the app against Supabase
- Vercel account only if testing deployments

You do **not** need local installation of:

- Node.js
- npm
- Git
- Docker
- Postgres
- Supabase CLI

for the initial browser-based workflow.

[Back to top](#table-of-contents)

---

<a id="recommended-workflow"></a>

## 3. Recommended Workflow

Use Codespaces for:

- documentation edits
- light feature work
- pull request updates
- CI fixups
- setup validation
- small app changes

Use local MacBook development for:

- longer coding sessions
- file-heavy work
- more complex debugging
- optional local Supabase CLI work later

Recommended branch pattern:

```text
docs/*
feature/*
fix/*
security/*
```

Avoid committing directly to `main` when practical.

[Back to top](#table-of-contents)

---

<a id="create-a-codespace"></a>

## 4. Create a Codespace

From GitHub:

1. Open the EvidenceSnap repository.
2. Select **Code**.
3. Select **Codespaces**.
4. Click **Create codespace on main**.

After the Codespace starts, open the integrated terminal.

Check Node and npm:

```bash
node --version
npm --version
```

If the project later includes a `.devcontainer` configuration, Codespaces should automatically use the configured Node version and extensions.

[Back to top](#table-of-contents)

---

<a id="install-dependencies"></a>

## 5. Install Dependencies

Install project dependencies:

```bash
npm install
```

After a lockfile exists, CI and reproducible environments should use:

```bash
npm ci
```

For initial setup before the app is scaffolded, this command may not work until `package.json` exists.

[Back to top](#table-of-contents)

---

<a id="configure-environment-variables"></a>

## 6. Configure Environment Variables

Copy the example environment file:

```bash
cp .env.example .env.local
```

Edit `.env.local` inside Codespaces.

Minimum expected variables:

```bash
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=
NEXT_PUBLIC_APP_URL=
```

Do not commit `.env.local`.

### Important

Only variables prefixed with `NEXT_PUBLIC_` are safe to expose to browser code.

Never expose:

```bash
SUPABASE_SERVICE_ROLE_KEY
```

to the browser.

See:

```text
docs/setup/environment-variables.md
docs/security/secrets-management.md
```

[Back to top](#table-of-contents)

---

<a id="run-the-development-server"></a>

## 7. Run the Development Server

Start the app:

```bash
npm run dev
```

The default Next.js development server usually runs on:

```text
http://localhost:3000
```

In Codespaces, GitHub will forward the port and provide a browser-accessible URL.

[Back to top](#table-of-contents)

---

<a id="open-the-app"></a>

## 8. Open the App

When the dev server starts, Codespaces should show a forwarded port.

Use the forwarded URL to open the app.

If the app uses authentication callbacks, ensure the Codespaces forwarded URL is configured in Supabase Auth redirect settings if needed.

Example redirect URL pattern:

```text
https://<codespace-name>-3000.app.github.dev/auth/callback
```

This may change per Codespace.

For early development, authentication callback setup may be easier to test locally or through a stable Vercel preview URL.

[Back to top](#table-of-contents)

---

<a id="working-with-supabase"></a>

## 9. Working with Supabase

The initial Codespaces workflow uses a hosted Supabase project.

This avoids needing Docker or the Supabase CLI.

### Expected Supabase Resources

- Supabase Auth
- Supabase Postgres
- Supabase Storage bucket
- RLS policies
- SQL migrations

### Migration Strategy

Schema changes should be committed under:

```text
supabase/migrations/
```

Early migrations may be created manually in SQL files.

Later, the Supabase CLI may be added for local database development.

### Prototype Limitation

The early prototype may use one Supabase project for development/demo data.

Before storing real customer evidence, create separate development and production Supabase projects.

[Back to top](#table-of-contents)

---

<a id="running-checks"></a>

## 10. Running Checks

Run available checks before committing.

```bash
npm run lint
npm run typecheck
npm test
npm run build
```

Some scripts may not exist during early setup. Add them incrementally as the app is scaffolded.

Recommended eventual scripts:

```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "typecheck": "tsc --noEmit",
    "test": "vitest"
  }
}
```

[Back to top](#table-of-contents)

---

<a id="git-workflow"></a>

## 11. Git Workflow

Create a branch:

```bash
git checkout -b docs/codespaces-setup
```

Make changes.

Check status:

```bash
git status
```

Stage changes:

```bash
git add docs/setup/codespaces.md
```

Commit:

```bash
git commit -m "Add Codespaces setup documentation"
```

Push:

```bash
git push origin docs/codespaces-setup
```

Open a pull request on GitHub.

[Back to top](#table-of-contents)

---

<a id="troubleshooting"></a>

## 12. Troubleshooting

### Port Does Not Open

Restart the dev server:

```bash
npm run dev
```

Check Codespaces **Ports** tab and make sure port `3000` is forwarded.

### Environment Variables Not Loading

Confirm the file is named:

```text
.env.local
```

Restart the dev server after changing environment variables.

### Supabase Auth Redirect Fails

Check:

- Supabase Auth redirect URL settings
- Codespaces forwarded URL
- `NEXT_PUBLIC_APP_URL`
- callback route path

### Permission or Access Errors

Check:

- Supabase RLS policies
- current user membership
- selected organization
- tenant-scoped query filters

### Package Install Problems

Try:

```bash
rm -rf node_modules package-lock.json
npm install
```

Only do this if dependency resolution appears broken.

[Back to top](#table-of-contents)

---

<a id="security-notes"></a>

## 13. Security Notes

Codespaces is a development environment.

Do not store production secrets in Codespaces unless absolutely necessary.

Rules:

- Do not commit `.env.local`.
- Do not paste service role keys into public issues or PR comments.
- Do not expose service role keys to browser code.
- Use demo/test data only during early prototype development.
- Do not upload real customer evidence during the prototype phase.
- Treat Codespaces URLs as temporary development URLs.

Before real customer evidence is stored, review:

```text
docs/security/secrets-management.md
docs/security/tenant-isolation.md
docs/security/threat-model.md
```

[Back to top](#table-of-contents)

---

<a id="related-documents"></a>

## 14. Related Documents

- [`docs/architecture/technical-stack.md`](../architecture/technical-stack.md)
- [`docs/setup/local-macos.md`](./local-macos.md)
- [`docs/setup/environment-variables.md`](./environment-variables.md)
- [`docs/security/secrets-management.md`](../security/secrets-management.md)
- [`docs/security/tenant-isolation.md`](../security/tenant-isolation.md)
- [`docs/decisions/ADR-0002-technical-stack.md`](../decisions/ADR-0002-technical-stack.md)

[Back to top](#table-of-contents)