---
title: "close-out-process"
doc-id: "mb-ed0bf026"
---
# Session Close-Out — Process Prompt

The developer says "close out" and pastes this file. The agent executes it
verbatim, start to finish. Every developer action is an explicit terminal
block; the agent does everything else.

**This is the universal close-out for EVERY task** — coding, analysis,
configuration, documentation, memory work — not a memory-family procedure.
The agent bases the close-out on the **context of the session being
closed**: what the session produced decides which phases run. Memory-family
machinery is a conditional branch (Branch M), entered only when the session
actually produced governed facts or touched a memory component. A simple
task closes with the spine alone: page, gate, logs, commit, sync, report.

**The developer is present.** When scope, classification, or routing is
genuinely ambiguous, the agent asks one focused question instead of
guessing — never defaults silently.

Self-contained: where the outer procedure documents are guard-blocked for
agent reads (e.g. [[opening-and-closing|Operating Procedure]] — sensitive-file class), this
prompt is the authoritative procedure. Proven end-to-end live on
2026-09-14 (session 2026-09-14-002); v2.1 encodes that run's challenges,
their solutions, and the developer's refinement that the process is
context-based, not memory-specific.

Reference files (developer-side; agent reads what the guards allow):
- `C:\mybizz\logs\gbrain-logs\facts-log\facts-log.md` — index of saved facts
- [[2026-09-15-session-numbers-from-sessions-log|session numbers come from the sessions log]] — index of session pages
- [[opening-and-closing|Operating Procedure]]
- [[fact-save-and-recall|Operating Procedure]]

---

## 0. Hard rules — do not deviate

1. **Grants.** The agent NEVER approves a grant for itself — that defeats
   the objective. When a write or read is blocked, the agent requests it in
   exactly this format and hands the line to the developer, who arranges it
   in the WSL terminal: `mg-grant --path <folder> --count N`.
2. **Grant counts and occasions.** A grant decrements per write — request
   the count the work actually needs (trap T7). A slip held from earlier in
   the session may not satisfy a later write. If a write is denied despite
   a grant: do NOT retry — same shape or reshaped — the item becomes a
   developer-run command in the next terminal block.
3. **Denials are honored absolutely.** Never retried through an alternate
   path, never reshaped, never token-substituted. A denied item becomes a
   developer-run command with its exact text; results return verbatim and
   are recorded developer-verified.
4. **Logs are indices; GBrain is the record.** `facts-log.md` and
   [[2026-09-15-session-numbers-from-sessions-log|session numbers come from the sessions log]] are brief, append-only indices. The durable record is
   GBrain: the session summary page must be substantial — full context,
   decisions, quirks, dispositions, standards the developer set.
5. **One approval gate.** The developer approves exactly once per close-out:
   the candidate facts (with any cancels/amendments and the proposed
   routing per fact) AND the proposed sessions-log row, presented together.
   After that gate the close-out is pure execution.
6. **Nothing left open.** Any step the agent cannot complete becomes a
   numbered command in the developer's next terminal block. The run is DONE
   only when: saved facts verified, both logs updated, commits pushed,
   sync completed and verified — each scaled to what the session actually
   produced.
7. **Context decides; ask when ambiguous.** Which branches run, which
   facts are governed vs advisory, which repos need commits — these are
   decided by the session's context, proposed by the agent at the gate, and
   confirmed by the developer. The developer is right here: one focused
   question is always cheaper than a wrong assumption.
8. **Log numbering is developer-supplied — the agent never guesses it.**
   The agent cannot read the logs (sensitive class), so it never knows the
   next index. Rows are drafted WITHOUT a number; the developer enters the
   log's own next zero-padded index at paste time via the `read -p` lines
   in M-3 / Phase G (live failure 2026-09-15: a guessed number plus a
   mangled paste produced a numberless row).

---

## Phase A — agent: session page to GBrain (first, before anything else)

Write the session summary page via put_page, slug
`sessions/<YYYY-MM-DD>-<NNN>-<short-slug>`. Substantial compiled truth,
adapted to the session's subject: what was done (with evidence), quirks and
their dispositions, standards the developer set, artifacts, open/next. The
put_page write-through creates the file in the repo's `sessions/` folder —
commit it in Phase F. No grant needed.

## Phase B — agent: fact candidates + contradiction check → the ONE gate

- Build candidate durable facts from the session, **if the session produced
  any** — a session with none simply says so and skips to the gate for the
  log row. Candidates are single-claim and short (substantial context lives
  in the session page and fact files). For each, query GBrain for related
  facts — recall with real grep terms drawn from the content, more than one
  query — and form a genuine confidence/contradiction judgment.
