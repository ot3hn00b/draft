# Draft - React + TypeScript + Vite

Project bootstrap command:

```bash
npm create vite@latest draft -- --template react-ts
```

Note: Vite was used to scaffold the project, and it is still used now by current scripts (`npm run dev`, `npm run build`, `npm run preview`).

## Setup Steps and Commands Used

### Step 1: Initialize Git and first commit

```bash
git init -b main
git add .
git commit -m "chore: initial project scaffold"
```

If Git identity is missing:

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

### Step 2: Connect local repo to GitHub

```bash
git branch -m main
git remote add origin https://github.com/<your-username>/<your-repo>.git
git push -u origin main
```

### Step 3: Protect `main` branch (GitHub UI)

Commands: none (configured in GitHub Settings).

Configured options:
- Require pull request before merging
- Require at least 1 approval
- Dismiss stale approvals
- Require conversation resolution
- Require linear history
- Block force pushes and deletions
- Squash merge as preferred strategy

### Step 4: Add collaboration standards

Files added:
- `CONTRIBUTING.md`
- `.github/pull_request_template.md`

Publish commands:

```bash
git add CONTRIBUTING.md .github/pull_request_template.md
git commit -m "docs(workflow): add contributing guide and PR template"
git push
```

### Step 5: Add CI workflow

File added:
- `.github/workflows/ci.yml`

Validation commands:

```bash
npm run lint
npm run build
```

Publish commands:

```bash
git add .github/workflows/ci.yml
git commit -m "ci(actions): add lint and build workflow"
git push
```

### Step 6: Add CD workflow (GitHub Pages)

File added:
- `.github/workflows/deploy-pages.yml`

Publish commands:

```bash
git add .github/workflows/deploy-pages.yml
git commit -m "ci(cd): deploy to github pages on main"
git push
```

GitHub Pages setting (UI):
- `Settings` -> `Pages` -> `Source: GitHub Actions`

### Step 7: Add release automation

File added:
- `.github/workflows/release.yml`

Publish commands:

```bash
git add .github/workflows/release.yml
git commit -m "ci(release): publish github release on version tags"
git push
```

### Step 8: Create first tagged release

```bash
git tag -a v0.1.0 -m "Release v0.1.0"
git push origin v0.1.0
```

### Step 9: Add security and dependency automation

Files added:
- `.github/dependabot.yml`
- `.github/workflows/codeql.yml`

Publish commands:

```bash
git add .github/dependabot.yml .github/workflows/codeql.yml
git commit -m "ci(security): add dependabot and codeql scanning"
git push
```

## Live App URL

- Pattern: `https://<github-username>.github.io/<repo-name>/`
- Current repo example: `https://ot3hn00b.github.io/draft/`

## Daily Development Flow

Branching policy for this repo:
- Keep only one working branch at a time.
- Fixed branch name: `work`.
- Open one PR from that branch to `main`, merge it, then delete the branch.

1. Start from updated `main` and create your single working branch:

```bash
git switch main
git pull --ff-only
git switch -c work
```

2. Develop and validate locally:

```bash
npm run lint
npm run build
```

3. Commit and push:

```bash
git add .
git commit -m "feat(scope): short summary"
git push -u origin work
```

4. Open a Pull Request to `main`, wait for CI checks, get review, then squash-merge.

## Commit Message Quick Guide

Use this format:

```bash
git commit -m "<type>(optional-scope): short summary"
```

Common types:
- `feat` new feature
- `fix` bug fix
- `docs` documentation only
- `chore` maintenance or config updates
- `ci` CI/CD workflow changes
- `refactor` code cleanup without behavior change
- `test` tests only

Examples:
- `chore(config): update eslint and prettier settings`
- `ci(actions): update workflow node version`
- `fix(score): prevent negative values`

## Common Branch Commands (Minimal)

```bash
# list branches
git branch -a

# create and switch to your single working branch
git switch -c work

# switch to your working branch
git switch work

# delete a local branch (safe, only if merged)
git branch -d work

# force delete a local branch (if not merged)
git branch -D work

# delete a remote branch
git push origin --delete work
```

After your PR is merged, clean up with:

```bash
git switch main
git pull --ff-only
git branch -d work
git push origin --delete work
```

## Quick Fix: "src refspec ... does not match any"

If you tried to push a branch name that does not exist locally:

```bash
git switch -c work
git push -u origin work
```

If you are already on `main` and want to push `main`:

```bash
git push origin main
```

## Quick Fix: "current branch has no upstream branch"

First push of branch `work`:

```bash
git push -u origin work
```

Optional (set upstream automatically for new branches):

```bash
git config --global push.autoSetupRemote true
```

## Quick Fix: "push rejected (fetch first)"

If remote moved ahead of your local branch:

```bash
git pull --rebase origin main
git push origin main
```

If rebase reports conflicts:

```bash
git status
# fix files, then:
git add <file>
git rebase --continue
# repeat until done, then:
git push origin main
```

If rebase does not stop, that means no conflicts. Just run the push command.

## How Changes Go From Local to GitHub Pages

1. You make changes locally on a feature branch.
2. You open and merge a PR into `main`.
3. Merge to `main` triggers:
   - `CI` workflow (lint + build),
   - then `Deploy to GitHub Pages`.
4. After deploy succeeds in Actions, the site updates at the Pages URL.

## Optional: Create a Versioned Release

```bash
git checkout main
git pull --ff-only
git tag -a v0.2.0 -m "Release v0.2.0"
git push origin v0.2.0
```

This triggers the `Release` workflow and publishes a GitHub Release with build artifact(s).
