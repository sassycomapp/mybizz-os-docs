---
title: "2026-10-15-cupcake-config-management-report"
doc-id: "mb-9c03546b"
updated: "2026-10-15"
---
# Report — mb-config-manager → mb-sysdoc-creator: cupcake (2026-10-15)

## What the task was

The developer invoked `/mb-config-manager cupcake`. The register showed the plugin (plugin, Active) with every value cell empty — the system's first examination. The examination ran 2026-10-14 (log-timestamp evidence; the register's Last checked carries that date) and closed 2026-10-15. It produced one BEHIND and three DRIFT findings; the developer stayed on the installed version, added a DEVIATION finding (the four documents predate [[[mybizz-config-docs:mb-sysdoc-creator/doc-suite-specification|Doc Suite Specification]]](C:/mybizz/mybizz-config-docs/mb-sysdoc-creator/doc-suite-specification.md)), folded the drifts into a full suite Refresh through mb-sysdoc-creator, and steered the run through a denied write, a live grant anomaly, a developer-authorised test, two approval gates and a post-apply addendum. The run ended DONE.

## Examination findings (mb-config-manager)

- **BEHIND** — upstream latest v0.5.1 (released 2025-12-10) vs installed `0.3.0` (upstream release 2025-11-27); every release carries boilerplate notes ("Initial Release") and the v0.3.0→v0.5.1 range is 103 commits / 311 files, too large for GitHub to render. Upstream also ships a continuously updated OpenCode plugin asset that would replace the locally amended plugin. Developer decision: **stay on 0.3.0**, recorded as learning L5.
- **DRIFT** — the backup-set record was incomplete: three pre-amendment backups from the last amendment run existed on disk unrecorded (plugin `.bak-20261004-pre-f1f2`, policy-folder cross-project `.bak-20261004-pre-f7`, five grant-tool `.bak-20261004-pre-f2` files), plus an unlisted plain `builtins/system_protection.rego.bak`. Resolution (developer-directed): recorded as one config fact — "backup files present in the plugin, policy, builtins and grant-tools folders; never loaded" — not a list.
- **DRIFT** — the F-4 cruft sentence (five `:Zone.Identifier` artifacts and `system_protection.rego (2).bak` "slated for developer removal") was stale: verified absent in every Cupcake folder. Resolution: deleted outright.
- **DRIFT** — the version row claimed "no upstream release listing is visible to corroborate it"; the upstream releases page now lists v0.3.0 (2025-11-27) and the developer's terminal confirmed `cupcake --version` = 0.3.0. Open item 3 closed; the register's Last updated = 2025-11-27 (the installed version's release date, developer-decided).
- **DEVIATION** (developer-added) — all four documents predate the doc-suite specification: config carried prose, an operational-history table and freeform tables; reference had no Authoritative-sources or Checks tables and carried values; explainer had history sections and lacked "What has to happen"; learnings used off-spec columns and carried dates. Correction: one full Refresh with every drift folded in.

## Live verification performed

Policy-folder inventory verified against the config (20 custom rules, 3 builtins, 3 grants data files, the `system/evaluate.rego` aggregator); rule-source correspondence verified both sides (20 sources ↔ 19 deployed files with sources + the source-less control-plane rule); the compiled cross-project rule read live (`/tmp/`-only exempt list; the skills-dir amendment present); the plugin source grepped for every recorded amendment marker (`failMode: "open"`, the fallback home, three-file prune/decrement, the mkdir-lock); `opa version` = 1.19.1; `"task": "deny"` confirmed in `opencode.jsonc`; the spawn denylist read; all three grants data files read (ephemeral, empty); the fallback skeleton listed (exactly the five documented files); the plugin-load log grep (global path only); a live liveness probe — an ungranted webfetch denied by the outbound-network rule, proving enforcement in-session; upstream fetched: the releases page, the version-pinned README at tag v0.3.0 and the LICENSE file (which revealed the license correction: MIT, not the recorded Apache-2.0); the outbound allowlist read live in the rule file (exactly 11 domains, now itemized); the plugin's spawn call read live (`cwd: skeletonDir`, superseding the recorded `cwd: event.cwd`); three GBrain searches with varied terms.

## The grant-channel events

1. The first write (learnings row 5) was denied by the cross-project rule — session project vs docs folder. Honoured; grant requested, issued, write applied and verified.
2. The register-row write was then denied **despite a live count-1 slip**; the grants file showed both entries at count 0 — one grant unit consumed without any successful write to that folder. Plugin-source reads pinned a suspect: `tool.execute.after` (lines 830–854) decrements every write-type call that reaches it and `decrementGrantCount` (lines 517–540) has no outcome guard, while the code comment claims success-only.
3. The developer rejected a learnings row for it ("unproven; learnings carry no dates or narrative") and offered the alternative hypothesis — write 1 decremented both entries — then revoked and re-issued both grants at count 3 as a developer-authorised test. The test never reached its observable: the developer had pre-applied the register row themselves, so the agent's edit failed on oldString-miss and wrote nothing. Clean datum from the failed attempt: a content-mismatch failure consumes no grant unit (counts stayed 3/3).
4. The anomaly is recorded in the refreshed config as an open item without a date — two named hypotheses, unproven, needing a developer-designed live test. Nothing stronger was written anywhere.

## Developer decisions applied

Stay on 0.3.0 (learnings entry verbatim, later date-stripped); the DEVIATION with the three drift folds; register Last updated = 2025-11-27; write nothing outside the cupcake documents folder and change no live Cupcake file; stop at each approval gate; change list approved with corrections (dates stripped from every learnings row; open item stated without a date); 4 whole-document writes at apply; drafts approved including the uninstall correction (the uninstall's grant-tool removal line now covers `git-grant`, `net-grant`, `net-revoke`); post-apply addendum: learnings L6 (failMode open, developer-supplied text) with the config's failMode row Learning cell pointed to L6, and sysdoc-creator learnings L18.

## Changes made

**1. `cupcake-learnings.md` — restructured to the specification's table** (`ID | Learning | Why it holds | Source`, IDs L1–L6). Dates stripped from every row; right-action and destination content refolded to their own homes (the count-2 parallel-writes re-probe and the cwd-independent liveness probe moved into the reference's Checks table; the denial-recording discipline into the Checks notes; the pending C2-5/C3-3 skill-amendment pointers flagged for mb-compile-rules, not stored). L5 records the stay decision; L6 records the fail-open decision.

**2. `cupcake-config.md` — rewritten as the specification's settings sheet** (102 lines). Identity: issuer EQTYLab (engine) / MyBizz (plugin amendments, rules, grant tools); installed version `0.3.0` corroborated (upstream release 2025-11-27; `cupcake --version`); standard taken from the repo at tag v0.3.0; **license corrected to MIT** (v0.3.0 LICENSE); last researched 2026-10-14. Settings tables: engine and install (including the spawn-cwd correction — the checker boots from the skeleton dir or the fallback home, not the recorded `cwd: event.cwd`); the rule estate (all 20 rules + 3 builtins + aggregator with their blocks, correspondence, and the allowlist itemized at exactly 11 domains — verified live in the rule file); governance (task deny, denylist, standing review layer); the three grant channels with schema, expiry, decrement scope and serialization; operational status as standing policy phrased per sysdoc-creator learning L8; backup files as the one approved fact. Open items (five): the control-plane rule's missing source document; the skills-dir ancestor-cwd case with its two-step developer-side check; the grant-decrement anomaly (unproven, two hypotheses, no date); the protected-file-rules revisit decision; the deeper v0.3.0 docs unfetched. History and prose retired — git keeps them.

**3. `cupcake-reference.md` — rewritten execution-only** (243 lines). The rule-build decision flow, the verified evaluation syntax (agent-never-runs stated), the three grant-tool procedures with full-cycle worked examples, the plugin-load-once check, total-crash recovery (including the historical diagnostic sequence), disable/uninstall procedures with the approved grant-tool correction, the new **Authoritative sources** table (README, LICENSE and releases page at v0.3.0, checked 2026-10-14), the new **Checks** table (twelve rows naming method and run-by — agent vs developer — including the decrement re-probe and the liveness probe), and the Do-not list extended (silent-pass temp suffixes, empty-skeleton prohibition, `inspect` named among blocked subcommands). Values moved to config.

**4. `cupcake-explainer.md` — restructured to the five required sections** (123 lines): What it is / What it can do / Suitability / Why it works this way / What has to happen. The incident history (§7A–7F, §8, §8A) and the closing retired — git keeps them — with their durable lessons refolded as compact why-statements (only real tests count; one complete write per rule; silent builtin failures; never work around exec-protection; fails open by design; grants make approval mechanical). Commands, values and citations removed to the other documents.

**5. `sysdoc-creator-learnings.md` — L18 added** (developer-approved): on Refresh, check the learnings table's own structure against the specification, not only the rows' content.

**6. Register row** in `config-manager-register.md`: `cupcake | plugin | cupcake | 0.3.0 | 2025-11-27 | 2026-10-14 | Active`.

All five documents' frontmatter (`doc-id`, `updated`) preserved byte-for-byte — never written or changed by hand. All cross-document links written short-form for the commit hook.

## Cross-system check results

- **Cupcake rules:** deployed↔source correspondence verified live this run; the control-plane source reconciliation remains open (recorded).
- **security-global:** one conflict found — `sec-fail-closed.md` records "Known Exceptions: None identified", but the environment's deliberate `failMode: open` default is exactly such an exception. This run resolved the record inside the cupcake suite (learnings L6, config pointer); the security-global document itself was not edited (outside the run's write scope) — see Still open. `sec-ai-agent-execution-sandbox.md` verified consistent with the recorded three-layer task denial.
- **Memory governor:** none found — no special governor treatment of this suite's documents.
- **Related systems:** mb-compile-rules owns the pending C2-5/C3-3 amendments (flagged in chat, not stored); the v5 architecture's rule-placement guidance (rules belong to Cupcake's config) is followed by this refresh.

