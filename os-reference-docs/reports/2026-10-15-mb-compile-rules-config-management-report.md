---
title: "2026-10-15-mb-compile-rules-config-management-report"
doc-id: "mb-dd9b8deb"
updated: "2026-10-15"
---
# Report — mb-config-manager → mb-sysdoc-creator: mb-compile-rules (2026-10-15)

## What the task was

The developer invoked `/mb-config-manager mb-compile-rules`. The register showed the skill (skill, Active) with every value cell empty — the system's first examination. The examination produced two drift findings and one shape deviation; the developer added an UNHEALTHY finding, approved a full suite Refresh through mb-sysdoc-creator with the findings folded in, corrected the skill's source mid-run, and steered two approval gates (change list, then drafts). The run ended DONE.

## Examination findings (mb-config-manager)

- **DRIFT** — the config's rule-estate row said "19 `.md` files, `cup-` prefix"; the live folder `rules-cupcake-global/` holds 20 `.md` files, of which only 2 carry the prefix. Resolution: the estate is not this suite's record at all — system integrity assigns the rules to Cupcake (see below), so the rows were retired to a pointer.
- **DRIFT** — the config and explainer claimed two skill copies (installed + mirror in the suite folder, "no sync mechanism"); the folder held exactly the four documents — the mirror had been deleted the same day by commit `12ffc6d` ("Install mb-config-manager agent and remove exam machinery"). The live state matches the specification; the documents were stale.
- **DEVIATION** — all four documents were out of [[[mybizz-config-docs:mb-sysdoc-creator/doc-suite-specification]]](C:/mybizz/mybizz-config-docs/mb-sysdoc-creator/doc-suite-specification.md)'s shape: config lacked the identity fields and five-column settings tables and carried history; reference had no Authoritative sources and no Checks tables; explainer lacked "What has to happen"; learnings had no table at all. Correction: one full Refresh, every other change folded in.
- **UNHEALTHY** (developer-added) — the skill then required sources named `cup-(rule-name).md`, but 18 of the 20 sources carry no prefix, so Prerequisite 1 would stop on them. Correct pattern: sources keep their own names; compiled files are `cup-{source name}.rego`, never a doubled prefix.
- No DOWN and no BEHIND: the issuer is MyBizz itself; the system was installed and invocable.

## Live verification performed

Installed copy and canonical source read and diffed against both source commits (`1fbdbe6` 2026-10-04, `2e2ac13` 2026-10-15); the docs folder's git history pulled for the mirror deletion; the estate folder listed live (20 files); all four documents and the register read in full; developer-run terminal output received for the live policy folder (`builtins/`: 23 `.rego` — 19 rules compiled from sources, 3 grants-data, 1 control-plane rule — plus never-scanned backups and a `system` directory) and for the binaries (`cupcake`, `opa` in `~/.cupcake/bin/`); two GBrain searches (varied terms) surfaced the canonical source's corrected text, the skills-catalog row, [[[mybizz-os-docs:os-reference-docs/system-integrity-explainer|System Integrity Explainer]]](C:/mybizz/mybizz-os-docs/os-reference-docs/system-integrity-explainer.md) § 6 and the current [[[mybizz-config-docs:cupcake/cupcake-config]]](C:/mybizz/mybizz-config-docs/cupcake/cupcake-config.md) estate rows.

**Mid-run discovery:** the developer's naming correction landed while the run was open — commit `2e2ac13` "mb-compile-rules: correct rule-file naming" (2026-10-15 06:39, tree clean) — and the installed copy verified body-identical to it, frontmatter transform aside. The UNHEALTHY defect therefore resolved during the run; the register record moved from the developer's initially specified `1fbdbe6`/2026-10-04 to the live `2e2ac13`/2026-10-15 by their explicit decision (option B).

## Developer decisions applied

Refresh approved folding findings 1 and 2 into the shape correction; UNHEALTHY added with the source correction made by the developer themselves; register option B; change-list corrections: the reference's authoritative procedure is the canonical source `C:\mybizz\mb-compile-rules\mb-compile-rules.md` with the installed copy required to stay body-identical, and citations name [[[mybizz-os-docs:os-reference-docs/system-integrity-explainer|System Integrity Explainer]]](C:/mybizz/mybizz-os-docs/os-reference-docs/system-integrity-explainer.md) § 6, not the v5 file; draft approval with one wording correction to learnings L1 ("Cupcake owns its rules; a second record of them here drifts." / Source "Developer decision"). Three grants issued: suite folder (4), config-manager folder (2), reports folder (1).

## Changes made

**1. `mb-compile-rules-config.md` — rewritten to specification shape.** Identity: Issuer MyBizz; Installed version `2e2ac13` (2026-10-15); standard taken from the canonical source at that commit; last researched 2026-10-15. Naming-convention table (source keeps own name; compiled `cup-(rule-name).rego` single prefix; package must match the deployment folder; all rules global), installed-copy table (single copy, body-identical to `2e2ac13`; invocation), downstream table (estate pointer to [[[mybizz-config-docs:cupcake/cupcake-config]]](C:/mybizz/mybizz-config-docs/cupcake/cupcake-config.md); Cupcake enforcement as standing policy). Open items: none.

