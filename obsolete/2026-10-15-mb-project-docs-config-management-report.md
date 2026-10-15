---
title: "2026-10-15-mb-project-docs-config-management-report"
doc-id: "mb-f146e7f3"
updated: "2026-10-15"
---
# Report — mb-config-manager → mb-sysdoc-creator: mb-project-docs (2026-10-15)

## What the task was

The developer invoked `/mb-config-manager mb-project-docs`. The register showed the skill (skill, Active) with every value cell empty — the system's first examination. The examination produced two document findings and one system finding; the developer approved a full suite Refresh through mb-sysdoc-creator, corrected the skill's source mid-run, and authorized the drafts to be applied without a second gate provided they matched the approved change list ("Draft, apply if the drafts match"). The run ended DONE.

## Examination findings (mb-config-manager)

- **GAP** — `project-docs-learnings.md` did not exist; the folder held only three of the four required documents.
- **DEVIATION** — the three existing documents were out of the doc-suite specification's shape: config lacked the required identity fields and five-column settings tables; reference had no Authoritative sources and no Checks tables; explainer lacked the required "What has to happen" section.
- **UNHEALTHY (skill source)** — the project table in `mb-project-docs.md` @ `a15fe8d` was stale against the live filesystem: it named `mb4ecom`/`mb5pdlf` and `dev-mb4ecom`/`dev-mb5pdlf` where the real folders are `dev-mb-4-ecom`/`dev-mb-5-pdlf` with `mb-4-ecom-project-library`/`mb-5-pdlf-project-library`; the single-slug code repositories no longer exist — each project now holds `app1/app2/app3/app5` (the five-app model). Consequence: the skill stopped with "not a covered project" in two of four documentation repositories and matched no code repository at all.
- **No DOWN, BEHIND or DRIFT** at examination time: installed and source bodies were byte-identical at `a15fe8d` (2026-10-10), the source repository was clean and GitHub-backed ([[[mybizz-os-docs:os-reference-docs/repo-organization]]](C:/mybizz/mybizz-os-docs/os-reference-docs/repo-organization.md) line 33).
- Internal inconsistency: the explainer said "five projects" where every other source said four.

## Live verification performed

Installed copy and source copy read and diffed; helper script `agents-from-reference.sh` read and its recorded behaviour confirmed line by line; [[[mybizz-config-docs:mb-sysdoc-creator/doc-suite-specification]]](C:/mybizz/mybizz-config-docs/mb-sysdoc-creator/doc-suite-specification.md) and [[[mybizz-os-docs:os-reference-docs/repo-organization]]](C:/mybizz/mybizz-os-docs/os-reference-docs/repo-organization.md) confirmed present; mb-close-out's Step 3 (runs the skill to Step 6 inside the close-out gate, then Step 7) and the global `AGENTS.md` "Project documents" rule confirmed live; skills-catalog row confirmed OK through GBrain; all four project libraries and the app1 workspaces read; `theme\docs` found present but empty in `mb-3-cs-app1`; [[[mybizz-os-docs:os-reference-docs/repo-organization]]](C:/mybizz/mybizz-os-docs/os-reference-docs/repo-organization.md) § Names and § Commit hook and [[[default:specifications-global/spec-five-app-architecture-model]]](C:/dev/project-library-global/specifications-global/spec-five-app-architecture-model.md) read for the corrected table's grounding.

Two policy denials were encountered and honoured — no retry, no reshape: the register-row edit (cross-project write), and a bash `ls` naming protected paths (file tools used instead, per sysdoc learning L11).

## Developer decisions applied

Refresh approved; "five projects" → "four projects" directed; the skill source's project table corrected by the developer themselves (slugs `mb-3-cs`, `mb-4-ecom`, `mb-5-pdlf`, `pdlf`; code repository = App 1) and the skill reinstalled — new commit `6835cfe` (2026-10-15, "mb-project-docs project table; one naming rule"); the specification's naming rule updated to the neighbours convention; learnings L1 (naming) dropped and L2 rewritten without history; installed version taken from the new `git log -1`; the register row approved; two grants issued (`mb-project-docs` count 6, `mb-config-manager` count 2).

## Changes made

**1. `project-docs-config.md` — rewritten to specification shape.** New identity table (Issuer MyBizz; Installed version `6835cfe` 2026-10-15, no version field declared; standard taken from the source at that commit; last researched 2026-10-15). All settings tables converted to Setting | Issuer standard | MyBizz value | Source | Learning, across installed surface, form, project coverage, destination, files written per repository, dependencies and integration. Completion statuses retired to the reference (one fact, one home). Open items re-verified: two retired (commit-hook membership verified in [[[mybizz-os-docs:os-reference-docs/repo-organization]]](C:/mybizz/mybizz-os-docs/os-reference-docs/repo-organization.md); no-version-field became a settings row), the `theme\docs` cloud round-trip item kept, and the stale-source item closed by the developer's correction.

