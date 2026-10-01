---
title: "close-out-process"
doc-id: "mb-ed0bf026"
updated: "2026-10-01"
---
# Session Close-Out — Process Prompt

The developer says "close out" and pastes this file. The agent executes it start to finish.
The developer is present: one approval, then one terminal block (two when governed facts need
a review), then done. Every developer action is an explicit terminal block; the agent does
everything else.

This is the close-out for every kind of session. What the session produced decides what runs.
A simple session closes with the spine alone: page, gate, one block. Governed facts (Branch M)
run only when the session set a rule or decision that must be trusted later without re-checking.
When scope or routing is genuinely ambiguous, the agent asks one focused question instead of
guessing.

Self-contained: where the outer procedure documents are guard-blocked for agent reads (for
example [[[mybizz-os-docs:operating-procedures/opening-and-closing]]](C:/mybizz/mybizz-os-docs/operating-procedures/opening-and-closing.md)), this prompt is the authoritative procedure.

Reference: `C:\mybizz\logs\gbrain-logs\facts-log\facts-log.md` (index of saved facts),
`C:\mybizz\logs\sessions-log\sessions-log.md` (index of session pages),
[[[mybizz-os-docs:operating-procedures/fact-save-and-recall]]](C:/mybizz/mybizz-os-docs/operating-procedures/fact-save-and-recall.md), [[[mybizz-os-docs:operating-procedures/commit-all-repositories]]](C:/mybizz/mybizz-os-docs/operating-procedures/commit-all-repositories.md).

---

## Rules

1. **Grants.** The agent never approves a grant for itself. When a write or read is blocked, it
   asks for exactly `mg-grant --path <folder> --count N`, with the count the work needs (a grant
   decrements per write), and hands the line to the developer.
2. **Denials are final.** A denied item is never retried, reshaped or routed around. It becomes
   a developer-run command with its exact text; the result is recorded as developer-verified.
3. **One approval gate.** The developer approves once: the candidate facts with their routing,
   the proposed sessions-log row, and the list of changed files. After the gate it is execution only.
4. **GBrain is the record; the logs are indices.** The log rows are appended by `log-row.sh`,
   which numbers each row itself. The agent never numbers a row and never writes to a log.
5. **The agent never commits.** The developer's block runs `commit-all.sh`, which commits all
   eight repositories, updates GBrain and pushes.
6. **No placeholders.** A block contains only values that are already known. Anything that
   exists only in a previous block's output goes into a second, fully literal block.
7. **Nothing left open.** A step the agent cannot complete becomes a numbered command in the
   developer's block.

---

## Step 1 — agent: the session page

Write the summary with put_page, slug `sessions/<YYYY-MM-DD>-<NNN>-<short-slug>`. Keep it to
about one screen: what was done and the evidence, decisions and standards the developer set,
quirks, the documents written or changed, and what is open. The documents hold the detail; the
page says what happened and where to look. The write creates a file in the repository's
`sessions/` folder; `commit-all.sh` commits it.

## Step 2 — agent: candidates and the gate

- **Candidate facts.** Only durable rules, decisions and gotchas. One claim each, short. Skip any
  fact that a committed document already states. A session with no facts says so.
- **Routing, proposed per fact.**
  - **Advisory** (default): context, pointers, findings that only need to be findable again.
    Saved directly by the agent with `remember`. No check is needed.
  - **Governed**: rules, decisions and procedures that must be trusted later. Only these get a
    check: recall with real grep terms from the content, more than one query, then a genuine
    confidence and contradiction judgment. They go through the memory governor (Branch M).
  - When it is unclear which kind a fact is, ask one focused question at the gate.
- **Changed files.** Run `bash /mnt/c/mybizz/scripts/commit-all.sh --status` (or ask the
  developer to paste it). Flag anything that is not this session's work; the developer decides
  whether to keep or restore it before the block runs.
- **Present once**, in one message: the candidates table (subject, fact text, routing,
  confidence, conflict flags), the proposed sessions-log row without a number, and the flagged
  files. Then STOP. The approval may cancel, amend, reword or re-route; the amended set is final.
- Log formats (dates `YYYY/MM/DD`):
  - sessions-log: `| No. | Date | Topic | Subject | Title | GBrain page name |`
  - facts-log: `| No. | Date | Subject | Title | Fact | GBrain reference |`

## Step 3 — agent: after the approval

- **Advisory facts.** Save each with `remember`, visibility `world`, provenance naming the
  session, entity set whenever the fact has a subject. Keep the returned `fact-NN`.
- **Governed facts (Branch M).** One real file per fact at `<current-repo>/facts/<date>-<slug>.md`,
  frontmatter `title`, `state: Live`, `date-created` (the commit hook adds `doc-id` and `updated`).
  The governor reads the file itself from `source_ref`. One file, one judgment, never batched.
- **Memory components.** When the session updated or verified GBrain, the dream cycle, the memory
  governor, Graphify, mb-submit-memory or PostgreSQL, update the master explainer. It is committed
  on its own, before `commit-all.sh`. Editing it needs `mg-grant --path /mnt/c/mybizz/mybizz-os-docs --count 1`;
  if that is denied, the edit becomes a developer-run command.

## Step 4 — developer: terminal block 1

The agent writes this block fully literal. The governor part appears only when governed facts
were approved. The credential lives in `~/memory-governor/.env`; the agent never sources it.

