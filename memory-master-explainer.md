---
document: "The Memory Ecosystem — Master Explainer"
doc-id: memory-master-explainer
state: Live
date-created: 2026-09-05
---

# The Memory Ecosystem — Master Explainer

## 1. What This Actually Is

There is no single memory store here, and there is no set of parallel, interchangeable tools. There is one memory ecosystem, and understanding how its pieces act on each other — not what each piece is in isolation — is the entire point of this document. Each component has its own suite of documents (explainer, reference, learnings, canonical skill) that owns its internals in full depth; nothing here re-explains them. This document owns the integration: how the components form one memory system, how the system actually functions as a whole, what its nuances and caveats are, how to use it, when to use it, and what runs by itself versus what a human must do.

The system is organised around **two storage layers**. The **mirror layer** is a faithful, constantly-refreshable reflection of the real project files — pages, documents, code — kept current by the sync process, and deliberately unchecked, because its authority comes from being a faithful mirror of what already exists. The **durable layer** is memory meant to be *believed* later: facts that survive and are trusted, each with provenance and a recorded confidence. Only the durable layer is gated, because only it claims to be believed rather than merely stored.

**The cast of actors, in one line each.** GBrain is the store itself — it holds both layers and answers questions about them. The **Dream Cycle** is GBrain's own deepening process, turning stored content into more structured understanding. **Memory Governor**, a gatekeeping service with its own API, decides whether a newly generated candidate deserves to become durable memory at all. **mb-submit-memory** is the sanctioned front door of that gate — the way anything read from a real file reaches the decision. **Graphify** completely independently knows what the code structurally is, via deterministic parsing. **PostgreSQL** is the physical foundation underneath the store and the gate. **Cupcake** is a mechanical enforcement layer beneath everything, inspecting every agent tool call against compiled policy. Of these, PostgreSQL and Cupcake are *indirectly* related to the memory work itself — they hold it up and police it, but they are not memory mechanisms; the other five are the memory mechanisms proper. The Governor's API is part of Memory Governor, not a separate member.

One thing is deliberately *outside* the ecosystem: the **memory functionality audit** (`memory-functionality-audit.md` in this repo) is an audit *of* this ecosystem, so it lives outside it — a system should not audit itself. It is referenced here where its role is needed and never treated as a family member.

---

## 2. The Map — Components and Their Documentation

| Component | Role in the ecosystem | Layer served | Runs | Documentation (suite folder) |
|---|---|---|---|---|
| GBrain | store and question-answerer; holds both layers | both | always-on query surface; sync invoked | `gbrain/` |
| Dream Cycle | autonomous deepening of stored content | durable growth | manual-on-demand | `gbrain-dream-cycle/` |
| Memory Governor (+ API) | durability gate: promote or review | durable | fully self-starting processes | `memory-governor/` |
| mb-submit-memory | front door to the gate for real content | durable | manual, deliberate | `mb-submit-memory/` |
| Graphify | deterministic code-structure truth | mirror-derived | manual-on-demand | `graphify/` |
| PostgreSQL | physical foundation (two instances) | substrate | boot-durable | `postgresql/` |
| Cupcake | mechanical policy enforcement | substrate | every tool call | `cupcake/` |

Every component is managed under the same config-management pattern — its spec, exam plan, exam report, log, learnings, and exam skill — described as a system in Section 7.

**Where each component's documentation lives.** For every component the four documents to reference are the same: the **explainer** (what it is and why), the **reference** (procedures and rules), the **learnings** (transient exam knowledge), and the **canonical skill installed in WSL** (`~/.config/opencode/skills/` — the installed `SKILL.md`, the authoritative procedure — which lives in WSL, not in these documentation folders). For infrastructure components the canonical skill is the component's installed exam skill; for `mb-submit-memory` and `graphify` the canonical skill is the subject skill itself.