**2. `project-docs-reference.md` — rewritten.** Added the required Authoritative sources table (skill source @ `6835cfe`; [[[mybizz-config-docs:mb-sysdoc-creator/doc-suite-specification]]](C:/mybizz/mybizz-config-docs/mb-sysdoc-creator/doc-suite-specification.md) `mb-f4adba8c` 2026-10-15; [[[mybizz-os-docs:os-reference-docs/repo-organization]]](C:/mybizz/mybizz-os-docs/os-reference-docs/repo-organization.md) `mb-f38fd0aa` — all checked 2026-10-15) and the required Checks table: Installed version (`git log -1`, **developer-run** — bash naming `/mnt/c/mybizz` is policy-blocked for the agent, proved live), Two copies in sync, Helper script present, Project table matches repo-organization, Works — the rest agent-run. Typo "the skill's source" → "The skill's source". Every other section carried verbatim, verified against the source and the live script.

**3. `project-docs-explainer.md` — rewritten to the five required sections.** What it is / What it can do / Suitability / Why it works this way / What has to happen. The new closing section records: the developer approves once and applies the `AGENTS.md` change with the handed-over terminal line; documentation repositories commit through commit-all and code repositories by hand; the skill's source keeps step with `repo-organization`'s naming (a stale table makes the skill stop in a renamed folder); the commit hook keeps covering the documentation repositories. Both "five projects" lines corrected to four; the code-repository paragraph now names App 1, the development workspace.

**4. `project-docs-learnings.md` — created** (was the GAP), named per the specification's new neighbours rule beside `project-docs-config.md`. Frontmatter carries `title` only — `doc-id` and `updated` are never written by hand; the commit hook assigns them. Single entry, the developer's wording verbatim:

> L1 — The covered slugs are `mb-3-cs`, `mb-4-ecom`, `mb-5-pdlf` and `pdlf`, as in `repo-organization.md`; a document spelling `mb4ecom` or `mb5pdlf` is out of date.

**5. Register row** in `config-manager-register.md`: Installed version `6835cfe`, Last updated 2026-10-15, Last checked 2026-10-15, State Active.

The refreshed suite lives in `/mnt/c/mybizz/mybizz-config-docs/mb-project-docs/`: [[[mybizz-config-docs:mb-project-docs/project-docs-config|config]]](C:/mybizz/mybizz-config-docs/mb-project-docs/project-docs-config.md) · [[[mybizz-config-docs:mb-project-docs/project-docs-reference|reference]]](C:/mybizz/mybizz-config-docs/mb-project-docs/project-docs-reference.md) · [[[mybizz-config-docs:mb-project-docs/project-docs-explainer|explainer]]](C:/mybizz/mybizz-config-docs/mb-project-docs/project-docs-explainer.md) · [[[mybizz-config-docs:mb-project-docs/project-docs-learnings|learnings]]](C:/mybizz/mybizz-config-docs/mb-project-docs/project-docs-learnings.md).

Verification evidence: source and installed bodies diffed identical at `6835cfe`; the new project table confirmed live in both copies; specification naming rule confirmed at line 25; `git log -1` returned `6835cfe 2026-10-15 mb-project-docs project table; one naming rule`.

## Cross-system check results

- **Cupcake:** no component-specific rule for mb-project-docs — none found. The run's two denials came from standing general rules (`no_cross_project_filesystem_access_by_default`, `no_bash_mediated_file_writes_without_grant`); enforcement state phrased per [[[mybizz-config-docs:cupcake/cupcake-config]]](C:/mybizz/mybizz-config-docs/cupcake/cupcake-config.md)'s Current Operational Status.
- **security-global:** `sec-local-filesystem-access-boundary.md` present at the cited path; its Verification Requirement ("attempt an operation outside the boundary; confirm it is blocked") was satisfied live by the run's two denials. Others: none found.
- **Memory governor:** none found — these suite documents are not governed facts; they reach GBrain through commit-all's sync.
- **Related systems:** mb-close-out Step 3 verified live; skills-catalog row OK; [[[mybizz-os-docs:os-reference-docs/system-integrity-explainer]]](C:/mybizz/mybizz-os-docs/os-reference-docs/system-integrity-explainer.md) § 9 already prescribed exactly this remediation (add the learnings document; bring the suite into the new shape).

## Not done (boundaries)

No edit to the skill's source or installed copy by the agent (the developer corrected and reinstalled both); no commit; GBrain used read-only (two searches, varied terms); the `mybizz-scope-list.md` staleness (lines 78–83 still name `dev-mb4ecom` and `dev-mb-3-cs\mb-3-cs`) flagged only — mybizz-os-docs material for a separate run.

## Still open

1. The developer runs `bash /mnt/c/mybizz/scripts/commit-all.sh "mb-project-docs: suite refreshed to doc-suite specification; source table corrected"` — the commit hook assigns `doc-id`/`updated` to the new learnings file, converts the short-form links and pushes the suite into GBrain.
2. Optional hygiene: `mg-revoke` both grant paths (partially consumed — 4 of 6 and 1 of 2 uses used; revoking clears the remainders).
3. The config's one open item stands: whether Anvil accepts and keeps `theme\docs` through its cloud round-trip (local persistence evidenced in `mb-3-cs-app1`; the cloud side unverified).
4. Flagged for a future run: `mybizz-scope-list.md` lines 78–83 still name the old repository paths.
