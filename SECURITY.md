# Security Policy

## Securing GitHub Pages Deployments

This repository deploys a static site to GitHub Pages via GitHub Actions.
The following measures are already in place or recommended.

### Already implemented in the workflows

- **Least-privilege `GITHUB_TOKEN`**  
  Workflow-level permission is `contents: read` only. Write scopes (`pages: write`, `id-token: write`) are granted only to the jobs that need them.

- **OIDC verification**  
  `id-token: write` enables GitHub’s OpenID Connect token so the Pages service can verify the deployment originated from this repository and workflow.

- **Environment binding**  
  The deploy job targets the `github-pages` environment. Protection rules configured on that environment are enforced before the deploy step runs.

- **Branch restriction**  
  Automatic deploys only run on pushes to `main`.

- **Concurrency control**  
  Only one deployment runs at a time; in-progress production deploys are not cancelled.

- **No credentials left on runner**  
  `persist-credentials: false` on checkout.

### OIDC & deployment security checklist

Use this checklist to verify the full protection stack:

- [x] Request `id-token: write` only on privileged jobs (not on PR / untrusted jobs)
- [x] Bind production deploys to a GitHub Environment (`github-pages`)
- [x] Restrict automatic deploys to the `main` branch only
- [x] Workflow-level permissions limited to `contents: read`
- [ ] **Required reviewers** enabled on the `github-pages` environment
- [ ] Deployment branches on `github-pages` limited to `main` only
- [ ] Branch protection on `main` (PR required + at least 1 approval)
- [ ] Required status checks (CI) must pass before merge to `main`
- [ ] Pages source set to **GitHub Actions** (not “Deploy from a branch”)
- [ ] Prefer immutable IDs in any external cloud OIDC trust policies
- [ ] Pin third-party Actions to full commit SHAs where practical

### Recommended one-time configuration (Settings UI)

These cannot be set via files; configure them in the GitHub web UI.

#### 1. Protect the `github-pages` environment (includes Required reviewers)

1. Go to **Settings → Environments**
2. Open (or create) the environment named **`github-pages`**
3. Enable **Deployment protection rules**:
   - **Required reviewers** — add yourself (or a trusted collaborator).  
     This forces a **manual approval before every deploy**. Without this step, the OIDC token alone is not enough to stop an unauthorized push to `main` from going live.
   - **Wait timer** (optional) — e.g. 5 minutes
   - **Deployment branches and tags** — restrict to **Selected branches** → only `main`

> **Why required reviewers matter with OIDC**  
> The OIDC token proves *which* workflow and environment ran. Required reviewers add a human gate so that even a valid token from `main` cannot publish until an authorized person approves the deployment.

#### 2. Protect the `main` branch

1. Go to **Settings → Branches → Add branch protection rule**
2. Branch name pattern: `main`
3. Enable:
   - Require a pull request before merging
   - Require approvals (at least 1)
   - Dismiss stale pull request approvals when new commits are pushed
   - Require status checks to pass (select the CI workflow)
   - Do not allow bypassing the above settings
   - Restrict who can push to matching branches (optional but recommended)

#### 3. Confirm Pages source

**Settings → Pages → Build and deployment → Source** must be set to **GitHub Actions** (not “Deploy from a branch”).

### Site-level security headers

The `index.html` includes basic browser security meta tags (Referrer-Policy, Content-Security-Policy, etc.).
GitHub Pages always serves over HTTPS and sets a strict HSTS policy.

### Reporting a vulnerability

If you discover a security issue in this project, please open a private security advisory on the repository or contact the maintainer.

Thank you for helping keep the project and its users safe.
