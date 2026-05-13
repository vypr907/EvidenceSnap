# Local macOS Setup

> Local development setup for EvidenceSnap on a MacBook Pro.

This guide explains how to run EvidenceSnap locally on macOS.

The local setup is intended for longer development sessions, deeper debugging, and work that is more comfortable outside GitHub Codespaces.

---

<a id="table-of-contents"></a>

<details open>
<summary><strong>📚 Table of Contents</strong></summary>

- [1. Purpose](#purpose)
- [2. Prerequisites](#prerequisites)
- [3. Recommended Tools](#recommended-tools)
- [4. Clone the Repository](#clone-the-repository)
- [5. Install Dependencies](#install-dependencies)
- [6. Configure Environment Variables](#configure-environment-variables)
- [7. Run the Development Server](#run-the-development-server)
- [8. Supabase Development Approach](#supabase-development-approach)
- [9. Running Checks](#running-checks)
- [10. Git Workflow](#git-workflow)
- [11. Optional Tools](#optional-tools)
- [12. Troubleshooting](#troubleshooting)
- [13. Security Notes](#security-notes)
- [14. Related Documents](#related-documents)

</details>

---

<a id="purpose"></a>

## 1. Purpose

The local macOS workflow should allow the founder or a contributor to:

- run the app locally
- edit code and docs
- connect to Supabase
- test authentication
- run build checks
- commit and push changes
- eventually work with local Supabase tooling

This setup should remain simple during the early prototype.

[Back to top](#table-of-contents)

---

<a id="prerequisites"></a>

## 2. Prerequisites

Install:

- Git
- Node.js LTS
- npm
- VS Code, Cursor, or another editor
- GitHub account
- Supabase account
- Access to the EvidenceSnap repository

Recommended Node installation methods:

- official Node.js installer
- Homebrew
- `nvm`

Using `nvm` is recommended if you frequently work across projects.

[Back to top](#table-of-contents)

---

<a id="recommended-tools"></a>

## 3. Recommended Tools

### Required

```text
Git
Node.js LTS
npm
code editor
```

### Recommended

```text
nvm
VS Code or Cursor
GitHub CLI
Supabase CLI, later
```

### Not Required Initially

```text
Docker
local Postgres
local Supabase
paid cloud services
```

The initial local workflow can connect to the hosted Supabase free project.

[Back to top](#table-of-contents)

---

<a id="clone-the-repository"></a>

## 4. Clone the Repository

Clone the repo:

```bash
git clone https://github.com/<your-username>/evidencesnap.git
cd evidencesnap
```

Check the current branch:

```bash
git branch
```

Create a feature branch:

```bash
git checkout -b feature/local-setup-test
```

[Back to top](#table-of-contents)

---

<a id="install-dependencies"></a>

## 5. Install Dependencies

Install dependencies:

```bash
npm install
```

After the lockfile is established, CI should use:

```bash
npm ci
```

If Node version issues occur, install and use the recommended Node LTS version.

Example with `nvm`:

```bash
nvm install --lts
nvm use --lts
```

[Back to top](#table-of-contents)

---

<a id="configure-environment-variables"></a>

## 6. Configure Environment Variables

Copy the example file:

```bash
cp .env.example .env.local
```

Edit `.env.local`:

```bash
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=
NEXT_PUBLIC_APP_URL=http://localhost:3000
```

Never commit `.env.local`.

### Supabase Auth Redirect

For local development, configure Supabase Auth redirect URLs to include:

```text
http://localhost:3000/auth/callback
```

The exact callback route may change depending on implementation.

See:

```text
docs/setup/environment-variables.md
```

[Back to top](#table-of-contents)

---

<a id="run-the-development-server"></a>

## 7. Run the Development Server

Start the app:

```bash
npm run dev
```

Open:

```text
http://localhost:3000
```

If port `3000` is already in use, Next.js may suggest another port.

[Back to top](#table-of-contents)

---

<a id="supabase-development-approach"></a>

## 8. Supabase Development Approach

### Initial Approach

Use a hosted Supabase development project.

This keeps setup simple and avoids requiring Docker or local Supabase during the earliest prototype.

### Later Optional Approach

Use Supabase CLI for local development.

Potential later commands:

```bash
supabase init
supabase start
supabase db reset
```

This requires Docker.

### Prototype Warning

The early hosted Supabase project should contain demo/test data only.

Before storing real customer evidence:

- create separate dev and production Supabase projects
- review RLS policies
- review backup/retention expectations
- verify storage bucket privacy

[Back to top](#table-of-contents)

---

<a id="running-checks"></a>

## 9. Running Checks

Run checks before committing:

```bash
npm run lint
npm run typecheck
npm test
npm run build
```

If a script does not exist yet, add it when the associated tooling is introduced.

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

## 10. Git Workflow

Recommended solo workflow:

```bash
git checkout -b feature/example-change
# make changes
npm run lint
npm run typecheck
npm test
npm run build
git status
git add .
git commit -m "Describe the change"
git push origin feature/example-change
```

Then open a pull request.

Even for solo development, pull requests are useful because they:

- trigger CI
- create review checkpoints
- document changes
- support future contributors

[Back to top](#table-of-contents)

---

<a id="optional-tools"></a>

## 11. Optional Tools

### GitHub CLI

Install with Homebrew:

```bash
brew install gh
```

Authenticate:

```bash
gh auth login
```

### Supabase CLI

Install later when needed:

```bash
brew install supabase/tap/supabase
```

### Homebrew

Homebrew is useful but not mandatory.

Install from:

```text
https://brew.sh
```

Do not make Homebrew a hard requirement unless necessary.

[Back to top](#table-of-contents)

---

<a id="troubleshooting"></a>

## 12. Troubleshooting

### Node Version Issues

Check Node:

```bash
node --version
```

Use Node LTS.

With `nvm`:

```bash
nvm install --lts
nvm use --lts
```

### Environment Variables Not Working

Confirm:

- `.env.local` exists
- variables are spelled correctly
- dev server was restarted after changes
- public variables use `NEXT_PUBLIC_`

### Supabase Auth Callback Fails

Check:

- Supabase redirect URL settings
- `NEXT_PUBLIC_APP_URL`
- local callback route
- browser console errors
- server logs

### Dependency Problems

Try:

```bash
rm -rf node_modules
npm install
```

Avoid deleting `package-lock.json` unless dependency resolution is broken and you understand the impact.

### Build Fails

Run:

```bash
npm run typecheck
npm run lint
npm run build
```

Fix the first error first.

[Back to top](#table-of-contents)

---

<a id="security-notes"></a>

## 13. Security Notes

Local development is not production.

Rules:

- Do not commit `.env.local`.
- Do not store production secrets locally unless necessary.
- Do not use real customer evidence in local development.
- Do not expose service role keys to browser code.
- Use demo/test data during prototype development.
- Keep dependencies updated.
- Review RLS policy behavior before storing sensitive data.

[Back to top](#table-of-contents)

---

<a id="related-documents"></a>

## 14. Related Documents

- [`docs/setup/codespaces.md`](./codespaces.md)
- [`docs/setup/environment-variables.md`](./environment-variables.md)
- [`docs/architecture/technical-stack.md`](../architecture/technical-stack.md)
- [`docs/security/secrets-management.md`](../security/secrets-management.md)
- [`docs/security/tenant-isolation.md`](../security/tenant-isolation.md)
- [`docs/decisions/ADR-0002-technical-stack.md`](../decisions/ADR-0002-technical-stack.md)

[Back to top](#table-of-contents)