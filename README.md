# .github

Community health files and shared CI for [somaz94](https://github.com/somaz94)'s
open-source repositories. Files here are inherited by any repository in the
account that does not provide its own copy.

<br/>

## Contents

| File | Purpose |
|---|---|
| [`CONTRIBUTING.md`](CONTRIBUTING.md) | Default contribution guide (inherited by all repos). |
| [`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md) | Contributor Covenant 2.1 — default code of conduct. |
| [`SECURITY.md`](SECURITY.md) | Default security policy and private vulnerability reporting path. |
| [`DCO.md`](DCO.md) | Developer Certificate of Origin — used instead of a CLA. |
| [`.github/PULL_REQUEST_TEMPLATE.md`](.github/PULL_REQUEST_TEMPLATE.md) | Default pull-request template. |
| [`.github/ISSUE_TEMPLATE/`](.github/ISSUE_TEMPLATE/) | Default bug-report / feature-request issue forms + chooser config. |
| [`.github/workflows/dco-reusable.yml`](.github/workflows/dco-reusable.yml) | Reusable DCO check, called by each repo. |
| [`.github/workflows/pr-welcome-reusable.yml`](.github/workflows/pr-welcome-reusable.yml) | Reusable pull-request greeting, called by each repo. |
| [`.github/workflows/ok-to-test-reusable.yml`](.github/workflows/ok-to-test-reusable.yml) | Reusable `/ok-to-test` command gate for CI on fork pull requests. |

<br/>

## Enabling the DCO check in a repository

Add this stub at `.github/workflows/dco.yml` in the target repository:

```yaml
name: DCO
on:
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  dco:
    uses: somaz94/.github/.github/workflows/dco-reusable.yml@main
```

The shared logic lives here, so the check is maintained in one place.

<br/>

## Enabling the PR welcome greeting

Add this stub at `.github/workflows/pr-welcome.yml` in the target repository:

```yaml
name: PR Welcome
on:
  pull_request_target:
    types: [opened]

permissions:
  pull-requests: write

jobs:
  welcome:
    uses: somaz94/.github/.github/workflows/pr-welcome-reusable.yml@main
```

Pass a custom message with `with: { message: "..." }` to override the default
greeting.

<br/>

## Enabling the `/ok-to-test` command

Add this stub at `.github/workflows/ok-to-test.yml` in the target repository:

```yaml
name: ok-to-test
on:
  issue_comment:
    types: [created]

jobs:
  ok-to-test:
    uses: somaz94/.github/.github/workflows/ok-to-test-reusable.yml@main
```

A maintainer comments `/ok-to-test` on a fork pull request; the workflow
verifies the commenter has write access and applies the `ok-to-test` label.
Gate the CI jobs that need secrets (or that are expensive) on that label so
they run only for same-repo branches or after approval:

```yaml
on:
  pull_request:
    types: [opened, synchronize, reopened, labeled]

jobs:
  e2e:
    if: >
      github.event.pull_request.head.repo.full_name == github.repository ||
      contains(github.event.pull_request.labels.*.name, 'ok-to-test')
    runs-on: ubuntu-latest
    # ...
```