- **GBrain** — `C:\mybizz\mybizz-config-docs\gbrain\: `gbrain-explainer.md` · `gbrain-reference.md` · `gbrain-learnings.md` — canonical skill: `~/.config/opencode/skills/mb-config-gbrain/SKILL.md`.
- **Dream Cycle** — `C:\mybizz\mybizz-config-docs\gbrain-dream-cycle\: `gbrain-dream-cycle-explainer.md` · `gbrain-dream-cycle-reference.md` · `gbrain-dream-cycle-learnings.md` — canonical skill: `~/.config/opencode/skills/mb-config-gbrain-dream-cycle/SKILL.md`.
- **Memory Governor** — `C:\mybizz\mybizz-config-docs\memory-governor\: `memory-governor-explainer.md` · `memory-governor-reference.md` · `memory-governor-learnings.md` — canonical skill: `~/.config/opencode/skills/mb-config-memory-governor/SKILL.md`.
- **mb-submit-memory** — `C:\mybizz\mybizz-config-docs\mb-submit-memory\: `mb-submit-memory-explainer.md` · `mb-submit-memory-reference.md` · `submit-memory-learnings.md` (note the irregular name) — canonical skill: `~/.config/opencode/skills/mb-submit-memory/SKILL.md` (the subject skill itself).
- **Graphify** — `C:\mybizz\mybizz-config-docs\graphify\: `graphify-explainer.md` · `graphify-reference.md` · `graphify-learnings.md` — canonical skill: `~/.config/opencode/skills/graphify/SKILL.md` (the subject skill).
- **PostgreSQL** — `C:\mybizz\mybizz-config-docs\postgresql\: `postgresql-explainer.md` · `postgresql-reference.md` · `postgresql-learnings.md` — canonical skill: `~/.config/opencode/skills/mb-config-postgresql/SKILL.md`.
- **Cupcake** — `C:\mybizz\mybizz-config-docs\cupcake\: `cupcake-explainer.md` · `cupcake-reference.md` · `cupcake-learnings.md` — canonical skill: `~/.config/opencode/skills/mb-config-cupcake/SKILL.md`.

---

## 3. How Knowledge Moves — The Write Paths

There are five ways content gets written into the memory system, and they differ in what they write, whether they touch the gate at all, and what they guarantee. The governing rule is clean: **the mirror layer is written by sync, ungated, because fidelity to real files is its authority; the durable layer is gated, because being believed is what it claims.**

**Path 1 — sync writes the mirror layer.** Bulk content — project pages, documents, code — is synchronized into GBrain, chunked, and embedded, becoming searchable without any gate. The sync process also triggers the `/sync-gbrain` integration surface in OpenCode, which wraps sync and the dream trigger and maintains a shared search-guidance block in project files. One real trap lives here: the orchestrator derives its own source identity and collides with manually registered sources — the direct command (`gbrain sync`/`gbrain dream --source <id>`) against the real source avoids that collision by construction. Sync-into-governance is prohibited by design: corpus indexing was never the governed layer's job.

**Path 2 — the governed spine — is how content becomes *governed* memory**: a real file read by `mb-submit-memory`, a genuine judgment formed about it, an explicit approval, a deterministic decision, and a dual, verified write. That path runs: **`mb-submit-memory** reads a real file, queries GBrain for related material, reads what comes back, forms a genuine confidence and contradiction judgment — never a placeholder — and pauses for an explicit yes before anything is submitted. Every score is honestly tagged `model_based` — an agent's reading-comprehension judgment, weighable downstream, never dressed up as human-verified. The skill refuses to force a submission past review and blocks loudly if the gate's API is down — the review queue exists precisely so nothing becomes permanent unexamined. **Memory Governor's API** then authenticates the submission by the writer's cryptographic credential — identity is derived server-side from the authenticating credential, never from anything the request claims, which is what closes the class of error where a writer's own self-report is trusted. Its decision engine is fully deterministic — a candidate is either promoted straight through or routed to the developer's review list. There is no automatic rejection and no automatic quarantine; those are dispositions the developer chooses when actually resolving a review item, never something the engine decides unilaterally. A review item is genuinely blocking — it cannot reach a durable state before an explicit developer decision — and it carries a recommendation, never a decision already made. Contradiction checking runs for every candidate, every memory type: a detected contradiction is a flag, not a verdict — a deliberate later change is handled through atomic supersession and doesn't block; a genuinely unexplained disagreement routes to review; cross-project contradictions always route to review regardless of confidence. **The worker** then delivers each promoted memory: a fact written into GBrain, stamped with its owning project, with visibility mapped from declared scope, and a second write — a human-readable index row plus the full JSON payload — committed to git. These two systems (PostgreSQL and git) cannot be committed atomically together, so the register-and-payload write is asynchronous — a queued, retryable consumer of the same outbox, so a partial failure never blocks or loses the promotion silently: it retries with backoff, and after five consecutive failures it surfaces rather than disappearing.

**Path 3 — PDLF's step-completion writer** submits through the same gate, under its own credential, automatically, as part of its routine. It is parked while PDLF runs no real steps; when it does, the pattern is identical to Path 2 with a different writer identity.

