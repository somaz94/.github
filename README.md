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
| [`.github/FUNDING.yml`](.github/FUNDING.yml) | Default Sponsor button target (inherited by all repos). |
| [`SECURITY.md`](SECURITY.md) | Default security policy and private vulnerability reporting path. |
| [`DCO.md`](DCO.md) | Developer Certificate of Origin — used instead of a CLA. |
| [`.github/PULL_REQUEST_TEMPLATE.md`](.github/PULL_REQUEST_TEMPLATE.md) | Default pull-request template. |
| [`.github/ISSUE_TEMPLATE/`](.github/ISSUE_TEMPLATE/) | Default bug-report / feature-request issue forms + chooser config. |
| [`.github/workflows/dco-reusable.yml`](.github/workflows/dco-reusable.yml) | Reusable DCO check, called by each repo. |
| [`.github/workflows/pr-welcome-reusable.yml`](.github/workflows/pr-welcome-reusable.yml) | Reusable pull-request greeting, called by each repo. |
| [`.github/workflows/ok-to-test-reusable.yml`](.github/workflows/ok-to-test-reusable.yml) | Reusable `/ok-to-test` command gate for CI on fork pull requests. |
| [`.github/workflows/issue-greeting-reusable.yml`](.github/workflows/issue-greeting-reusable.yml) | Reusable greeting comment posted when an issue is opened. |
| [`.github/workflows/stale-issues-reusable.yml`](.github/workflows/stale-issues-reusable.yml) | Reusable stale-issue sweeper (mark stale, then close after inactivity). |
| [`.github/workflows/dependabot-auto-merge-reusable.yml`](.github/workflows/dependabot-auto-merge-reusable.yml) | Reusable Dependabot auto-merge for minor/patch bumps. |
| [`.github/workflows/contributors-reusable.yml`](.github/workflows/contributors-reusable.yml) | Reusable `CONTRIBUTORS.md` generator. |
| [`.github/workflows/semantic-pr-reusable.yml`](.github/workflows/semantic-pr-reusable.yml) | Reusable Conventional Commits check for pull-request titles. |
| [`.github/workflows/labels-sync-reusable.yml`](.github/workflows/labels-sync-reusable.yml) | Reusable label sync — applies the canonical `.github/labels.yml` to each repo. |
| [`.github/workflows/lock-threads-reusable.yml`](.github/workflows/lock-threads-reusable.yml) | Reusable lock for long-closed issues and pull requests. |
| [`.github/workflows/pr-size-labeler-reusable.yml`](.github/workflows/pr-size-labeler-reusable.yml) | Reusable PR size labeler (`size/xs` .. `size/xl`). |
| [`.github/workflows/auto-assign-reusable.yml`](.github/workflows/auto-assign-reusable.yml) | Reusable auto-assign — sets the PR author as assignee. |
| [`.github/labels.yml`](.github/labels.yml) | Canonical label set synced into every repo by the label-sync workflow. |
| [`.github/workflows/gitlab-mirror.yml`](.github/workflows/gitlab-mirror.yml) | This repo's own one-way backup mirror to GitLab (not a reusable workflow). |

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

# The stub must grant the write scopes the reusable workflow needs, otherwise
# the call fails to start (a called workflow's permissions cannot exceed the
# caller's).
permissions:
  pull-requests: write
  issues: write

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

<br/>

## Enabling the issue greeting

Add this stub at `.github/workflows/issue-greeting.yml` in the target repository:

```yaml
name: Issue Greeting Bot
on:
  issues:
    types: [opened]

permissions:
  issues: write

jobs:
  greeting:
    uses: somaz94/.github/.github/workflows/issue-greeting-reusable.yml@main
```

Pass a custom body with `with: { message: "..." }` to override the default
greeting. Pull-request greetings are handled separately by the PR welcome
workflow above.

<br/>

## Enabling the stale-issue sweeper

Add this stub at `.github/workflows/stale-issues.yml` in the target repository.
The schedule/dispatch trigger lives in the stub; the reusable only exposes the
tuning knobs:

```yaml
name: Close Stale Issues
on:
  schedule:
    - cron: '0 0 * * *'
  workflow_dispatch:

permissions:
  issues: write
  pull-requests: write

jobs:
  stale:
    uses: somaz94/.github/.github/workflows/stale-issues-reusable.yml@main
```

Defaults: mark stale after 30 days of inactivity, close 7 days later, exempt
the `pinned` / `security` / `on-hold` labels. Override any of
`days-before-stale`, `days-before-close`, `stale-issue-message`,
`close-issue-message`, `stale-issue-label`, or `exempt-issue-labels` via
`with:`.

<br/>

## Enabling Dependabot auto-merge

Add this stub at `.github/workflows/dependabot-auto-merge.yml` in the target
repository:

```yaml
name: Dependabot auto-merge
on:
  pull_request:
    types: [opened, synchronize, reopened]

permissions:
  contents: write
  pull-requests: write

jobs:
  auto-merge:
    uses: somaz94/.github/.github/workflows/dependabot-auto-merge-reusable.yml@main
```

Only `version-update:semver-minor` and `semver-patch` bumps are auto-merged;
major bumps are left for a human. The default merge method is `squash` —
override it with `with: { merge-method: rebase }`. Repo-level auto-merge must
be enabled in the target repository's settings.

<br/>

## Enabling the CONTRIBUTORS.md generator

Add this stub at `.github/workflows/contributors.yml` in the target repository:

```yaml
name: Generate Contributors
on:
  workflow_dispatch:
  workflow_run:
    workflows: ["Generate changelog"]
    types: [completed]

permissions:
  contents: write

jobs:
  update_contributors:
    uses: somaz94/.github/.github/workflows/contributors-reusable.yml@main
    secrets: inherit
```

The stub MUST pass `secrets: inherit` — the checkout/commit steps push with
`PAT_TOKEN` (a personal access token with push access), which the reusable
receives as a named secret. Tune `output_file`, `format`, `columns`,
`exclude`, `commit_message`, or `branch` via `with:`.

<br/>

## Enabling the PR title check

Add this stub at `.github/workflows/semantic-pr.yml` in the target repository:

```yaml
name: Semantic PR
on:
  pull_request_target:
    types: [opened, edited, reopened, synchronize]

permissions:
  pull-requests: read

jobs:
  semantic-pr:
    uses: somaz94/.github/.github/workflows/semantic-pr-reusable.yml@main
```

The PR title is validated against Conventional Commits. Because a squash merge
turns the title into the commit subject, this keeps the merged history in
`feat:` / `fix:` / `docs:` form. The default type list is
`feat, fix, docs, refactor, test, ci, chore` — override it with
`with: { types: "feat\nfix\n..." }`, or require a scope with
`with: { require-scope: true }`.

<br/>

## Enabling label sync

Add this stub at `.github/workflows/labels.yml` in the target repository:

```yaml
name: Sync Labels
on:
  workflow_dispatch:
  schedule:
    - cron: '0 0 * * 1'   # weekly, Monday 00:00 UTC

permissions:
  issues: write

jobs:
  labels:
    uses: somaz94/.github/.github/workflows/labels-sync-reusable.yml@main
```

The canonical set lives in [`.github/labels.yml`](.github/labels.yml) of this
repo and is fetched at run time, so a rename or recolor there rolls out to
every repo on its next sync. `skip-delete` defaults to `true` (additive — the
sync creates/updates the canonical labels and leaves repo-specific labels
alone); flip it with `with: { skip-delete: false }` to prune anything not in
the file, or preview with `with: { dry-run: true }`. This is what backs the
`ok-to-test`, `stale`, `pinned`, `security`, and `on-hold` labels the other
workflows rely on.

<br/>

## Enabling the thread lock

Add this stub at `.github/workflows/lock-threads.yml` in the target repository:

```yaml
name: Lock Threads
on:
  schedule:
    - cron: '0 1 * * *'
  workflow_dispatch:

permissions:
  issues: write
  pull-requests: write

jobs:
  lock:
    uses: somaz94/.github/.github/workflows/lock-threads-reusable.yml@main
```

Long-closed issues and pull requests are locked so resolved threads stop
collecting drive-by comments — the natural sibling of the stale sweeper.
Defaults: lock after 365 days of inactivity, no comment. Override
`issue-inactive-days`, `pr-inactive-days`, `issue-comment`, `pr-comment`, or
`process-only` (`issues` / `prs`) via `with:`.

<br/>

## Enabling PR size labels

Add this stub at `.github/workflows/pr-size.yml` in the target repository:

```yaml
name: PR Size Labeler
on:
  pull_request:
    types: [opened, synchronize, reopened]

permissions:
  pull-requests: write

jobs:
  size:
    uses: somaz94/.github/.github/workflows/pr-size-labeler-reusable.yml@main
```

Each PR gets a `size/xs` .. `size/xl` label by changed-line count for quick
triage. Those labels live in [`.github/labels.yml`](.github/labels.yml), so
label sync gives them consistent colors. Tune the thresholds
(`xs-max-size`, `s-max-size`, `m-max-size`, `l-max-size`), require a split with
`with: { fail-if-xl: true }`, or exclude files with `with: { files-to-ignore: "package-lock.json" }`.

<br/>

## Enabling auto-assign

Add this stub at `.github/workflows/auto-assign.yml` in the target repository:

```yaml
name: Auto Assign
on:
  pull_request_target:
    types: [opened, reopened]

permissions:
  pull-requests: write

jobs:
  assign:
    uses: somaz94/.github/.github/workflows/auto-assign-reusable.yml@main
```

The pull-request author is set as the assignee so open PRs always show an
owner. Bot-authored PRs (Dependabot, Renovate, ...) are skipped.