- **Propose the routing per candidate** (the developer confirms at the
  gate):
  - **Advisory** — context, pointers, findings that just need to be
    findable again. Saved directly via `remember` (world) by the agent —
    instant, no developer block.
  - **Governed** — rules, decisions, procedures that must be trusted later
    without re-checking. Saved through the memory governor (Branch M) —
    deliberate by design.
  - When it is genuinely unclear which kind a fact is, ask the developer
    one focused question at the gate rather than guessing.
- Draft the proposed sessions-log row in the same pass (format below).
- Present exactly once, in one message: the candidates table (subject,
  fact text, proposed kind + routing, confidence, conflict flags) + the
  proposed sessions-log row. Then STOP.
- The developer's single approval may cancel, amend, reword, or re-route
  items; the amended set is authoritative. Log formats (dates YYYY/MM/DD):
  - facts-log: `| No. | Date | Subject | Title | Fact | GBrain reference |`
    — reference is `fact-NN`, filled after saving.
  - sessions-log: `| No. | Date | Topic | Subject | Title | GBrain page name |`
- **Two numbering spaces, never conflated** (live failure 2026-09-15): the
  `No.` column is the LOG's own zero-padded sequential index (`001`,
  `002`, … — last row + 1). The gbrain fact number (`fact-NN`) goes ONLY
  in the GBrain reference column. The agent drafts rows WITHOUT a number
  and never guesses one — it cannot read the logs (sensitive class).

## Phase C — routing (decided by the session's context, confirmed at the gate)

| session context | runs |
|---|---|
| Advisory facts approved | Phase C-A (agent-run `remember`) |
| Governed facts approved (rules, decisions, procedures — typical of memory-family and standards-setting sessions) | Branch M (fact files → developer block 1 → verify) |
| No facts approved | skip to Phase F |
| Session touched a memory component (GBrain, dream cycle, memory-governor, Graphify, mb-submit-memory, PostgreSQL) | master-explainer check inside Branch M |
| Any file changes in any repo | Phases F–H (commits, sync) |
| Pure-analysis session, no file changes | commit only the session page (its write-through file), then Phase H |

### Phase C-A — agent: advisory facts (no developer block needed)

For each advisory fact: save via `remember` with visibility `world`,
provenance naming the session and context, entity set whenever the fact has
a subject. Capture the returned `fact-NN` per fact for the facts log.

## Branch M — governed facts (memory-governor path)

Entered only when governed facts were approved at the gate.

### M-1 — agent: fact files (current repo, no grant needed)

One real file per approved governed fact at
`<current-repo>/facts/<date>-<slug>.md`, frontmatter: document / doc-id /
state: Live / date-created. Substantial context per the developer's record
guidance — the governor reads the file itself from `source_ref`, and the
file is committed in Phase F. One file, one judgment — never batched, never
a shared assessment.

### M-2 — master-explainer check (only when a memory component was touched)

If the session updated or verified a memory-family component, the
master-explainer update at close-out is the standing rule (own commit, in
Phase G). Editing that file needs a cross-project write: request
`mg-grant --path /mnt/c/mybizz/mybizz-os-docs --count 1` and apply once
granted; if the grant path is denied, the edit becomes a developer-run
command.

### M-3 — DEVELOPER TERMINAL BLOCK 1 (governor submissions + sessions log)

The credential lives in `~/memory-governor/.env`; agent-side `.env`
sourcing is a permanent guard-denial class — the developer's terminal needs
no grant. The agent prepares this block with every value that is already
known filled in — the block contains NO angle-bracket placeholders (trap
T8). Values that only exist in this block's OUTPUT (the `memory_id`s) are
resolved in M-3b, never here.