**Path 4 — the Dream Cycle** is the one structural exception to "every writer is governed through the gate." GBrain's dreaming process has no interception point of any kind — no hook where a gate could attach — so its output reaches durable, queryable memory unreviewed. That limit is permanent and accepted. The compensation is procedural and always applied: every real run is followed by a suspected-contradictions evaluation whose findings are reviewed. The cycle itself is a fixed sequence of phases — housekeeping first, then extraction, then synthesis and pattern-finding, then judgment and calibration, then maintenance and closure — and it is deepens rather than ingests; it runs only when invoked, deliberately.

**Path 5 — the ungoverned quick path** is real and always present: raw `remember`-style writes through the memory tool surface exist in every session, ungoverned, with nothing between the generating model and durable memory. It was built knowing this path would always exist. The gate exists because the easy path is real; the discipline that routes durable submissions through the gate — and reserves the quick path for genuinely disposable writes — is documented, not mechanical.

Each path has its cost. Sync costs nothing and guards nothing. The governed path is slow and deliberate per file because one confidence score standing for a folder of varied content is exactly the placeholder the gate exists to prevent. The Dream Cycle costs real provider calls and real review. The quick path costs nothing and guarantees nothing. Choosing the path is choosing what the content is allowed to claim.

---

## 4. How Questions Are Answered — The Read Paths, and the Authority Model

Reading is where the two independent mechanisms genuinely cooperate. A question answered from one source is answered well only up to that source's ceiling; the authority model below orders what wins when they disagree — but the reason to understand both mechanisms is that a question is best answered from both.

GBrain's search is **hybrid**: literal keyword matching combined with vector similarity against embedded content, so two pieces of text with similar meaning end up mathematically close even when they share no words in common, and an optional query expansion generates related phrasings the literal words wouldn't have matched. Its universe is scoped by **sources** — a query is scoped to its own project's sources by default, and reaching across into another registered source requires naming it explicitly, except for the sources marked federated, which participate in every other source's search by default. Its content is advisory: nothing that builds, deploys, or enforces ever reads it — it only ever answers when asked, and if it disagrees with a real file, the reader defers to the file. Within GBrain's own content there is an informal trust tier: sourced-document content (mirror-of-reality pages) carries more weight than synthesized concept pages (pages GBrain itself reasoned into existence).

Graphify answers differently: it reads real code structure deterministically, building a literal graph where nodes are things that exist in the code and edges are typed relationships — calls, imports, contains, inherits — produced by structural parsing with zero ambiguity. Every edge carries a label: **EXTRACTED** means the parser literally saw the syntax — the trust of reading the code yourself; **INFERRED** means a relationship concluded probable without literal syntax; **AMBIGUOUS** means unresolved. Treating EXTRACTED and INFERRED the same is the single easiest way to get a wrong answer with high apparent confidence. Graphify also derives structural truths — dense communities that reveal real coupling that doesn't match the directory layout, and god nodes whose blast radius is demonstrable — and the confidence label, the freshness of the graph against the current commit, and the recurring traversal trap: its graph is stored undirected but path-finding defaults to directed, so a real relationship can come back "no path found" purely from traversal direction.

The **authority model** orders everything:

1. **Current repository files, explicit user instructions, and tests** — always first, unconditionally.
2. **Graphify's EXTRACTED edges are trustworthy as read; INFERRED or AMBIGUOUS edges need independent verification** before anything consequential rests on them.
3. **Graph provenance must match the current commit** — a stale graph describes a codebase that no longer exists.
4. **Within GBrain, sourced content outranks synthesized content** — ADR-tier, document-derived material over concept pages the brain reasoned into being.
5. **Memory Governor decides what reaches durable GBrain memory** — a generated fact is not automatically permanent knowledge.
6. **Neither mechanism ever replaces actually looking** — both accelerate the path to the real source; neither is a substitute for reading it once there.

Read and the model together: a question answered by both at the top of each tool's own trust scale, agreeing, is a settled answer. A lower-confidence signal — an INFERRED edge, a synthesized page — downgrades the agreement to a lead worth verifying. Section 10 shows it live.

---

## 5. What Holds It Up — The Substrate

Everything above rests on **PostgreSQL**, and underneath everything sits **Cupcake**.

