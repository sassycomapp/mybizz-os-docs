---
title: "2026-10-15-mb-doc-check-config-management-report"
doc-id: "mb-cf0fa723"
updated: "2026-10-15"
---
# Report — mb-config-manager → mb-sysdoc-creator: mb-doc-check (2026-10-15)

## What the task was

The developer invoked `/mb-config-manager mb-doc-check`. The register row existed (script, Active) but every version cell was empty — this was the system's first examination. It produced five findings, all approved as one full suite Refresh through mb-sysdoc-creator, ending DONE.

## Examination findings (mb-config-manager)

All verified live against the installed program, its settings file, GBrain and the issuer's own source:

- **DRIFT** — recorded byte size 88,758 vs live 89,032 (commit `c344d51`, 2026-10-13).
- **DRIFT** — covered-repositories table carried wrong source ids (`mb4ecom-project-library`, `mb5pdlf-project-library`) and was missing `mb-close-out`; `sources-check` said "everything matches" for all 17 sources in `mb-doc-check.sources`.
- **DEVIATION** — config not in the doc-suite specification's shape: no Issuer/Installed version/Standard taken from/Last researched identity rows; settings tables lacked the Issuer standard | MyBizz value | Source | Learning columns.
- **DEVIATION** — reference lacked the required Authoritative sources and Checks tables.
- **GAP** — `doc-check-learnings.md` did not exist (folder held three of the four documents).

No DOWN, UNHEALTHY or BEHIND — MyBizz is the issuer; the standard is the program's own source at commit `c344d51`. Borderline explainer issues (section 2 title, extra S.W.O.T. section) folded into the same Refresh by developer instruction.

## Live verification evidence gathered before any change

- `sources-check`: everything matches (all 17 sources, both directions).
- `slug-selftest`: 1,404 paths, 0 differences (recorded baseline was 1,292 paths on 2026-10-01).
- `check --root /mnt/c/mybizz/mybizz-os-docs`: all clear.
- `run-tests.sh`: ends with `OK`.
- Old open item 2 re-verified still open: the built-in help text still shows a relative-path example (`relative/path/File%20Name.md`, source lines 6 and 83).

## Cross-system check

Cupcake: no rule specific to mb-doc-check; enforcement active everywhere (standing policy). Memory governor: no treatment of mb-doc-check; its folder correctly not covered by the hook. security-global: no mention found. Related: the script's source repository is `mybizz-scripts` (`C:\mybizz\scripts`), listed in [[[mybizz-os-docs:os-reference-docs/repo-organization]]](C:/mybizz/mybizz-os-docs/os-reference-docs/repo-organization.md). The remediation this run executed was already recorded in the system-integrity explainer's remediation notes ("Add `doc-check-learnings.md` to its folder; bring its suite into the new shape through mb-sysdoc-creator's Refresh").

## Developer decisions

- All five findings approved as one `/mb-sysdoc-creator mb-doc-check` Refresh, explainer issues folded in.
- Register row: Installed version `c344d51`, Last updated 2026-10-13, Last checked 2026-10-15.
- Learning L1 corrected to: "The ids are GBrain's data, not MyBizz's; sources-check and repo-organization.md are the two sources to copy from." Source: Developer-approved lesson.
- Pre-authorized application: drafts matching the approved change list were applied without a second approval gate.

## Changes made

1. **`mb-doc-check-config.md` — rewritten to specification shape.** New Identity table (Issuer MyBizz, with GBrain as issuer for the page-name rule and source-id list; Installed version `c344d51`; Standard taken from `mb-doc-check.py` and `mb-doc-check.sources` at that commit; Last researched 2026-10-15). Settings re-formed into three 5-column tables: program (name, location with corrected 89,032 bytes, settings file, how it runs, failure behavior, network/GBrain, tests), covered repositories (all 17 rows, corrected ids, `mb-close-out` added), link and storage (`@paths`, `@eol`, stored link form, page name, frontmatter, environment variables). Open item 1 dropped as closed; open item 2 kept, now verified. All other facts carried, nothing retired silently.
2. **`mb-doc-check-reference.md`** — Authoritative sources table (program at `c344d51`, settings file, repo-organization, commit-all procedure) and Checks table (Installed version, Settings file, Covered repositories vs GBrain, Page-name rule vs GBrain, Works — all agent-run) added in specification position; every command, message, procedure and the Do-not list carried.
3. **`mb-doc-check-explainer.md`** — section 2 renamed "What it can do", section 4 renamed "Why it works this way"; the S.W.O.T. section removed (strengths/weaknesses duplicated sections 3–4, threats hypothetical). Sections now exactly 1–5 in the specification's order.
4. **`mb-doc-check-learnings.md` — created.** L1 (developer-corrected wording) and L2 (no release system; version = source commit plus file size). The folder now holds exactly the four documents.
5. **Register row updated** in [[[mybizz-config-docs:mb-config-manager/config-manager-register]]](C:/mybizz/mybizz-config-docs/mb-config-manager/config-manager-register.md): `mb-doc-check · script · c344d51 · 2026-10-13 · 2026-10-15 · Active`.

`updated` dates were left untouched for the hook to move at commit, per the linking rule; links were written in short form for the hook to store.

## Blocks and denials encountered

- **Cupcake text-match denial (examination):** a shell command naming the `cupcake` folder was denied on the token itself; the same cross-system check was completed through the Read/Grep tools, the documented alternative (sysdoc-creator learning L11). Never retried, never disguised.
- **Grant-depletion anomaly (application):** the count-6 grant for `/mnt/c/mybizz/mybizz-config-docs/mb-doc-check` produced 5 successful writes, then denied the S.W.O.T. removal and the learnings creation with "requires a separate, explicit developer permission slip for this occasion" — on the same path where neighbouring writes succeeded. Not retried (a denial is final). A count-3 re-grant completed both writes: 2 used, evidence consistent with Cupcake's open item 3 (a grant unit consumed without a successful write). Fresh live evidence now exists for that open item.

## Grants

- `/mnt/c/mybizz/mybizz-config-docs/mb-doc-check` — count 6, then count 3: fully consumed by the approved writes and the denials.
- `/mnt/c/mybizz/mybizz-config-docs/mb-config-manager` — count 1; 1 write used (register row). **Open — revoke with `mg-revoke --path /mnt/c/mybizz/mybizz-config-docs/mb-config-manager`.**

## Open items left

- `doc-check-config.md` open item 1: the built-in help text still shows a relative-path example.
- Optional follow-up offered, undecided: record this run's grant-decrement evidence in the Cupcake config's open items.

## Next steps

- Developer: revoke the open grant above, then commit all repositories with `bash /mnt/c/mybizz/scripts/commit-all.sh "mb-doc-check: refresh suite to doc-suite-specification; register row set"` — commit-all runs the hook, which completes the new learnings document's frontmatter and moves the `updated` dates.
