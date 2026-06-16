# Contributing

Thanks for taking the time to contribute! These guidelines apply to all of
[somaz94](https://github.com/somaz94)'s open-source repositories unless a
specific repository overrides them with its own `CONTRIBUTING.md`.

<br/>

## Developer Certificate of Origin (DCO)

This project uses the **DCO** instead of a CLA. Every commit must carry a
`Signed-off-by` trailer certifying you wrote the change (or have the right to
submit it). Add it with the `-s` flag:

```bash
git commit -s -m "feat: add --dry-run flag"
```

A CI check rejects pull requests whose commits are not signed off. For the
full text and how to fix unsigned commits, see [DCO.md](DCO.md).

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
6. **Sign off every commit** (see above) and open the pull request.

<br/>

## Commit messages

Use [Conventional Commits](https://www.conventionalcommits.org/):
`feat:`, `fix:`, `docs:`, `refactor:`, `test:`, `ci:`, `chore:`. Keep the
subject line concise and in English.

<br/>

## Reporting security issues

Please do **not** open a public issue for security vulnerabilities. Report
them privately via the repository's Security tab (Report a vulnerability) or
by contacting the maintainer directly.
