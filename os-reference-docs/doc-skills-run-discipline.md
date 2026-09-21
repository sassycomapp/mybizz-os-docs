---
document: "Documentation Skills — Shared Run Discipline"
doc-id: doc-skills-run-discipline
state: Live
date-created: 2026-09-19
---

# Documentation Skills — Shared Run Discipline

Single source of the run frame for every documentation skill (mb-align-docs,
mb-wikilinks, mb-docs-manager). Each skill points here at Phase 0 and adds only its own
specifics. Duplicated process text in skill files is retired in favour of this document.

## Scope authority

- Read [[mybizz-scope-list|Mybizz Scope List]] at Phase 0 of
  every run. It is the single scope authority; the skills never carry their own scope
  lists and never edit it (developer-owned).
- In-scope roots and exceptions come from that list alone. The list's criterion governs:
  an exception is content we never reference, in any form, at any time, ever.
- Transient state (a project under development) is a scheduling question — when to work
  on it — never a scope question. Transient projects stay in scope.
- Corpus enumeration within the roots: [[repo-organization|Repo organization]] (same folder).
- Treatment rules — how in-scope content is handled — live in each skill, not here.

## Run frame (every run, in order)

1. **Resume check.** Check the skill's `in-progress\` folder. If a record exists, stop
   and ask the developer: resume exactly from the recorded phase (decisions included),
   or move the record to `abandoned-runs\` (timestamp preserved, never deleted) and
   start fresh. Empty folder: clear `last-completed-run\` and proceed as a new run.
2. **Learnings load.** Load the skill's learnings folder before Detect. Never skipped,
   never deferred, never assumed empty without looking.
3. **Preconditions.** Git working trees clean for every repo the run may commit to;
   stop and report if any is dirty.
4. **Verified backup, stop-cold.** Before any write phase: copy the target corpora to
   the skill's backup root and verify by direct count comparison. A short or empty
   destination is BACKUP_FAILED and aborts the run. Git history is never a substitute.
5. **Working record.** One file in `in-progress\`, named with the run timestamp
   (Filename-Safe ISO 8601, UTC+2, per
   [[adr-timezone-utc-storage-display-conversion|Client Timezone: IANA String, UTC Storage, Display-Time Conversion]]), created the moment the run
   begins. Append at every decision point — full findings report, approval decisions
   verbatim, exact before/after per edit, verification results — minimum four appends
   per run. Never batched to the end, never summarized down.
6. **Run record at close.** The working record MOVES to `last-completed-run\` (a move,
   never a delete) and ONE index row is appended to the skill's run index file
   (align-docs: `run-index.md`; wikilinks: `run-log.md`; docs-manager: its log): run-id,
   date, target, findings, decisions, commits. The index row is the permanent audit
   trail; the full record is kept only for the most recent run.
7. **Learnings write-back** at close for anything the next run should know.

## Approvals

- Asked in chat, never the question tool (option lists are not scrollable on this host).
- Standard findings: grouped by target and finding type, one decision per group, every
  item individually listed inside it.
- High-risk findings (identifier changes, register surgery, quarantine moves): presented
  individually, labelled by risk type, each approved individually.
- Structural writes require a second explicit confirmation ("about to do this, proceed?")
  on top of the approval.
- One approval is one approval — never standing. A lost session never loses work:
  resume picks up prior decisions.

## Permission discipline (no sub-agents)

- Detection is read-only — never writes.
- Text-level corrections and structural actions are separate write classes; structural
  actions are renames, moves, retires, and register surgery.
- Logging is append-only.
- The orchestrating agent enacts these disciplines itself; sub-agents are never spawned
  for them (standing workchat rule 8).

## Verification

- After every write: re-read what was written, or diff against the backup, and confirm
  the approved change landed exactly before proceeding to the next.
- Evidence or silence — "done" without an evidence line is not done.

## Commits and sync

- **Commit plan first:** one table — repo, folders touched, file count — one batch
  approval for the whole commit set, then one commit per repo.
- After commits: `gbrain sync --source <id>` then `gbrain embed --stale --source <id>`
  for every changed source; verify `gbrain sources list` last-sync timestamps before
  reporting done. The default-source sync script covers only the default source.

## File search — es.exe (Everything) first

- Binary: `C:\Program Files\Everything 1.5a\es.exe`; invoke via WSL interop as
  `"/mnt/c/Program Files/Everything 1.5a/es.exe" <search-term>`.
- Flags: `-p` path-scoped (exact or folder prefix), `-n` result limit, `-s` sort by
  path, `-w` whole word, `-i` case-sensitive, `-r` POSIX regex.
- Raise `-n` (≥20) or scope with `-p` before filtering — a small unfiltered limit is
  consumed by backup copies before any live path appears and reads as "file not found."
- A bare filename searches the whole index (fastest way to find a moved/renamed file
  anywhere). Scoped searches use `-p` with a folder prefix. Results are Windows paths
  (`C:\...`); convert to `/mnt/c/...` before opening.
- Fallback to `find`/`glob` only if es.exe is unavailable, the service is down, or the
  index is clearly stale — and report the fallback in the run record. A silent fallback
  hides a stale-indexed answer; a zero-hit result on a supposedly existing file is
  verified with one fallback check before any missing/retire finding is proposed.

## Anvil code repos — denied entirely

Never opened, read, backed up, or edited — including any documentation folder inside
them (e.g. `theme\assets\docs`); those files are Anvil app assets and Anvil owns their
interpretation. Top-level paths are existence-checked only when referenced.