**2. `mb-compile-rules-reference.md` — rewritten.** Opens on the canonical-source authority and the body-identity requirement; corrected invocation and naming; the nine run-shaping rules carried with the naming bullet corrected; the evaluation schema trimmed to a pointer to [[[mybizz-config-docs:cupcake/cupcake-reference]]](C:/mybizz/mybizz-config-docs/cupcake/cupcake-reference.md) keeping only this suite's two exclusive facts (eval is inert; the agent never runs it); amendment pattern kept with history stripped; new Authoritative sources table (5 rows) and Checks table (Installed version diff and naming/estate checks agent-run; live-folder and binaries `ls` developer-run — command text names protected paths; Works = correspondence current in [[[mybizz-config-docs:cupcake/cupcake-config]]](C:/mybizz/mybizz-config-docs/cupcake/cupcake-config.md)); Do-not list carried with the incident date stripped.

**3. `mb-compile-rules-explainer.md` — rewritten to the five required sections.** Carried reasoning with date-stamped narrative stripped; new "Why the source keeps its own name" block; [[[default:security-global/infrastructure/sec-local-filesystem-access-boundary|Local Filesystem Access Boundary]]](C:/dev/project-library-global/security-global/infrastructure/sec-local-filesystem-access-boundary.md) added beside the existing three security principles; two-copies open question retired; new § 5 "What has to happen" (developer invokes by name, runs every real test and keep decision, keeps the installed copy body-identical after source changes, Cupcake stays active as standing policy, commit-all syncs the documents).

**4. `mb-compile-rules-learnings.md` — created with its required table** (the file had been empty of content). L1: the rule estate is recorded in [[[mybizz-config-docs:cupcake/cupcake-config]]](C:/mybizz/mybizz-config-docs/cupcake/cupcake-config.md), not in this suite — corrected wording as approved. L2: checks whose command text contains `cupcake` are developer-run — the guard denies on command text, not intent.

**5. Register row** in `config-manager-register.md`: Installed version `2e2ac13`, Last updated 2026-10-15, Last checked 2026-10-15, State Active.

All four documents' frontmatter (`doc-id`, `updated`) preserved byte-for-byte — never written or changed by hand.

The refreshed suite lives in `/mnt/c/mybizz/mybizz-config-docs/mb-compile-rules/`: [[[mybizz-config-docs:mb-compile-rules/mb-compile-rules-config|config]]](C:/mybizz/mybizz-config-docs/mb-compile-rules/mb-compile-rules-config.md) · [[[mybizz-config-docs:mb-compile-rules/mb-compile-rules-reference|reference]]](C:/mybizz/mybizz-config-docs/mb-compile-rules/mb-compile-rules-reference.md) · [[[mybizz-config-docs:mb-compile-rules/mb-compile-rules-explainer|explainer]]](C:/mybizz/mybizz-config-docs/mb-compile-rules/mb-compile-rules-explainer.md) · [[[mybizz-config-docs:mb-compile-rules/mb-compile-rules-learnings|learnings]]](C:/mybizz/mybizz-config-docs/mb-compile-rules/mb-compile-rules-learnings.md).

Verification evidence: `2e2ac13` confirmed in the identity, naming and installed-copy rows and in the sources table; register row confirmed at line 26; L1's approved wording confirmed verbatim; the source repository clean at `2e2ac13`; installed↔canonical body-identity confirmed by diff.

## Cross-system check results

- **Cupcake:** found and current — the estate (20 sources ↔ 19 deployed rule files plus the source-less control-plane rule), the fallback-home design and the operational status are recorded in [[[mybizz-config-docs:cupcake/cupcake-config]]](C:/mybizz/mybizz-config-docs/cupcake/cupcake-config.md), refreshed 2026-10-14 the day before this run. No mb-compile-rules-specific rule fired; enforcement state phrased as standing policy with a pointer, never as permanently true.
- **security-global:** all three previously cited principles verified present (`sec-review-before-trust`, `sec-dependency-skill-supply-chain`, `sec-defense-in-depth`); `sec-local-filesystem-access-boundary` added to the explainer's supply-chain paragraph.
- **Memory governor:** none found — suite documents are not governed facts; they reach the memory system through commit-all's sync.
- **Related systems:** skills-catalog row OK (source repo and install path correct); [[[mybizz-os-docs:os-reference-docs/repo-organization|Repository Organization]]](C:/mybizz/mybizz-os-docs/os-reference-docs/repo-organization.md) lists the repo Ready; siblings `mb-sentinel-review` and `mb-submit-memory` present, no structural coupling.

## Errors and denials

One policy denial, honoured with no retry and no reshape: a bash verification grep whose *pattern text* contained the word "Cupcake" was denied — the guard matches command text, not intent (recorded as this suite's learning L2). The writes were untouched; verification was completed with the read-only file tools, which the learnings record as unaffected. No bash command touching any protected path was ever issued by the agent; the live-folder and binaries checks were developer-run throughout.

## Not done (boundaries)

No edit to the canonical source or the installed copy by the agent — the developer corrected and installed both; no commit; no proposal for the writing skill's own learnings (nothing beyond its existing entries surfaced).

## Still open

1. The developer runs `bash /mnt/c/mybizz/scripts/commit-all.sh "mb-compile-rules: suite refreshed to doc-suite specification; naming convention corrected; estate delegated to cupcake-config"` — the hook assigns this report's `doc-id`/`updated`, converts the short-form links and pushes both suites into GBrain.
2. Revoke the three grant paths: `mg-revoke --path /mnt/c/mybizz/mybizz-config-docs/mb-compile-rules` (4/4 used), `mg-revoke --path /mnt/c/mybizz/mybizz-config-docs/mb-config-manager` (1/2 used), `mg-revoke --path /mnt/c/mybizz/mybizz-os-docs/os-reference-docs/reports` (1/1 used).
3. The refreshed suite's open items: none.
