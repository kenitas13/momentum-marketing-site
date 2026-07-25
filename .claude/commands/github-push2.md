---
description: Security-scan, then push code to GitHub with an up-to-date README, repo About section, and GitHub Actions-based Pages deployment.
argument-hint: [optional commit message]
---

Run the following steps in order. This repo is a static site (currently `index.html`, `README.md`, `CLAUDE.md`) deployed to GitHub Pages at `https://<owner>.github.io/<repo>/`. On this machine, `git`/`gh` may not be on PATH in a fresh PowerShell session — if a command reports `git`/`gh` not recognized, refresh PATH first with:

```powershell
$env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine") + ";" + [System.Environment]::GetEnvironmentVariable("Path","User")
```

## 1. Security scan (do this before touching git)

Scan all new/modified files (`git status`, `git diff`) for anything that shouldn't be published:

- Secret-shaped strings: AWS keys (`AKIA[0-9A-Z]{16}`), private key headers (`-----BEGIN ... PRIVATE KEY-----`), generic `api_key`/`secret`/`token`/`password` assignments followed by a long literal value.
- Files that shouldn't be tracked: `.env*`, `*.pem`, `*.key`, credential/service-account JSON, `.aws/`, `id_rsa*`.
- Do **not** flag the Formspree endpoint's form ID (`FORMSPREE_ENDPOINT` in `index.html`) — Formspree form IDs are meant to be public client-side and are not secrets.

If anything genuinely sensitive is found: **stop, do not commit or push**, and report exactly what was found and where so the user can remove/rotate it first. Otherwise, continue.

## 2. Update README.md

Read the current `README.md` and the current state of the site/repo. Update it (don't just append) to accurately reflect: what the project is, the live Pages URL, file/structure overview, any local-run instructions, and any configuration steps (e.g. the Formspree placeholder). Keep it concise.

## 3. Commit and push

```powershell
git add -A
git commit -m "<descriptive message summarizing the actual changes>"
git push
```

Use `$ARGUMENTS` as the commit message if the user supplied one; otherwise write one based on `git diff --stat`/`git status`. If `git push` fails with a credential/terminal-prompt error, run `gh auth setup-git` then retry.

## 4. GitHub Pages via GitHub Actions

Check whether `.github/workflows/` already has a Pages deployment workflow. If not, create `.github/workflows/deploy-pages.yml`:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: ["main"]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Setup Pages
        uses: actions/configure-pages@v5
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: "."
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

Commit and push this workflow file (it can go in the same commit as step 3, or its own commit — either is fine).

Then make sure the repo's Pages source is set to "GitHub Actions" (not the legacy branch deploy):

```powershell
gh api -X PUT repos/<owner>/<repo>/pages -f build_type=workflow
```

(If the repo has no Pages site yet, use `-X POST` instead of `-X PUT`.)

Watch the workflow run to confirm it deploys successfully:

```powershell
gh run list --workflow=deploy-pages.yml --limit 1
gh run watch <run-id>
```

## 5. Update the repo's About section

```powershell
gh repo edit <owner>/<repo> `
  --description "<accurate one-line description>" `
  --homepage "https://<owner>.github.io/<repo>/" `
  --add-topic <relevant-topic> --add-topic <relevant-topic> ...
```

Reuse the existing topics/description if they're still accurate; update them if the project has changed. The `--homepage` flag is the "add the GitHub Pages link to About" step — always set it to the live Pages URL.

## 6. Report back

Summarize concisely: what was committed/pushed, whether the security scan found anything (and what you did about it), the Pages deployment status, and the live URL.
