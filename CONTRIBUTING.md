# Contributing

Thanks for taking the time to contribute! These guidelines apply to all of
[somaz94](https://github.com/somaz94)'s open-source repositories unless a
specific repository overrides them with its own `CONTRIBUTING.md`.

<br/>

## Workflow

1. **Open an issue first** for anything beyond a trivial fix, so the approach
   can be agreed before you spend time on it.
2. **Fork** the repository and create a topic branch off `main`.
3. **Keep the change focused** — one logical change per pull request.
4. **Match the existing style** of the repository (linters, formatters, and
   tests configured in CI are the source of truth).
5. **Add or update tests** when you change behavior, and update the README or
   docs if the user-facing surface changes.
6. **Open the pull request** once tests pass.

<br/>

## Commit messages and PR title

Use [Conventional Commits](https://www.conventionalcommits.org/):
`feat:`, `fix:`, `docs:`, `refactor:`, `test:`, `ci:`, `chore:`. Keep the
subject line concise and in English.

The **pull-request title** must follow the same convention — a CI check
rejects titles that do not. Because these repositories squash-merge, the PR
title becomes the merged commit's subject, so it needs to be a valid
Conventional Commit on its own (e.g. `fix: handle empty parentRefs`).

<br/>

## Reporting security issues

Please do **not** open a public issue for security vulnerabilities. Report
them privately via the repository's Security tab (Report a vulnerability) or
by contacting the maintainer directly.
