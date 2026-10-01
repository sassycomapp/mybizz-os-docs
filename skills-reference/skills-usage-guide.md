---
title: "skills-usage-guide"
doc-id: "mb-84d02901"
updated: "2026-10-01"
---

# Skills Usage Guide

When to invoke each managed skill. Inventory, paths, and states live in
[[[mybizz-os-docs:skills-reference/skills-register|Skills Register]]](C:/mybizz/mybizz-os-docs/skills-reference/skills-register.md) — this document covers invocation and
sequence only.

## Ordering rules

1. **mb-doc-maintainer runs before mb-doc-auditor.** Maintainer keeps
   documentation current at save time; auditor verifies periodically. An audit
   right after a maintainer run audits a clean queue.
2. **mb-config-manager runs after any new skill or system is added.** A new
   skill is not "documented" until its config-manager run has completed
   (suite build-out, exam skill, register The use of registers has been discontinued. Please review row). Current example:
   mb-memory-system-diagnostic.
3. **`gbrain doctor --fast` runs before a full `mb-config-gbrain` exam.**
   Fast check first; escalate to the full exam only when it flags something
   or on a scheduled deep pass.
4. **mb-sysdoc-creator creates a suite; mb-config-manager exams it.** For a
   brand-new system: creator first, then manager.
5. **mb-submit-memory runs only on explicit invocation**, one file per run.
6. **mb-archify-scope runs inside archify workflows** (agent-invoked), never
   standalone.
7. **mb-sentinel-review runs at PDLF steps** (step-triggered), never
   standalone.

## 1. /mb-doc-maintainer — user + agent invoked

**When:** after saving documents (drains the pending-events queue left by the
save-watcher), or when asked to maintain documentation. Runs before
mb-doc-auditor (rule 1).

## 2. /mb-doc-auditor — user invoked

**When:** periodic or on-demand deep audit of the documentation estate; after
restructuring, migration, or G-brain ingestion changes; after a maintainer
run (rule 1).

## 3. /mb-submit-memory — user invoked

**When:** explicitly, to submit one real file's content through the governed
memory pipeline. One file per invocation, each with its own judgment. Never
auto-triggered, never batched.

## 4. /mb-sysdoc-creator — user invoked

**When:** creating the canonical doc suite (config/reference/explainer) for a
new system component. Sequence: creator → mb-config-manager (rule 4).

## 5. /mb-config-manager — user invoked (with `mb-config-manager\config-setup-prompt.md`)

**When:** after a new skill or system is added (rule 2); when a system's exam
skill needs building or re-verification. Not self-examinable (see its
explainer §9).

## 6. /mb-compile-rules — user invoked

**When:** converting a Cupcake rule from Markdown to a tested Rego policy.
Placement and testing are one action (wrong package name = silently dead
rule).

## 7. /mb-archify-scope — agent invoked

**When:** inside archify runs, for scoping. Never standalone (rule 6).

## 8. /mb-sentinel-review — agent invoked

**When:** step-triggered security review at a real PDLF step (rule 7).

## 9. /mb-memory-system-diagnostic — user invoked

**When:** after any memory-system change, or when memory health is in
question. Pending its own config-manager run (register The use of registers has been discontinued. Please review gap 1). Renamed from
`/memory-system-diagnostic` 2026-09-28.

## 10. /mb-config-gbrain — user invoked

**When:** deep GBrain configuration exam. Run `gbrain doctor --fast` first
(rule 3); full exam when the fast check flags issues or on a scheduled pass.

## 11. `gbrain doctor --fast` — terminal CLI

**When:** quick GBrain health check before deciding whether a full exam
(above) is warranted.