## Errors and denials

Three denials, all honoured with no retry and no reshape: (1) the cross-project write denial that triggered the grant flow; (2) the register-write denial despite a live slip — the run's central anomaly, investigated as above and recorded as an open item, never worked around; (3) the deliberate outbound-network deny used as the liveness probe — denial by design, recorded as evidence. A failed-content edit (oldString-miss, after the developer's own pre-application of the row) wrote nothing and consumed no grant unit. The webfetch to `example.org` and the GitHub fetches behaved as the allowlist dictates.

## Not done (boundaries)

No live Cupcake file was changed at any point — the policy folder, the plugin, the grant tools and all grants data files were only read. No upstream install or upgrade (the stay decision). `sec-fail-closed.md` not edited. No commits. The C2-5/C3-3 amendments to mb-compile-rules not relocated (flagged only — that is that system's run).

## Still open

1. Developer: `bash /mnt/c/mybizz/scripts/commit-all.sh "cupcake: doc suite refreshed to doc-suite specification — settings sheet, execution reference, five-section explainer, learnings L1-L6"` — the hook assigns `doc-id`/`updated`, converts the short-form links and pushes into GBrain.
2. Developer: revoke the grant paths — `mg-revoke --path /mnt/c/mybizz/mybizz-os-docs/os-reference-docs/reports` (1/1 used), `mg-revoke --path /mnt/c/mybizz/mybizz-config-docs/cupcake` (2/2 used on the last slip), `mg-revoke --path /mnt/c/mybizz/mybizz-config-docs/mb-config-manager` (last known 3, idle since the refresh issuance), `mg-revoke --path /mnt/c/mybizz/mybizz-config-docs/mb-sysdoc-creator` (1/1 used).
3. Developer: design and run the live test for the grant-decrement anomaly (config open item 3) — deciding between the two recorded hypotheses.
4. Developer: the skills-dir ancestor-cwd two-step check (config open item 2) — needs a session started in `/home/dev-p`; the exact command text lives in the run #2 exam report, which was not found on disk under `/mnt/c/mybizz/logs/` (locate or restate).
5. Developer: decide whether `sec-fail-closed.md` gains a Known Exceptions entry naming the fail-open exception (a security-global edit, separate run).
6. Agent (future runs): the protected-file-rules revisit decision and the control-plane source reconciliation remain recorded open items; the deeper `docs/` tree at tag v0.3.0 remains unfetched.