```bash
cd /home/dev-p/memory-governor && set -a && source .env && set +a

# 1. Owning project (idempotent — "already exists" is fine); project_id MUST
#    equal the gbrain source id of the repo holding the fact files
#    (agent fills FILLED-BY-AGENT before handing over)
curl -s -X POST http://127.0.0.1:8321/memory/projects \
  -H "X-Governor-Credential: $MEMORY_GOVERNOR_WRITER_MANUAL_SUBMISSION" \
  -H "Content-Type: application/json" \
  -d '{"project_id": "FILLED-BY-AGENT", "display_name": "FILLED-BY-AGENT"}'

# 2. One submission per fact file — visibility shared_all ALWAYS (trap T1);
#    confidence/method from the agent's gate-passed judgment; source_ref is
#    the absolute path to the fact file (agent fills FILLED-BY-AGENT)
curl -s -X POST http://127.0.0.1:8321/memory/candidate \
  -H "X-Governor-Credential: $MEMORY_GOVERNOR_WRITER_MANUAL_SUBMISSION" \
  -H "Content-Type: application/json" \
  -d '{"owning_project": "FILLED-BY-AGENT", "visibility": "shared_all", "retention": "long_term", "memory_type": "fact", "source_type": "user", "source_ref": "FILLED-BY-AGENT", "policy_version": "v1", "confidence": {"score": 0.0, "method": "model_based"}}'

# 3. Sessions log — the approved row, verbatim (universal: every close-out).
#    The agent drafts the row WITHOUT a number; the developer enters the
#    log's next zero-padded index at the prompt (hard rule 8)
read -p "Next sessions-log No: " SLNO
printf '%s\n' "| $SLNO | DATE-YYYY/MM/DD | topic | subject | title | gbrain-page-name |" >> /mnt/c/mybizz/logs/sessions-log/sessions-log.md
```

### M-3b — DEVELOPER TERMINAL BLOCK 1b (review resolution)

One promote per returned memory_id (world-visible content routes to
needs_review by design, trap T3; the promote is the developer's executing
act on content already approved at the gate). The developer pastes Block 1's
output back; the agent composes this block FULLY LITERAL from the returned
ids — one curl per memory_id, zero placeholders:

```bash
cd /home/dev-p/memory-governor && set -a && source .env && set +a

curl -s -X POST http://127.0.0.1:8321/memory/LITERAL-MEMORY-ID/review \
  -H "X-Governor-Credential: $MEMORY_GOVERNOR_WRITER_MANUAL_SUBMISSION" \
  -H "Content-Type: application/json" \
  -d '{"decision": "promote", "reason": "approved at the DATE close-out gate", "reviewer": "dev-p"}'
```

### M-4 — agent: verify the flush, capture fact numbers

- `grep "remembered as fact" /tmp/memory-governor-worker.log | tail -10` —
  the authoritative record of what landed and its fact numbers.
- Confirm with recall greps (world facts surface to the agent).
  **Trap T2:** if the worker log confirms the write but recall returns
  empty, the facts exist — record the numbers from the worker log; NEVER
  re-submit. (Agent-side recall is remote-classified and filters private
  facts; `include_private` may not pass through — a known 0.46.1.0
  property, recorded in the skill learnings 2026-09-14.)
- Any candidate still `under_review` → report plainly with its memory_id
  (DONE_WITH_REVIEWS_PENDING); the developer resolves via the review
  endpoint — never the agent. The review queue must be empty before the
  close-out ends (open review items block PDLF steps system-wide).

## Phase F — agent: stage the commits, hand the commands

- `git add` explicit paths only — the folders/files this session actually
  touched (suite folders, source changes, `facts/` files, the session
  page). NEVER `-A`.
- Leave anything that is not this session's work unstaged and FLAG it
  (unexplained file deletions, untracked artifact dirs such as `.sources/`,
  pre-existing drift) — the developer decides; the agent never silently
  commits or restores them.
- Separate commits where repositories differ (e.g. `mybizz-os-docs`
  master-explainer update is always its OWN commit — standing rule). The
  agent never commits — the exact `git commit` / `git push` commands go to
  the developer (Phase G).

## Phase G — DEVELOPER TERMINAL BLOCK 2 (facts log + commits)

```bash
# facts-log rows — one per saved fact (advisory and governed alike), with the
# real fact-NN captured in Phase C-A / M-4. The No. column is the LOG's own
# zero-padded index (hard rule 8): the developer enters the next index once
# and the helper increments per row. The agent drafts each frow line fully
# literal; the fact-NN goes ONLY in the last column (trap T9)
read -p "Next facts-log No: " N
frow() { printf '%s\n' "| $(printf '%03d' "$((10#$N))") | $1 | $2 | $3 | $4 | $5 |" >> /mnt/c/mybizz/logs/gbrain-logs/facts-log/facts-log.md; N=$((10#$N+1)); }
frow "YYYY/MM/DD" "subject" "title" "fact text" "fact-NN"
# (one frow line per saved fact)

git -C WORKING-REPO commit -m "close-out message" && git -C WORKING-REPO push
# additional repos/commits as the session's work requires, e.g.:
git -C /mnt/c/mybizz/mybizz-os-docs commit -m "memory: master-explainer - TOPIC (close-out)" && git -C /mnt/c/mybizz/mybizz-os-docs push
```

## Phase H — agent: sync + verify (only after push is confirmed)

