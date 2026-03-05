# Contributing Guide

This project follows a pull-request-first workflow with a protected `main` branch.

## Branch Strategy

- `main`: stable, deployable branch. Direct pushes are not allowed.
- Create all work on short-lived branches from `main`.

Use this naming pattern:

- `feat/<short-description>`
- `fix/<short-description>`
- `chore/<short-description>`
- `docs/<short-description>`
- `refactor/<short-description>`
- `test/<short-description>`
- `ci/<short-description>`

Examples:

- `feat/add-auth-form`
- `fix/eslint-config-path`

## Commit Convention

Use Conventional Commits:

`<type>(optional-scope): <short summary>`

Common types:

- `feat`: new feature
- `fix`: bug fix
- `chore`: tooling or maintenance
- `docs`: documentation only
- `refactor`: code change without behavior change
- `test`: tests only
- `ci`: CI/CD configuration

Examples:

- `feat(auth): add login form validation`
- `fix(build): resolve TypeScript path alias`
- `ci(actions): run lint and build on pull request`

## Pull Request Workflow

- Open a PR from your branch into `main`.
- Keep PRs focused and reasonably small.
- Ensure `npm run lint` and `npm run build` pass before requesting review.
- Address all review comments and resolve conversations.
- Merge using **Squash and merge**.

## Keep Main Current

Before starting new work:

1. `git checkout main`
2. `git pull --ff-only`
3. `git checkout -b <new-branch-name>`
