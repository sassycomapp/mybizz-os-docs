---
document: "Skill Index"
doc-id: skill-index
state: Live
date-created: 2026-09-05
---

# Skill Index

Real skill files on this machine, enumerated from the confirmed discovery paths
(~/.config/opencode/skills/, ~/.agents/skills/, ~/.claude/skills/).
Each path is the real file on disk, not a symlink alias.

Legend: [working] = SKILL.md opens and reads cleanly; [BROKEN] = discovery
entry exists but its target is missing.

---

## GStack

### gstack (main)
- gstack (repo root) — C:\mybizz\gstack\SKILL.md — working
- gstack (opencode copy) — C:\mybizz\gstack\.opencode\skills\gstack\SKILL.md — working

### gstack-* skills (real path: C:\mybizz\gstack\.opencode\skills\<name>\SKILL.md)
- gstack-autoplan — working
- gstack-benchmark — working
- gstack-benchmark-models — working
- gstack-browse — working
- gstack-canary — working
- gstack-careful — working
- gstack-claude — working
- gstack-context-restore — working
- gstack-context-save — working
- gstack-cso — working
- gstack-design-consultation — working
- gstack-design-html — working
- gstack-design-review — working
- gstack-design-shotgun — working
- gstack-devex-review — working
- gstack-diagram — working
- gstack-document-generate — working
- gstack-document-release — working
- gstack-freeze — working
- gstack-guard — working
- gstack-health — working
- gstack-investigate — working
- gstack-ios-clean — working
- gstack-ios-design-review — working
- gstack-ios-fix — working
- gstack-ios-qa — working
- gstack-ios-sync — working
- gstack-land-and-deploy — working
- gstack-landing-report — working
- gstack-learn — working
- gstack-make-pdf — working
- gstack-office-hours — working
- gstack-open-gstack-browser — working
- gstack-pair-agent — working
- gstack-plan-ceo-review — working
- gstack-plan-design-review — working
- gstack-plan-devex-review — working
- gstack-plan-eng-review — working
- gstack-plan-tune — working
- gstack-qa — working
- gstack-qa-only — working
- gstack-retro — working
- gstack-review — working
- gstack-scrape — working
- gstack-setup-browser-cookies — working
- gstack-setup-deploy — working
- gstack-setup-gbrain — working
- gstack-ship — working
- gstack-skillify — working
- gstack-spec — working
- gstack-sync-gbrain — working
- gstack-unfreeze — working
- gstack-upgrade — working

### GStack — broken discovery entries
- codex — ~/.config/opencode/skills/codex -> C:\mybizz\skills\codex\ — BROKEN (target directory C:\mybizz\skills does not exist)
- connect-chrome — ~/.config/opencode/skills/connect-chrome -> C:\mybizz\skills\connect-chrome\ — BROKEN (target directory C:\mybizz\skills does not exist)
- hackernews-frontpage — ~/.config/opencode/skills/hackernews-frontpage -> C:\mybizz\skills\browser-skills\hackernews-frontpage\ — BROKEN (target directory C:\mybizz\skills does not exist)

---

## Anvil

Real path: C:\mybizz\anvil-agent-references\skills\<name>\SKILL.md
- anvil-yaml — working
- client-forms — working
- client-modules — working
- data-models — working
- server-code — working

---

## Matt Pocock

Real path: C:\mybizz\matt-pocock-skills-source\skills\<category>\<name>\SKILL.md
- ask-matt — working
- claude-handoff — working
- code-review — working
- codebase-design — working
- diagnosing-bugs — working
- domain-modeling — working
- git-guardrails-claude-code — working
- grill-me — working
- grill-with-docs — working
- grilling — working
- handoff — working
- implement — working
- improve-codebase-architecture — working
- loop-me — working
- migrate-to-shoehorn — working
- prototype — working
- research — working
- resolving-merge-conflicts — working
- scaffold-exercises — working
- setup-matt-pocock-skills — working
- setup-pre-commit — working
- setup-ts-deep-modules — working
- tdd — working
- teach — working
- to-questionnaire — working
- to-spec — working
- to-tickets — working
- triage — working
- wait-what — working
- wayfinder — working
- wizard — working
- writing-beats — working
- writing-for-agents — working
- writing-fragments — working
- writing-shape — working

---

## Magdoub / Wireframe

- wireframe — ~/.claude/skills/wireframe/SKILL.md (WSL: /home/dev-p/.claude/skills/wireframe/SKILL.md) — working

---

## Custom Skills (mb-*)

Real path: ~/.config/opencode/skills/<name>\SKILL.md
- mb-align-docs — working
- mb-compile-rules — working
- mb-docs-manager — working
- mb-sentinel-review — working
- mb-submit-memory — working
- mb-wikilinks — working

---

## Archify

- archify — C:\mybizz\archify\archify\SKILL.md — working

---

## Graphify

- graphify — ~/.config/opencode/skills/graphify/SKILL.md (WSL: /home/dev-p/.config/opencode/skills/graphify/SKILL.md) — working

---

## PDLF

- pdlf — ~/.config/opencode/skills/pdlf/SKILL.md -> C:\pdlf\skill\SKILL.md — BROKEN (target file does not exist; C:\pdlf exists but has no skill\ subdirectory)

---

## Totals

- Total skills listed: 108
- Working: 104
- Broken: 4 (codex, connect-chrome, hackernews-frontpage, pdlf)
