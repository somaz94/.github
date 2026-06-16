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