PostgreSQL is two independent installations, not one environment wearing two hats. The memory-relevant facts: two servers, on two sides of the WSL boundary, holding the brain (`gbrain` database above all registered sources), the gate's own bookkeeping (`memory_governor` database: projects, candidates, policy versions, the event table and the audit log), and — on the *other* side — the PDLF build ledger, unrelated to memory. All table-access uses row-level security, scoped per instance. The operational trap is permanent and must be carried: both servers listen on the same port, one from each side of the WSL boundary, so a call made from the wrong side silently connects to the wrong server — every database check names its instance explicitly.

The gate's delivery architecture replaces a message broker with a **transactional outbox**: any state change that must trigger downstream processing inserts an event row in the same transaction as the change and notifies the worker; the worker holds a persistent listen and wakes near-instantly, polling as a safety net — one mechanism is both the durable event log and the queue. The second, independent copy of every governed memory — the index row plus the full payload — is a git commit and push, giving genuine disaster recovery twice over (the index alone cannot reconstruct a lost record; only the payload can). On failure, delivery retries with backoff and after five consecutive failures surfaces the failure rather than blocking the promotion or retrying silently. Backups are a second line of defense for content that exists only in the database — the weekly PostgreSQL dump, alongside a periodic export of dream-synthesized concept pages which have no backing file at all.

Cupcake is not a memory component — it is the enforcement layer that polices the memory components and everything else. It intercepts every agent tool call before execution and mechanically allows or denies it against compiled policy — a written rule in `AGENTS.md` is a request the agent might not follow; a Cupcake rule is checked every single time, regardless of what the agent believes or agrees to. The rules that matter to the memory ecosystem directly: destructive-database guards that protect the memory databases themselves; guidance-file guards that protect the shared `AGENTS.md`/`CLAUDE.md` block in which GBrain and Graphify both write persistent guidance — a single combined, marker-delimited block, owned by one orchestrator, so two independent writers never fight over the same section; an **exec-protection** rule that forbids agents from executing the Cupcake binary itself — which is why live tests of any new rule must be run by the developer, never by the agent whose behaviour the rule exists to police; and a network rule that **always allows loopback** — the gate's own submission traffic runs on the machine's internal address, so anything less would mechanically block the governed pipeline's own delivery. Every rule is compiled from a plain-language source document into real policy through a proven pipeline (`mb-compile-rules`); a rule is never trusted because it was written carefully; it is trusted because it was proven, live, against the real engine it operates in.

---

## 6. Autonomous vs Manual — The Definitive Ledger

The single most useful answer this document gives: what the memory ecosystem does by itself, and what a human or agent must do every time. No component explainer can own this — it is visible only from the whole.

**Runs on its own.** The Governor's two processes — the deciding API and the delivering worker — are permanent auto-start services with restart-on-failure, so the memory system's running state survives restarts of the PC, OpenCode, and WSL, independently of anyone remembering to start them; a gate that quietly closed is a failed state, and the system no longer has one. Delivery is autonomous: the worker wakes on notification, drains the outbox, retries with backoff, surfaces real failures. PostgreSQL starts its workloads at boot. Cupcake fires on every tool call, mechanically, without being asked. Search federation makes the federated sources reachable everywhere without per-query action. All of these are *configured once, running forever* behaviour.

**Requires invocation, every single time.** Sync (the mirror refresh) runs when invoked, by an agent or the developer — nothing syncs itself. The Dream Cycle runs only when invoked — deliberately: it is the one writer whose phases can fail silently, and an unattended deepening pass that hides its own failure would compound damage until a later run found it; on-demand invocation means a human is present at every run, and the mandatory post-run verification procedure is applied to every run. `mb-submit-memory` is a deliberate run — a pausing, per-file cycle, never ambient. The config-management exams run per component, on a cycle. The whole-system audit runs when the developer asks, or after any restart or repair. PDLF's step-completion writer will submit automatically once PDLF runs real steps, but is parked until then.

**Requires the developer, always.** Anything credentialed — the agent's environment-sourcing of secrets is a proven permanent denial class, so credentialed checks fall to the developer by design. Live testing of a Cupcake rule — the agent is forbidden to execute the binary it exists to police. Upgrades. Protected-path operations. The review queue's dispositions — the review queue exists precisely so that judgement is the developer's, by construction.

---

## 7. Configuration Management — How the Ecosystem Stays Healthy, Self-Healing, and Self-Aligning

Every component in the family is under standing configuration management, and every component is examined by the **same repeating cycle** — the architecture standard lives in the `mb-config-manager` suite. Each component carries its own spec, exam plan, exam report, log, and learnings, plus its own installed exam skill, and the cycle is invariant across them:

- The **spec** states the ideal state — what "working and correct" means for that component.
- Before each run, the cycle synthesizes the **exam plan — a fresh job-spec built from the spec, the last report, the log, and the learnings, carrying forward what was deferred, addressing what erred, deriving probes from new events, folding in lessons not yet graduated. The plan is transient.
- The **exam** runs with the plan in hand. **Detection is fully autonomous; healing is gated** — the only agent-healable items are suite-folder file repairs; every repair to a system's own internals is developer-run or proposed through the report.
- The run rewrites the **exam report** (current state, no history), appends one row to the **log**, files new **learnings**, and prunes absorbed entries.

**Self-aligning.** The learnings → plan → exam loop is the alignment mechanism: an incident becomes a learning; a learning folds into the next plan; the plan sharpens the exam that guards against it. Each exam watches its spec for drift — a spec that silently drifts from reality is flagged by its own exam. Scope is disciplined between adjacent exams — each exam owns its component's configuration state and cross-references its neighbours without re-probing them, so there are no overlaps and no gaps. The master explainer is itself part of the alignment: it is the cross-component coordination point, passed to every memory-component run and updated by that run to reflect current state, committed as its own update at that run's close-out — so the map never goes stale. And the one thing deliberately outside the ecosystem — the audit — closes the loop by auditing the whole, end-to-end, on demand: it runs the real governed pipeline once, remediates what is remediable in-session, and reports anything it cannot.

**Self-healing.** Restart-on-failure services mean processes come back without anyone noticing they were gone. Delivery retries with backoff and surfaces after repeated failure rather than silently retrying forever. The enforcement layer runs fail-open on evaluation crash — configured so a broken evaluation can arrest nothing, only logged and allowed, fix at leisure, and the exam catches the silent-degradation class between runs. The honest boundary is the shape of all of it: processes restart themselves, delivery retries itself, exams catch drift and silent failure — but credentials, upgrades, protected paths, and any repair to a system's own internals require the developer. The ecosystem aligns itself as far as mechanics allow and proposes the rest.

---

## 8. How, When, and Why to Use It

**When you want a fact to be believed later**, use the governed path — `mb-submit-memory` to the gate. That is the only path that confers governed status — verification before the fact, a recorded provenance, a committed register — which is why it exists.

**When you want content findable**, use sync — the mirror layer — for bulk, ungated, faithful. The mirror is searchable because it mirrors reality.

**When you want code-structure truth — what calls what, what imports what, where the blast radius spreads** — use Graphify, which answers deterministically from syntax.

**When you want a deepened understanding formed out of what you already have — concepts across documents, patterns, contradictions surfaced** — run the Dream Cycle, and immediately run its post-run verification.

**When you want to know the whole system is healthy, or after any restart or repair or suspicion** — run the audit.

**When you are unsure whether something is already known** — ask GBrain first, because the governed path's judgment before any submission depends on it.

The "why of each path is the guarantee that path uniquely provides: the governed path guarantees provenance and a human gate; sync guarantees fidelity; Graphify guarantees determinism; the Dream Cycle guarantees depth; the audit guarantees function. Choose the path by which guarantee you need, never by which is easiest, because the path is what the content is allowed to claim.

---

## 9. Nuances That Only Exist at the Ecosystem Level

**The name coupling.** A submission names a project, and the gate's background writer hands the outcome to GBrain by that project's name. The registry of projects the gate knows and GBrain's own list of sources are separate systems that drift independently — a mismatch means approved memory can silently fail delivery. The credentialed exam check is the only close watch on it.

**The startup coupling.** The gate reads its configuration and credentials from the environment at the moment it starts, not from files it hunts for. Started carelessly it runs but cannot reach anything — every submission fails authentication. There is one correct start procedure.

**"Completed" is not "worked."** The Dream Cycle reaching its end says nothing about whether each phase did meaningful work — a phase can fail and report a bare count, or be blocked by a missing credential and report nothing — and the cycle still exits cleanly. The verification procedure exists because of this.

**The silent-failure class is a family trait.** A phase reporting a bare failure count; an enforcement layer silently degraded rather than off; a gate quietly closed whose own submissions 401. Each is exactly what its standing exam exists to catch. The measure of the ecosystem is not that failures happen, but that none of them are silent for long.

**Two servers, one port.** PostgreSQL on each side of the WSL boundary listens on the same port, and a call made from the wrong side silently connects to the wrong server. Every database check names its instance explicitly.

