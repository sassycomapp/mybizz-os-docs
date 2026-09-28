---
title: "skills-register"
doc-id: "skills-register"
---

# Skills Register

Single reference for every skill installed on this machine: where it is
installed, where its documentation lives, what class it is, and its current
state. Companion document: [[skills-usage-guide|Skills Usage Guide]] (when to
invoke each skill).

## 1. Scope

**Managed** — every `mb-*` operational skill, every `mb-config-*` exam skill,
and the documentation suites of the systems those skills govern.

**Not managed (frontmatter)** — skills belonging to external suites or host
systems. They appear below at suite level only: GStack, Matt Pocock Skills,
Anvil agent skills, Magdoub wireframe, i-have-adhd, OpenCode's own skills,
GBrain's own skills.

## 2. Conventions

- **Naming.** Homegrown (Mybizz-created) skills carry the `mb-` prefix.
  Third-party skills keep their proprietary names (archify, graphify,
  wireframe, secure-software-engineering, and all suite members).
- **Two lives.** A managed skill exists as a documentation copy under
  `C:\mybizz\mybizz-config-docs\<system>\` and (when installed) as an
  executable copy in WSL.
- **Documentation-copy frontmatter:** `title` + `doc-id`, where doc-id equals
  the filename stem.
- **Installed-copy frontmatter:** `name` + `description`.
- **Install = copy, never move.** The suite folder keeps the canonical source;
  the installed copy is the only executing copy.
- **Classes.** *Operational* = the skill does work. *Config-management* = the
  skill examines/verifies a system's configuration. A system may have both.
- **States.** `Installed` (live) · `Deferred` (deliberately postponed, per
  config-manager register) · `Pending` (suite incomplete, work queued) ·
  `Retired` (no longer managed) · `Gap` (missing piece recorded).
- **Paths.** Installed paths are WSL paths. From Windows, prepend
  `\\wsl.localhost\Ubuntu\`. Doc folders are Windows paths under
  `C:\mybizz\mybizz-config-docs\`.
- Suite member installed paths follow the pattern `<install root>\<member>\SKILL.md`
  unless stated otherwise.

## 3. Operational skills (managed, individual rows)

| Skill                       | Invoked by                                  | Installed path (`~/.config/opencode/skills/…`)                  | Doc folder                                                                   | Frontmatter                                | State                                             |
| --------------------------- | ------------------------------------------- | --------------------------------------------------------------- | ---------------------------------------------------------------------------- | ------------------------------------------ | ------------------------------------------------- |
| mb-doc-maintainer           | user + agent                                | `mb-doc-maintainer/SKILL.md`                                    | `C:\mybizz\mybizz-config-docs\mb-doc-maintainer\`                            | OK                                         | Installed                                         |
| mb-doc-auditor              | user                                        | `mb-doc-auditor/SKILL.md`                                       | `C:\mybizz\mybizz-config-docs\mb-doc-auditor\`                               | OK                                         | Installed                                         |
| mb-submit-memory            | user                                        | `mb-submit-memory/SKILL.md`                                     | `C:\mybizz\mybizz-config-docs\mb-submit-memory\`                             | OK                                         | Installed                                         |
| mb-sysdoc-creator           | user                                        | `mb-sysdoc-creator/SKILL.md`                                    | `C:\mybizz\mybizz-config-docs\mb-sysdoc-creator\`                            | OK                                         | Deferred (exam)                                   |
| mb-compile-rules            | user                                        | `mb-compile-rules/SKILL.md`                                     | `C:\mybizz\mybizz-config-docs\mb-compile-rules\`                             | OK                                         | Deferred (exam)                                   |
| mb-sentinel-review          | agent                                       | `mb-sentinel-review/SKILL.md`                                   | `C:\mybizz\mybizz-config-docs\mb-sentinel-review\`                           | OK                                         | Deferred (exam)                                   |
| mb-archify-scope            | agent                                       | `mb-archify-scope/SKILL.md`                                     | documented inside `C:\mybizz\mybizz-config-docs\archify\` (own folder empty) | wrong — doc-style fields on installed copy | Installed                                         |
| mb-memory-system-diagnostic | user                                        | `mb-memory-system-diagnostic/SKILL.md`                          | `C:\mybizz\mybizz-config-docs\mb-memory-system-diagnostic\`                  | OK                                         | Pending (config-manager run outstanding)          |
| secure-software-engineering | agent                                       | `secure-software-engineering/SKILL.md`                          | none                                                                         | OK                                         | Gap (no doc copy)                                 |
| mb-config-manager           | user (via `config-setup-prompt.md` + agent) | not installed as a skill                                        | `C:\mybizz\mybizz-config-docs\mb-config-manager\`                            | OK                                         | Active (excluded from its own register by design) |
| archify                     | agent                                       | `archify/SKILL.md`; source `C:\mybizz\archify\archify\SKILL.md` | `C:\mybizz\mybizz-config-docs\archify\`                                      | OK                                         | Installed                                         |
| graphify                    | agent                                       | `graphify/SKILL.md`                                             | `C:\mybizz\mybizz-config-docs\graphify\`                                     | OK                                         | Installed                                         |

## 4. Config-management exam skills (managed, individual rows)

All installed at `~/.config/opencode/skills/<name>/SKILL.md`. Doc folder is
`C:\mybizz\mybizz-config-docs\<system>\<system>-config.md` (spec doc). All
states: Installed.

| Exam skill                       | System doc folder         |
| -------------------------------- | ------------------------- |
| mb-config-postgresql             | `postgresql\`             |
| mb-config-cupcake                | `cupcake\`                |
| mb-config-gbrain                 | `gbrain\`                 |
| mb-config-gstack                 | `gstack\`                 |
| mb-config-opencode               | `opencode\`               |
| mb-config-memory-governor        | `memory-governor\`        |
| mb-config-archify                | `archify\`                |
| mb-config-graphify               | `graphify\`               |
| mb-config-magdoub-wireframe      | `magdoub-wireframe\`      |
| mb-config-matt-pocock-skills     | `matt-pocock-skills\`     |
| mb-config-gbrain-dream-cycle     | `gbrain-dream-cycle\`     |
| mb-config-anvil-agent-references | `anvil-agent-references\` |
| mb-config-i-have-adhd            | `i-have-adhd\`            |
| mb-config-doc-maintainer         | `mb-doc-maintainer\`      |
| mb-config-doc-auditor            | `mb-doc-auditor\`         |
| mb-config-mb-submit-memory       | `mb-submit-memory\`       |

## 5. Suites (represented at suite level; members listed for completeness)

| Suite              | Members            | Install root                                                                                     | Doc folder                                             | Frontmatter managed |
| ------------------ | ------------------ | ------------------------------------------------------------------------------------------------ | ------------------------------------------------------ | ------------------- |
| GStack             | 53 + main `gstack` | `C:\mybizz\gstack\.opencode\skills\<member>\SKILL.md` (main also at `C:\mybizz\gstack\SKILL.md`) | `C:\mybizz\mybizz-config-docs\gstack\`                 | No                  |
| Matt Pocock        | 35                 | `C:\mybizz\matt-pocock-skills-source\skills\<category>\<member>\SKILL.md`                        | `C:\mybizz\mybizz-config-docs\matt-pocock-skills\`     | No                  |
| Anvil agent skills | 5                  | `C:\mybizz\anvil-agent-references\skills\<member>\SKILL.md`                                      | `C:\mybizz\mybizz-config-docs\anvil-agent-references\` | No                  |
| Magdoub wireframe  | 1 (`wireframe`)    | `~/.claude/skills/wireframe/SKILL.md`                                                            | `C:\mybizz\mybizz-config-docs\magdoub-wireframe\`      | No                  |
| i-have-adhd        | 1                  | `i-have-adhd/SKILL.md`                                                                           | `C:\mybizz\mybizz-config-docs\i-have-adhd\`            | No                  |

**GStack members:** autoplan, benchmark, benchmark-models, browse, canary,
careful, claude, context-restore, context-save, cso, design-consultation,
design-html, design-review, design-shotgun, devex-review, diagram,
document-generate, document-release, freeze, guard, health, investigate,
ios-clean, ios-design-review, ios-fix, ios-qa, ios-sync, land-and-deploy,
landing-report, learn, make-pdf, office-hours, open-gstack-browser,
pair-agent, plan-ceo-review, plan-design-review, plan-devex-review,
plan-eng-review, plan-tune, qa, qa-only, retro, review, scrape,
setup-browser-cookies, setup-deploy, setup-gbrain, ship, skillify, spec,
sync-gbrain, unfreeze, upgrade.

**Matt Pocock members:** ask-matt, claude-handoff, code-review,
codebase-design, diagnosing-bugs, domain-modeling, git-guardrails-claude-code,
grill-me, grill-with-docs, grilling, handoff, implement,
improve-codebase-architecture, loop-me, migrate-to-shoehorn, prototype,
research, resolving-merge-conflicts, scaffold-exercises,
setup-matt-pocock-skills, setup-pre-commit, setup-ts-deep-modules, tdd, teach,
to-questionnaire, to-spec, to-tickets, triage, wait-what, wayfinder, wizard,
writing-beats, writing-for-agents, writing-fragments, writing-shape.

**Anvil members:** anvil-yaml, client-forms, client-modules, data-models,
server-code.

## 6. Retired skills (not installed; kept here to prevent re-documentation)

mb-align-docs, mb-docs-manager, mb-wikilinks, mb-doccycle — all superseded
(see config-manager register, 2026-09-22 and 2026-09-27).

## 7. Known gaps

1. **mb-memory-system-diagnostic** — config-manager run outstanding: no exam
   skill, no managed-systems row; suite holds 4 of 8 standard files. Renamed
   from `memory-system-diagnostic` 2026-09-28 — the naming violation is
   resolved; only the config-manager run remains.
2. **secure-software-engineering** — installed, no doc copy in config-docs.
3. **mb-archify-scope** — installed copy carries doc-style frontmatter
   (`document:`/`doc-id:`) instead of `name:`/`description:`; flagged for
   correction.

## 8. Standing decisions (do not "fix")

1. **Out of service by intent.** The codex, connect-chrome, and
   hackernews-frontpage discovery entries are permanently retired. They are
   not defects; do not repair or re-register them.
2. **Skill output locations are fixed by design.** Run outputs live at
   `C:\mybizz\logs\` (8 lanes: cupcake, gbrain-logs, github-logs,
   mb-doc-maintainer, mb-submit-memory, mb-sysdoc-creator, mb-wikilinks,
   sessions-log — ~226 files), plus `C:\mybizz\memory-audit-reports\`,
   `C:\mybizz\memory-diagnostic-reports\`, and `C:\mybizz\archify\output\`.
   These paths are embedded in the skills' own logic (e.g.
   mb-doc-maintainer's pending-events queue, archify's output lane).
   Relocation is deliberately rejected: it would require re-editing skills —
   which must not happen — and the effort is not worth the outcome.
3. **memory-governor naming exemption.** The `memory-governor` suite folder
   and system name lack the `mb-` prefix but stay exactly as named — the
   system is deeply entrenched (MakePDLF among others); renaming effort is
   not worth the outcome. Do not rename.
4. **Root system folders.** `C:\mybizz\mb-doc-maintainer\` and
   `C:\mybizz\mb-doc-auditor\` are the skills' system/source folders — correct
   where they are. The actual skills are installed in WSL
   (`~/.config/opencode/skills/mb-doc-maintainer/SKILL.md`,
   `~/.config/opencode/skills/mb-doc-auditor/SKILL.md` — verified
   2026-09-28).