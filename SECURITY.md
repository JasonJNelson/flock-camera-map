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

### Recommended one-time configuration (Settings UI)

These cannot be set via files; configure them in the GitHub web UI.

#### 1. Protect the `github-pages` environment

1. Go to **Settings → Environments**
2. Open (or create) the environment named **`github-pages`**
3. Enable **Deployment protection rules**:
   - **Required reviewers** — add yourself (or a trusted collaborator). This forces a manual approval before every deploy.
   - **Wait timer** (optional) — e.g. 5 minutes
   - **Deployment branches and tags** — restrict to **Selected branches** → only `main`

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

The `index.html` includes basic browser security meta tags (Referrer-Policy, etc.).
GitHub Pages always serves over HTTPS and sets a strict HSTS policy.

### Reporting a vulnerability

If you discover a security issue in this project, please open a private security advisory on the repository or contact the maintainer.

Thank you for helping keep the project and its users safe.