**The brain is brain-global.** GBrain's facts layer has no native source scoping — an unscoped recall finds any fact from any context. Governed-memory isolation is therefore enforced by the gate itself: every governed fact is stamped with its owning project, and visibility is mapped from declared scope, so project-scoped recall is entity-scoped recall. This is real, enforced, within the platform's capabilities — never papered over.

**The ungoverned quick path is real and deliberate.** The memory tool surface exists in every session, enabled; the gate was built knowing it would. The discipline that routes durable submissions through the gate is documented, not mechanical.

**MCP servers start once, at process bootstrap.** Changing a memory-system's MCP entry while OpenCode is already running writes correctly to disk and does nothing observable until the process is fully quit and relaunched — no error, no warning, silent absence in the meantime.

**Local-only by design.** The shared guidance block, the MCP entries, the scaffolded skills live outside version control, local to one machine, not traveling with the repo — which means none of it reproduces automatically on a fresh clone. Deliberate, reversible, accepted.

**Supersession, not erasure.** A deliberate later change correctly replaces an earlier fact through atomic supersession; a resolved contradiction doesn't block. What is superseded is never silently rewritten into silence.

**A flag is not a verdict.** A detected contradiction only means two things disagree — it is the developer's disposition that resolves it.

**Self-observation allowed; self-modification not.** The gate may notice patterns in its own decision history and propose a policy change — but a policy change is high-risk content, so its own proposal still has to pass through its own ordinary review pipeline before anything about its actual behaviour can change. The system can propose a change to how it operates; it can never apply one to itself.

---

## 10. Two Worked Examples

**Read-path agreement.** Ask GBrain and Graphify the same question about a real component — say, the Vault's secret-encryption mechanism. GBrain's answer draws from ADR-tier, sourced documents, not synthesized concept pages — the higher of GBrain's two trust tiers. Graphify's answer returns neighbor relationships — calls, imports, containment — every edge labeled EXTRACTED, the highest of its three confidence labels. Both answers sit at the trustworthy end of their own scales, and they agree. That is the load-bearing point: not that two tools were asked the same question, but that each answer sat at the top of its own trust scale and the conclusions matched. Had either been lower — an INFERRED graph edge, a synthesized GBrain page — the same agreement would carry meaningfully less weight, and would be a lead worth verifying rather than a settled answer.

**The governed write, end to end.** A real file is read by `mb-submit-memory`. It queries GBrain, reads what comes back, forms a genuine confidence and contradiction judgment — honestly tagged `model_based — and pauses for an explicit yes. Submission arrives at the gate's API, authenticated by the writer's credential; the wrong or absent credential is rejected before any decision (the rejection itself is the auth gate working). The engine decides: nothing triggered, promoted; something triggered, review — never rejection. The worker wakes, writes the fact into GBrain stamped and visibility-mapped, and commits the index row plus the full payload to the register. The audit procedure replays exactly this pattern deliberately, with a disposable candidate and mandatory cleanup, as its end-to-end proof.

---

## 11. Current State, Open Questions, and the Maintenance Rule

The whole memory family is under standing configuration management with completed exam cycles; the Governor's permanent auto-start is installed and active — both processes self-healing; the API's standalone operation is confirmed — listening on its loopback port, auth-gating credential-less requests, accepting credentialed calls; backups run on the standing weekly routine. Everything in this document states current state, and the standing rule keeps it so.

**The maintenance rule (developer-stated, standing): this document is passed to every memory-component run and must be updated by each such session to reflect current verified state. It is the cross-component coordination point, and a stale version defeats its purpose. At that run's close-out, the update is committed and pushed to `mybizz-os-docs` as its own commit — standing authorization, not re-decided per run.

**Genuinely open, system-level decisions.** Whether the shared guidance block and Graphify's scope configuration should ever be committed to travel with the repo, versus staying local-only. Whether the remaining governance-conflict design (rules that block by default where conversational approval is invisible) stays as accepted friction or is revisited. The standing upgrade policy: never upgrade on newness, only on check failure. Graphify's git-hooks decision is a tripwire, contingent on the graph output staying gitignored; re-examine explicitly if that ever changes.

---

*Companion documents: the full memory-system map lives here; each component suite's explainer, reference, learnings, and canonical skill (installed in WSL) are listed per component in Section 2. The audit of this ecosystem: `memory-functionality-audit.md` (this repo) — deliberately outside the family, by design. The architecture standard: `mb-config-manager/config-manager-explainer.md` (mybizz-config-docs).*

*End of document*