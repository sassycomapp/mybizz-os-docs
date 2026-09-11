---
document: Deferred and Unresolved Matters
state: Living reference — not a task list
location: mybizz-os
---

# Deferred and Unresolved Matters

## Purpose of This Document

A permanent register of matters that are **closed as active work** but
**not fully resolved in substance** — too valuable to discard, not
actionable enough to belong on `master-task-list.md`.

Every entry exists for one of two reasons, and every entry must state
which:

1. **Genuinely irresolvable by nature** — with the specific reason
   stated.
2. **Deliberately, intentionally deferred** — with the specific reason
   for the deferral stated.

**This is not a task list.** Nothing here implies "this still needs to
be done." If an item needs doing, it belongs in `master-task-list.md`
instead.

**Growth:** new entries are added over time as further qualifying
matters are discovered. No fixed template governs what can be added —
only the two qualifying conditions above.

**Standard entry shape**, used consistently so a reader returning to
this document months later can understand any entry without needing to
reconstruct the conversation that produced it:

- **What it is**
- **Why unresolved or deferred**
- **Where else to look** (if the fuller, current-state version of this
  subject lives in a canonical explainer/config/reference document
  elsewhere)

---

## Entry 1 — Compliance Architecture: Reference Material and Deferred Items

### What it is

The reasoning framework and set of deliberately-unresolved items
originally recorded in the retired `structural-compliance-enforcement-architecture-
v3.md`. That document's actively-current content (Cupcake's identity,
the rules-authoring pipeline, the cross-project grant system, the full
rule inventory) has since been folded into the canonical Cupcake
document set and is not repeated here. What remains, and belongs here,
is the general-purpose reasoning framework used to build that
architecture, plus the specific items it left open by decision.

**Threat model** — the five failure modes this compliance architecture
was built to address:
- Agent skips a required check by omission, not malice.
- Agent fabricates or asserts compliance without it being true.
- Agent performs a harmful action that never reaches a git commit.
- Compliance state is stale relative to current reality (TOCTOU).
- A control's *presence* is confirmed but its *logic* is broken or
  incomplete.

**Enforcement classification taxonomy** — a reusable way of describing
any control's real strength, not just Cupcake's:
- **Preventive hard control** — blocks the action before it happens.
- **Procedural control** — depends on consistent human application; no
  technical enforcement.
- **Detective control** — records or flags after the fact; does not
  prevent.
- **Advisory control** — informational only; nothing technical stops
  the agent from proceeding without it.

**Cross-system bypass and adversarial test methodology** — the practice
of testing a control not just for whether it works, but for how it
fails: attempting the exact bypass a control is meant to prevent, and
confirming either it's caught or the gap is honestly recorded as a
residual risk rather than assumed closed. Applied across every layer
together (Cupcake, git-boundary decisions, the ledger, the session-start
checklist) rather than one tool at a time.

**Evidence base** — a set of directly-sourced findings about OpenCode's
real plugin and permission behavior (cited GitHub issues, official
documentation), gathered specifically because earlier versions of this
architecture had inferred OpenCode's behavior from a different tool
(GStack) rather than verifying it directly. The general lesson —
verify a platform's real behavior directly rather than inferring it
from an adjacent tool — is the part worth retaining; the specific
citations are dated and not repeated here.

### Why unresolved or deferred

**`gbrain doctor` custom checks — deliberately deferred, indefinitely.**
Considered as a final-state validator for anything bypassing Cupcake's
tool-call-level coverage. Decision: not built, and not scheduled,
because no specific, currently-known gap exists to write a check
against. Building speculative checks against hypothetical cases risks
being ineffective against whatever the real, eventual gap turns out to
be, while still costing real effort now. The correct trigger for this
work is a real, identified case Cupcake structurally cannot cover —
at which point a targeted check can be built for that specific case,
not before.

**A cross-project grant's scope is the folder and the count, not the
originating task — accepted, not solved.** Once a folder is granted,
any tool call to it is permitted for the grant's duration, not only the
specific action that prompted the request. Time and count limits bound
the exposure; they do not eliminate this gap. No further mechanism is
planned — this is accepted as the cost of a workable design, not
tracked as an open problem.

**Semantic and architectural judgment rules — permanently
unenforceable, by nature.** Whether a design "reflects the five-app
model correctly," for instance, has no deterministic check. LLM-as-Judge
(Cupcake's own optional AI-evaluation feature) was investigated and
explicitly declined as a substitute — documented bias toward longer,
better-formatted answers regardless of correctness, a named adversarial
vulnerability (Rubric-Induced Preference Drift), and documented
inconsistency across repeated evaluations on identical input. This
remains permanently dependent on human review; no technical
replacement is being sought.

**The compliance ledger has no tamper protection — a closed decision,
not an open gap.** Reconsidered directly: the ledger is not
load-bearing in this design — nothing reads it to authorize any
action — so protecting it against tampering would add complexity
without addressing real risk.

**An unrecognized, differently-named spawn tool remains a narrow,
residual gap in sub-agent creation blocking.** Mitigated, not
eliminated, by the standing skill-review habit (checking any new skill
or plugin for spawn-tool capability before trust) — which depends on
consistent human application, with no automatic detection of an
unrecognized tool name.

### Where else to look

Cupcake's current, live implementation — rule inventory, the grant
system's real mechanics, known limitations, and the full incident
history behind them — lives in `cupcake-explainer.md`, `cupcake-
config.md`, and `cupcake-reference.md`. This entry retains only the
reasoning and the items those documents don't cover.

---

## Entry 2 — Memory Governor / PDLF Live-Integration Test

### What it is

The Memory Governor system itself is fully built, tested, and proven
live — every writer, the GBrain write path, disaster recovery, and
cross-project isolation have all been directly, independently verified.
What remains is a single, final proof: running one real PDLF step,
end-to-end, through the full governed pipeline in its natural setting,
confirming the whole chain genuinely works together in practice, not
just in isolated component tests.

### Why unresolved or deferred

**Deliberately deferred, not blocked or forgotten.** This test's own
exit criteria requires a real, running PDLF step — and PDLF itself does
not yet exist as a complete, running system. A narrower stand-in
(invoking the submission skill in isolation, outside a real step's
genuine context) was considered and explicitly rejected: it would only
prove the skill works alone, which is already established by every
other test already run — it would not prove what this test is actually
meant to prove. A weaker test wearing the same label was judged less
honest than genuinely waiting for the real one.

This rests on real, already-proven confidence, not blind faith: every
governed writer has been tested live, the GBrain write path has
survived two full disaster-recovery drills including genuine data loss
and reconstruction, and cross-project isolation was proven by
deliberately attempting a leak and confirming it could not happen. The
system's own build is complete; only its final live-integration proof
awaits PDLF's real existence.

### Where else to look

Memory Governor's full, current, built system — architecture,
mechanics, real incidents — lives in `memory-governor-explainer.md`,
`memory-governor-config.md`, and `memory-governor-reference.md`. The
recurring, per-step work of actually wiring each new PDLF step into
this system (which mechanism to call, when) is tracked on
`dev-makepdlf-todo.md`, not here — this entry concerns only the final,
one-time proof that the whole chain works together.
