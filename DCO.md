# Developer Certificate of Origin

Contributions to this project are accepted under the
[Developer Certificate of Origin (DCO) 1.1](https://developercertificate.org/).

By adding a `Signed-off-by` line to your commits, you certify the statement
below. This is a lightweight, no-paperwork alternative to a Contributor
License Agreement: there is nothing to sign, store, or register — you only
add one trailer line per commit.

<br/>

## How to sign off

Add the trailer automatically when you commit:

```bash
git commit -s -m "fix: correct retry backoff"
```

This appends a line using your configured `user.name` / `user.email`:

```
Signed-off-by: Jane Doe <jane@example.com>
```

Forgot to sign earlier commits in a branch? Re-apply the trailer across the
whole branch and force-push:

```bash
git rebase --signoff origin/main
git push --force-with-lease
```

Use a real name and a reachable email — they must match the commit author.

<br/>

## The certificate

```
Developer Certificate of Origin
Version 1.1

Copyright (C) 2004, 2006 The Linux Foundation and its contributors.

Everyone is permitted to copy and distribute verbatim copies of this
license document, but changing it is not allowed.


Developer's Certificate of Origin 1.1

By making a contribution to this project, I certify that:

(a) The contribution was created in whole or in part by me and I
    have the right to submit it under the open source license
    indicated in the file; or

(b) The contribution is based upon previous work that, to the best
    of my knowledge, is covered under an appropriate open source
    license and I have the right under that license to submit that
    work with modifications, whether created in whole or in part
    by me, under the same open source license (unless I am
    permitted to submit under a different license), as indicated
    in the file; or

(c) The contribution was provided directly to me by some other
    person who certified (a), (b) or (c) and I have not modified
    it.

(d) I understand and agree that this project and the contribution
    are public and that a record of the contribution (including all
    personal information I submit with it, including my sign-off) is
    maintained indefinitely and may be redistributed consistent with
    this project or the open source license(s) involved.
```