```bash
# --- governed facts only ---------------------------------------------------
cd /home/dev-p/memory-governor && set -a && source .env && set +a

# owning project: idempotent ("already exists" is fine); project_id equals the gbrain
# source id of the repository that holds the fact files
curl -s -X POST http://127.0.0.1:8321/memory/projects \
  -H "X-Governor-Credential: $MEMORY_GOVERNOR_WRITER_MANUAL_SUBMISSION" \
  -H "Content-Type: application/json" \
  -d '{"project_id": "FILLED-BY-AGENT", "display_name": "FILLED-BY-AGENT"}'

# one submission per fact file; visibility shared_all always; confidence from the gate judgment
curl -s -X POST http://127.0.0.1:8321/memory/candidate \
  -H "X-Governor-Credential: $MEMORY_GOVERNOR_WRITER_MANUAL_SUBMISSION" \
  -H "Content-Type: application/json" \
  -d '{"owning_project": "FILLED-BY-AGENT", "visibility": "shared_all", "retention": "long_term", "memory_type": "fact", "source_type": "user", "source_ref": "FILLED-BY-AGENT", "policy_version": "v1", "confidence": {"score": 0.0, "method": "model_based"}}'

# --- every close-out -------------------------------------------------------
# the sessions-log row, and one facts-log row per saved advisory fact
bash /mnt/c/mybizz/scripts/log-row.sh sessions "YYYY/MM/DD" "topic" "subject" "title" "gbrain-page-name"
bash /mnt/c/mybizz/scripts/log-row.sh facts "YYYY/MM/DD" "subject" "title" "fact text" "fact-NN"

# master explainer, only when a memory component was touched (its own commit)
git -C /mnt/c/mybizz/mybizz-os-docs add os-reference-docs/memory-master-explainer.md && git -C /mnt/c/mybizz/mybizz-os-docs commit -m "memory: master-explainer - TOPIC (close-out)"

# commit all repositories, update GBrain, push
bash /mnt/c/mybizz/scripts/commit-all.sh "close-out message"
```

## Step 5 — developer: terminal block 2 (only when governed facts were submitted)

The developer pastes block 1's output. A shared (world-visible) fact lands in `needs_review` by
design. The agent composes one fully literal promote per returned memory id; the promote is the
developer's executing act on content already approved at the gate. The same block adds one
facts-log row per governed fact, with the `fact-NN` taken from the worker log (Step 6).

```bash
cd /home/dev-p/memory-governor && set -a && source .env && set +a

curl -s -X POST http://127.0.0.1:8321/memory/LITERAL-MEMORY-ID/review \
  -H "X-Governor-Credential: $MEMORY_GOVERNOR_WRITER_MANUAL_SUBMISSION" \
  -H "Content-Type: application/json" \
  -d '{"decision": "promote", "reason": "approved at the DATE close-out gate", "reviewer": "dev-p"}'

bash /mnt/c/mybizz/scripts/log-row.sh facts "YYYY/MM/DD" "subject" "title" "fact text" "fact-NN"
```

## Step 6 — agent: verify and report

- **Governed facts.** `grep "remembered as fact" /tmp/memory-governor-worker.log | tail -10` is the
  authoritative record of what landed and its fact numbers. If recall returns nothing but the worker
  log confirms the write, the fact exists: record the number, never re-submit. A candidate still
  `under_review` is reported with its memory id and resolved by the developer, never by the agent.
  The review queue must be empty before the close-out ends.
- **Commit, GBrain, push.** The `commit-all.sh` summary must show every repository `committed` or
  `nothing to commit`, `pushed` or `already up to date`, and end with `All done.` A BLOCKED
  repository is fixed by the developer and the same command is run again.
- **Final report.** Verified outcome of each step: facts and numbers, log rows, commit summary,
  flagged anomalies for the developer's decision, and the pointer to the next work.

Completion status: `DONE`; `DONE_WITH_REVIEWS_PENDING` (list every open memory id); `BLOCKED`
(state exactly what stopped it and what was tried). Never mark DONE with an unverified step.

---

## Guard classes a close-out meets (expect them; never fight them)

| surface | class | disposition |
|---|---|---|
| `~/memory-governor/.env` sourcing | secrets guard, permanent | developer terminal only (Step 4) |
| file-tool read or write of protected-path `.md` (the logs, opening-and-closing.md) | sensitive file, permanent | the developer's block appends; this prompt carries the formats |
| write outside the current project | per-occasion slip | request `mg-grant --path <folder> --count N`; if denied anyway, developer block, never retry |
| command text containing guarded tokens (for example `cupcake`) | token-literal, permanent | approved glob form where one exists, otherwise hand to the developer |

## Traps

- **A later visibility correction is swallowed.** A `project_local` submission followed by a
  `shared_all` twin is deduplicated by the worker and the world version never lands. Submit the
  final intended visibility, `shared_all`, as the first and only submission.
- **Recall may not show a saved fact.** Agent-side recall filters private facts. The worker log is
  the truth for existence and numbers. Never re-submit on an empty recall.
- **needs_review is designed behaviour** for shared content. Resolve it through the review
  endpoint with the developer's decision, never by re-submitting with a different confidence.
- **An open review item blocks PDLF steps everywhere.** The queue must be empty at the end.
- **Credentials never enter the agent's environment.** Reference by variable name only; never
  print, log or copy one.