```bash
setsid bash /mnt/c/mybizz/scripts/sync-gbrain.sh > /tmp/sync-output.txt 2>&1 &
```
Then verify `/tmp/sync-output.txt` ends `=== gbrain sync complete ===` and
report pages added / embed state (0 stale = fully embedded).

## Phase I — final report

Completion report: every step's verified outcome (facts + numbers, logs,
commits + hashes, sync), artifacts, flagged anomalies for developer
decision, and the next-work pointer. Then the session is closed.

---

## Guard classes a close-out meets (expect them; never fight them)

| surface | class | disposition |
|---|---|---|
| `~/memory-governor/.env` sourcing (credential) | secrets guard — permanent, no grant | developer terminal only (M-3) |
| file-tool read/write of protected-path `.md` (e.g. sessions-log, opening-and-closing.md) | sensitive-file — permanent | developer reads/appends; this prompt self-contains the formats |
| bash or file-tool write outside the current project | per-occasion slip | request `mg-grant --path <folder> --count N`; if denied anyway → developer block, never retry |
| command text containing guarded tokens (e.g. `cupcake`) | token-literal — permanent | approved glob forms where one exists; otherwise hand to developer |

## Known traps (all hit live 2026-09-14)

- **T1 — dedup swallows visibility corrections** *(Branch M)*. A
  `project_local` submission that later gets a `shared_all` twin is
  deduplicated by the worker ("already knew this — kept fact #NN") and the
  world version NEVER lands — the fact stays private and
  agent-unrecallable. Always submit the final intended visibility
  (`shared_all`) as the first and only submission.
- **T2 — recall may not surface saved facts** *(Branch M)*. Agent-side
  recall (remote-classified MCP surface; CLI call path) filters private
  facts, and `include_private` may not pass through. The worker log
  (`/tmp/memory-governor-worker.log`) is the source of truth for existence
  and fact numbers. Never re-submit on an empty recall.
- **T3 — needs_review is designed behavior** *(Branch M)* for
  world-visible (shared) content — decision-chain classification
  `medium_risk_moderate_confidence` ("any sharing"). Resolve via
  `POST /memory/{memory_id}/review` with the developer's decision — never
  by re-submitting with a different confidence.
- **T4 — open review items block PDLF steps system-wide** *(Branch M)*.
  The review queue must be empty before the close-out ends.
- **T5 — credentials never enter the agent environment** *(Branch M)*.
  Reference by variable name only; never printed, logged, or copied into
  any artifact.
- **T6 — the sessions-log row is presented at the gate** (Phase B), so the
  single approval covers it; appending is developer-run (M-3) because the
  agent-side write to the logs is boundary-blocked.
- **T7 — grants decrement per write.** Request the count the work actually
  needs before starting (live lesson 2026-09-14: a `count 1` grant consumed
  by the first write forced a second request).
- **T8 — angle-bracket placeholders do not survive pasting** *(M-3, live
  2026-09-15)*. A block pasted with `<memory_id_1>` unreplaced made bash
  redirect into files named `memory_id_1` and silently skipped the review
  promotes. Rule: Block 1 contains only pre-known, agent-filled values;
  anything that exists only in a previous block's OUTPUT goes into a
  SECOND, fully literal block composed by the agent from the pasted output
  (M-3b).
- **T9 — two numbering spaces, never conflated** *(live 2026-09-15)*. The
  logs' `No.` column is the log's own zero-padded index (001-style); the
  gbrain fact number (`fact-NN`) belongs only in the GBrain reference
  column. The agent never knows the next index (cannot read the logs —
  sensitive class) and never guesses one: `read -p` at paste time (M-3 /
  Phase G) or the developer supplies it at the gate.
- **T10 — padded-indices map to octal; `frow` must force base-10** *(live
  2026-09-15)*. The `read -p` captures a zero-padded index like `018`;
  passing it bare to `printf '%03d'` — and to `$((N+1))` — treats the
  leading zero as an octal literal, and `018` is invalid octal (the digit
  8). Result: `printf: 018: invalid octal number`, the substitution
  collapses to empty, and every row appends an index of `001`. Fix: force
  decimal on both — `$(printf '%03d' "$((10#$N))")` for output and
  `N=$((10#$N+1))` for the increment. When this still slips (a mangled
  paste), the corrective edit to facts-log.md is agent-run under an
  `mg-grant --path /mnt/c/mybizz/logs/gbrain-logs/facts-log --count 1` slip —
  not a re-paste of the same block.

Completion status per the mb-submit-memory protocol (Branch M runs): DONE,
or DONE_WITH_REVIEWS_PENDING (list every open memory_id), or BLOCKED (state
exactly what stopped the run and what was tried). Never mark DONE with an
unverified step.
