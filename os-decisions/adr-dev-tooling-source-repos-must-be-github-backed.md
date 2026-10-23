---
title: "adr-dev-tooling-source-repos-must-be-github-backed"
doc-id: "mb-6b7f097f"
updated: "2026-10-23"
---
# Every MyBizz Repository Is GitHub-Backed

**Status:** Accepted

---

## Context

Every MyBizz repository, for documents, tools or application code, lives on David's computer. A repository that exists only there has no off-machine copy, and a repository GBrain indexes but that is never pushed lets uncommitted work sit unseen: GBrain's `sync_freshness` check fails on it, correctly, because the work is neither committed nor backed up.

---

## Decision

**Every MyBizz repository has a GitHub remote under the `sassycomapp` account, set up when the repository is created.** A local-only working tree is never an acceptable state.

| Repository function | GitHub repository type |
|---|---|
| A live tool, document set or application, developed in place | A normal repository |
| A scaffold copied to start new projects (`project-template`) | A normal repository that is only ever copied |

Every repository is public, so that Claude can read it.

Each repository's work is committed and pushed by the developer with `commit-all`, as set out in [[[mybizz-os-docs:operating-procedures/commit-all-repositories|Commit All Repositories]]](C:/mybizz/mybizz-os-docs/operating-procedures/commit-all-repositories.md). Every repository is listed in [[[mybizz-os-docs:os-reference-docs/repo-organization|Repo Organization]]](C:/mybizz/mybizz-os-docs/os-reference-docs/repo-organization.md).

---

## Why

- **Simple.** One workflow everywhere: commit, push, sync. No list of exceptions to remember.
- **Secure.** Every push is an off-machine copy. The loss of the computer loses nothing that was pushed.
- **Robust.** A `sync_freshness` failure on a GBrain source always means real uncommitted work, and its fix is always the same: commit and push.
- **Fit for purpose.** Preserving the source of truth is a requirement for the tools MyBizz depends on, not a footnote.

---

## Setting Up a New Repository

At creation, in WSL:

```bash
cd <new-repo-local-path>
git init
git add -A
git commit -m "Initial commit"
gh repo create sassycomapp/<repo-name> --public --source=. --remote=origin --push
```

Then add it to `repo-organization.md` and, for a documentation repository, to `commit-all.sh` and `mb-doc-check.sources`.

---

## Finding a Local-Only Repository

In the repository's folder, `git remote -v` with empty output means no remote. Commit any pending work, create the GitHub repository as above, and run `gbrain sync --source <id>` to confirm `sync_freshness` clears.
