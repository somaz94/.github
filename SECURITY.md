# Security Policy

This policy applies to all of [somaz94](https://github.com/somaz94)'s
open-source repositories unless a specific repository provides its own
`SECURITY.md`.

<br/>

## Reporting a Vulnerability

Please do **not** open a public issue for security vulnerabilities. Report
them privately so a fix can be prepared before the issue becomes public.

### Preferred channel: GitHub Security Advisories

Open the **Security** tab of the affected repository and use the
**"Report a vulnerability"** button (e.g.
`https://github.com/somaz94/<repo>/security/advisories/new`). This creates a
private advisory visible only to the maintainer.

### Alternative channel: email

If the repository does not have private advisories enabled, send a private
email to **genius5711@gmail.com** with subject prefixed `[security]`. Include:

- Affected repository, component, and version
- Steps to reproduce
- Impact assessment (what an attacker could achieve)
- Any suggested mitigation

<br/>

## Response expectations

- **Acknowledgement**: within 7 days of report
- **Initial assessment**: within 14 days
- **Fix release**: timeline depends on severity, typically within 30 days for
  high/critical issues

<br/>

## Disclosure

Once a fix is released, the advisory is published on the repository's GitHub
Security Advisories page so downstream users can upgrade.

<br/>

## Scope

This policy covers vulnerabilities in the source code, build, and release
workflows of repositories under this account. Vulnerabilities in upstream
projects that a repository merely references should be reported to those
upstream maintainers.
