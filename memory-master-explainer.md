---
document: "The Memory System — Master Explainer"
doc-id: memory-master-explainer
state: Live
date-created: 2026-09-05
---

# The Memory System — Master Explainer

## 1. Opening

There isn't one "memory system" here — there are four distinct mechanisms, each solving a genuinely different problem, and understanding how they actually work — not just what they're for — is the point of this document. Each tool's own explainer covers its own internals in full depth; this document exists to explain how the four fit together, and to walk through the nuances that only make sense once you understand more than one piece at a time.

The shape of it: **GBrain** remembers what was decided and why, via semantic search over a real database. **Graphify** knows what the code actually contains and connects to, right now, via deterministic parsing. GBrain's own **Dream Cycle** is an autonomous background process that deepens what GBrain already has, without a human asking first. And **Memory Governor** is a gate that decides whether something newly generated is trustworthy enough to become permanent memory at all.

None of these four replace each other, and none of them replace the one thing that outranks all of them: the actual repository files, on disk, right now.

---

## 2. GBrain — How It Actually Works

GBrain organizes everything it knows into **brains** (databases) and **sources** (repositories registered inside a brain). A single brain can hold many project sources at once — this brain currently holds thirteen (verified 2026-09-06). This matters because it shapes how a query behaves: by default, a query is scoped to its own project's sources; reaching across into another registered source requires explicitly naming it. Cross-source querying is possible, but not automatic — a query doesn't silently compound across everything GBrain has ever seen unless told to.

Four sources are marked **federated** — they participate in every other source's search by default, without being explicitly named: the global specification library (`default`), the division root hub, and the two division documentation repos (`mybizz-config-docs`, `mybizz-os-docs`). This is the mechanism by which shared, cross-project material becomes reachable from any project's query without every other source needing to name it individually. Every other source stays scoped to itself unless explicitly reached for.

**How search actually works, mechanically.** A plain keyword search matches literal text — fast, exact, and blind to synonyms or rephrasing. GBrain's hybrid search combines that with **vector search**: content is embedded — converted into a long list of numbers representing its meaning, via an external embedding model — and two pieces of text with similar meaning end up mathematically close to each other in that space, even sharing no words in common. The two result sets (keyword and vector) are then combined, and an LLM can optionally expand the original query first, generating related phrasings the literal words wouldn't have matched. This is why a query for "vault secret handling" can find a function called `get_vault_secret` even though the words don't overlap.

**What a schema pack actually governs.** GBrain doesn't extract everything from every source uniformly — a schema pack defines a set of *page types* (categories of content GBrain recognizes) and *link types* (categories of relationship between pages), and, critically, which of those types are actually *extractable*. A page type marked `extractable: false` in the active pack is never turned into structured, queryable content, no matter how much of it exists in the source material — it's indexed for plain search, but nothing more sophisticated is ever built from it. This single mechanic is the entire reason the code-symbol call-graph doesn't work through GBrain (see `gbrain-explainer.md` Section 6 for the full account) — it isn't a bug or a missing sync, it's a page type's extractability flag being false in every bundled pack.

**What "advisory" actually means in practice, not just in principle.** GBrain's content is never read by anything that builds, deploys, or enforces — it only ever answers a query when asked. There is no path by which GBrain's own stored belief about something can silently override or bypass an actual file. If GBrain says one thing and a real file says another, nothing mechanically prevents that contradiction from existing — it's on whoever's reading GBrain's answer to notice and defer to the file, which is precisely why the authority model in Section 7 exists as an explicit, stated ordering rather than assumed common sense.

---

## 3. Graphify — How It Actually Works

Graphify builds a literal graph: **nodes** are things that exist in the codebase — a function, a class, a module, a file — and **edges** are typed relationships between them — `calls`, `imports`, `contains`, `inherits`, and others. Every edge is produced one of two ways. **AST extraction** parses real code with Tree-sitter — a structural parser, not a language model — and produces edges deterministically, at zero cost and zero ambiguity, for anything the parser can actually see in the syntax. **Semantic extraction** covers content Tree-sitter can't parse structurally — markdown, HTML-as-document, images — and involves real LLM or vision-model work, with real cost and real uncertainty.

**What a confidence label actually encodes.** Every edge carries one of three labels. **EXTRACTED** means the edge came directly from parsing real syntax — the parser saw a literal function call, a literal import statement — and carries essentially the same trust as reading the code yourself. **INFERRED** means Graphify concluded a relationship probably exists without it being literally spelled out in syntax — for instance, relating two functions because they're mentioned together in a docstring, without a direct call between them. **AMBIGUOUS** means the extractor found conflicting or unclear signals and didn't resolve to a confident answer either way. These are not decoration — they're the difference between "this is what the code does" and "this is Graphify's best guess," and treating the two identically is the single easiest way to get a wrong answer with high apparent confidence.

**What a "community" actually represents.** After extraction, Graphify clusters the graph into communities — groups of nodes that are more densely connected to each other than to the rest of the graph. A community isn't an arbitrary folder-based grouping; it emerges from the actual structure of the calls and imports, meaning it can reveal real architectural groupings that don't match the literal directory layout — two modules in entirely different folders can end up in the same community if they call into each other heavily, which is itself a useful signal about coupling that isn't visible just from browsing the file tree.

**What a "god node" is, and why it matters.** A god node is a node with an unusually high number of connections relative to the rest of the graph — a function or module that a very large portion of the codebase touches, directly or indirectly. Identifying these matters because they're disproportionately risky to change: a modification to a god node has a wide, structurally-confirmed blast radius, and Graphify can name that radius explicitly rather than leaving it to intuition.

**Why directed-versus-undirected traversal is a genuine, recurring trap, not a one-off bug.** Graphify's graph is stored *undirected* — a relationship is recorded as existing between two nodes without inherent direction — but path-finding commands default to *directed* traversal. This means a real, existing relationship can come back as "no path found" purely because of which direction the search happened to walk, not because the relationship doesn't exist. This isn't a quirk to memorize once; it's a standing property of how the tool queries its own data, and needs to be accounted for every time a path or relationship query returns a negative result that seems surprising.

---

## 4. The Dream Cycle — How It Actually Works

The Dream Cycle runs a long, fixed sequence of phases, and understanding what the sequence actually does — not just that it exists — is where the real nuance lives. The confirmed phase order:

**lint → backlinks → sync → synthesize → extract → extract_facts → extract_atoms → resolve_symbol_edges → patterns → synthesize_concepts → recompute_emotional_weight → consolidate → propose_takes → grade_takes → calibration_profile → drift → conversation_facts_backfill → enrich_thin → skillopt → embed → orphans → schema-suggest → purge**

Grouped conceptually, rather than as a flat list:

- **Housekeeping first** (`lint`, `backlinks`, `sync`): the cycle starts by making sure its own structural bookkeeping — internal links, basic content hygiene, freshness against source repos — is correct before doing any deeper reasoning on top of potentially-stale foundations.
- **Extraction** (`extract`, `extract_facts`, `extract_atoms`, `resolve_symbol_edges`): this is where raw stored content gets turned into discrete, structured pieces — individual facts pulled out of longer documents, and an attempt at resolving which code symbols relate to which others. This is also the most provider-dependent stage, and the one most likely to fail silently — see `gbrain-dream-cycle-explainer.md` for the real incident this caused.
- **Synthesis and pattern-finding** (`patterns`, `synthesize_concepts`, `recompute_emotional_weight`, `consolidate`): having extracted discrete pieces, the cycle now looks *across* them — finding recurring patterns, generating higher-level concept pages that summarize many individual facts, and consolidating near-duplicate entity pages into one.
- **Judgment and calibration** (`propose_takes`, `grade_takes`, `calibration_profile`, `drift`): the cycle can propose its own interpretive conclusions ("takes") about stored content, grade its own confidence in them, and track whether its calibration is drifting over time — a form of the system checking its own reliability, not just accumulating more content.
- **Maintenance and closure** (`conversation_facts_backfill`, `enrich_thin`, `skillopt`, `embed`, `orphans`, `schema-suggest`, `purge`): filling in gaps in thinly-detailed pages, generating embeddings for anything new, identifying content that's become disconnected from everything else, suggesting schema improvements, and removing what's genuinely no longer needed.

**The nuance worth carrying forward:** a dream cycle reporting "completed successfully" tells you the sequence reached its end, not that every phase inside it did meaningful, correct work. A phase can silently produce zero results — as the call-graph resolution phase does by design, given the schema-pack limitation — while the overall cycle still exits cleanly. "Completed" and "worked" are not the same claim, and treating them as interchangeable is exactly how a real, extended silent failure went unnoticed for as long as it did (see the Dream Cycle's own explainer for that incident in full).

**Operational reality in this environment, verified 2026-09-06: the cycle runs only when invoked.** A `gbrain autopilot` cron schedule (every 5 minutes) was discovered completely inert — the cron daemon was not running, its target repo path predated the current machine layout, and its log file had never been created — and was deliberately removed; on-demand invocation is the confirmed design, which makes the post-run verification procedure in `gbrain-dream-cycle-reference.md` mandatory rather than optional. Two bookkeeping facts from a real completed run (2026-08-16 record, plus the 2026-09-06 acceptance run): a completed dream clears `cycle_freshness` for its source scope only when its sync phase actually processes new commits — a dream over an already-fresh brain skips sync and stamps nothing — and the gstack `/sync-gbrain --dream` orchestrator carries a real source-overlap trap (it derives its own source ID and collides with manually registered sources; bypass it with `gbrain dream --source <id>` directly). The 23-phase sequence above was live-verified in exact order by the 2026-09-06 acceptance run.

---

## 5. Memory Governor — How It Actually Works

Memory Governor exists to answer one question mechanically, rather than trusting a self-report: *should this newly-generated candidate actually become durable, queryable memory?*

**The automatic engine has exactly two possible outputs, deliberately narrow.** A candidate is either promoted straight through, or routed to the developer's review list. There is no automatic rejection and no automatic quarantine — those are dispositions the developer chooses *when actually resolving* a review item, never something the governor decides unilaterally. This was a deliberate correction from an earlier design that did permit automatic rejection; the current design's own stated principle is that the governor proposes, it never autonomously shuts a candidate out without the developer being told.

**Where confidence and contradiction judgments actually come from — and where they deliberately do not.** The governor enforces rules against a candidate's confidence score and contradiction status, but it never computes those values itself. Doing so would require exactly the kind of AI judgment the system's own founding principle rules out — a fully deterministic decision engine cannot also be the one forming the subjective judgment it then mechanically checks. Instead, the agent that actually did the work being remembered — the one with direct, contemporaneous knowledge of what it just did — is responsible for assessing its own candidate's confidence and checking it against existing memory *before* submission. The governor's job is to enforce rules against a judgment already made, not to make the judgment.

**Provenance is derived, never self-reported.** A candidate's `writer_identity` — who actually generated this — is never taken from the request payload itself. It's derived server-side from which per-writer credential authenticated the request; each real writer that actually submits through the governor's API — manual developer submission, PDLF's own step-completion process — holds a distinct credential. This closes a documented, real failure mode seen elsewhere in comparable systems: a memory platform observed writing an incorrect date into permanent memory by trusting the generating model's own current-date claim instead of the source document's actual timestamp. Deriving identity from the authenticating credential, rather than trusting a claim inside the payload, is what specifically prevents that class of error here.

**One real, important correction to the picture above: GBrain's own dreaming process is not actually one of the governor's writers, despite being the original motivating example.** Exhaustive investigation found that `dream` has no interception point of any kind — no redirect, nothing in its tool schema, nothing in its config. It cannot be routed through the governor's API, structurally, no matter how the governor itself is designed. It's governed instead through a separate, manual discipline — see `memory-governor-explainer.md` Section 9 for the full account. This is a genuine, permanent limit, not a temporary gap.

**Why a promoted memory writes to two places, not one, and why that write is asynchronous.** Every promoted memory produces a human-readable index row and a full JSON payload of the complete candidate, committed together — the index alone was found insufficient for genuine disaster recovery, since it can't represent the full content text, only the payload can fully reconstruct a lost record. These two systems (Postgres and git) cannot be committed to atomically in a single request — there's no way to guarantee both succeed or both fail together — so rather than coupling them directly into the promotion request itself and risking a partial failure, the register-and-payload write happens asynchronously, driven separately from the promotion decision itself.

**Self-observation is allowed; self-modification is not.** The governor may notice patterns in its own decision history — a particular source's candidates getting routed to review unusually often, and typically rejected once reviewed — and generate its own memory candidate describing that observation. But because policy and threshold changes are themselves classified as high-risk content, that observation still has to pass through the governor's own ordinary review pipeline before anything about its actual behavior changes. The system can propose a change to how it operates; it can never apply one to itself.

**The governor's processes, and steady state across restarts (documented 2026-09-08).** The governor runs as exactly two processes, both required: the **API** (`uvicorn app.main:app` on `127.0.0.1:8321` — decides every governed candidate) and the **worker** (`python -m app.worker` — a persistent outbox consumer that delivers every promoted memory: the GBrain write and the register/payload backup write). Both read their configuration from the environment at start (`MEMORY_GOVERNOR_DATABASE_URL` and the writer credentials from `~/memory-governor/.env`, which must be sourced before starting — the governor has no dotenv autoload), and both must be running for the system to function: an API without a worker means promoted candidates silently strand in the outbox, never reaching GBrain or the backup register. Both were brought into persistent operation on 2026-09-08 — the worker's first-ever persistent run (an entry-point defect had previously made it unstartable outside tests). The documented start procedure brings both up after any restart; a permanent auto-start installation (WSL systemd, two user units with `Restart=on-failure`) is approved and pending execution, after which the memory system's running state is independent of anyone remembering to start it — the PC, OpenCode, and WSL can restart at different times and the system either keeps running or, after the permanent stage, comes back on its own. System health is verifiable at any time by the `memory-functionality-audit.md` procedure in this repo, which writes a dated report to `C:\mybizz\memory-audit-reports\`.

---

## 6. A Worked Example — Watching the Nuances Actually Matter

During setup, GBrain and Graphify were both asked about the same real component — the Vault's secret-encryption mechanism — as a direct test of whether the confidence-and-authority model described above actually holds up in practice, not just in theory.

**GBrain's answer** drew from ADR-tier, sourced documents — not synthesized concept pages — describing the Vault as owner-only, gated behind a TOTP step-up, with the encryption key living in Anvil Secrets. Being sourced-document content, not GBrain-synthesized, this carries the higher of GBrain's two informal trust tiers described in Section 2.

**Graphify's answer** returned eight neighbor relationships for `encrypt()` — calls from `save_vault_secret()` and four test methods, an import, a containment edge, and a docstring-derived edge — every one of them labeled EXTRACTED, the highest of Graphify's three confidence tiers.

**Both signals were independently high-confidence, and they agreed.** That's the actual, load-bearing point — not merely that two tools were asked the same question, but that each answer sat at the trustworthy end of its own tool's own confidence scale, and the conclusions matched. Had either signal been lower-confidence — an INFERRED Graphify edge, a synthesized rather than sourced GBrain page — the same agreement would carry meaningfully less weight, and would be a lead worth verifying rather than a settled answer. The nuance is in knowing which situation you're actually in before trusting the combined result.

---

## 7. The Authority Model

A specific, deliberate ordering governs what wins when sources disagree:

1. **Current repository files, explicit user instructions, and tests** — always first, unconditionally.
2. **Graphify's EXTRACTED edges are trustworthy as read; INFERRED or AMBIGUOUS edges need independent verification** before anything consequential rests on them.
3. **Graph provenance must match the current commit.** A stale graph is describing a version of the codebase that no longer exists.
4. **Memory Governor decides what reaches durable GBrain memory** — a generated fact is not automatically permanent knowledge.
5. **Neither GBrain nor Graphify ever replaces actually looking.** Both accelerate the path to the real source; neither is a substitute for reading it once you're there.

---

## 8. Real Operational Nuances That Cut Across the Whole System

**A single, shared write-conflict risk, solved once, not per-tool.** Both GBrain and Graphify write persistent guidance into the same `AGENTS.md` file. Rather than two independent writers fighting over the same section, one combined, managed block — delimited by its own markers — holds a single policy for both. Only one orchestrator owns that block.

**The security checker and the memory governor are now mechanically connected — and were briefly in conflict.** The memory-governor's API is loopback traffic on this machine (`127.0.0.1:8321`), and the outbound-network rule originally blocked every destination not on its external allowlist — including loopback. Discovered live 2026-09-08: with enforcement active, the governed memory pipeline's own submission path was mechanically denied by the security system. Amended the same day through the rule pipeline: loopback destinations (`127.0.0.1`, `localhost`, `::1`) are always allowed — internal service traffic is not outbound access. Also verified the same day: MEMORY_VERBS is explicitly enabled (`mcp_surface: verbs`), so the ungoverned quick path is a live tool surface; the discipline that routes durable submissions through the governor is documented, not mechanical.

**MCP servers start once, at process bootstrap, never per chat session — for both tools identically.** Changing an MCP entry while OpenCode is already running writes correctly to disk and does nothing observable until the process is fully quit and relaunched — no error, no warning, just silent absence in the meantime.

**The local-only, gitignored architecture is a deliberate, reversible tradeoff, not a default.** Nearly the entire agent-tooling layer for both memory systems — the `AGENTS.md` guidance block, MCP entries, scaffolded skills — lives outside version control by design, kept local to one machine rather than traveling with the repo. The real cost: none of it reproduces automatically on a fresh clone.

**Deferred sources are policy, not neglect.** A dormant project's content stays registered and searchable without receiving ongoing sync maintenance — a health check flagging this as "stale" is functioning correctly, not detecting a problem.

---

## 9. Genuinely Open, System-Level Decisions

- Whether to fold `mb-3-cs`'s pre-existing "Access Model" bullets into the combined guidance block — raised once, never revisited.
- Whether the guidance block and Graphify's scope configuration should ever be committed to travel with the repo, versus staying local-only — a deliberately separate future decision, never made.
- GBrain's own version-upgrade policy — `0.50.0.0` is available while `0.46.1.0` remains in use (verified live 2026-09-14 via the upgrade banner; earlier notes recording 0.48.x as the available version were stale), under the standing rule: **never upgrade on newness — only on check failure**. Re-affirmed 2026-09-14 by the dream-cycle exam (run #1 + full DC3-1 cycle: no check failed, recommendation recorded to stay on 0.46.1.0). See `gbrain-config.md`.
- Graphify's git-hooks decision is a tripwire, not closed — contingent on the graph output staying gitignored; re-examine explicitly if that ever changes. See `graphify-config.md`.
- Dream-synthesized concept pages are DB-only (no backing file) — second line of defense approved 2026-09-14: `gbrain export` joins the weekly backup routine, destination `C:\backup-dream-cycle` (`/mnt/c/backup-dream-cycle`), alongside the weekly PostgreSQL dump (which remains line one, taken 2026-09-13). Estate backup-convention update flagged to `postgresql-config.md` §Backups.

**Dream-cycle verified state (2026-09-14, config-management run #1):** the cycle is working well and is now under standing configuration management (`mb-config-gbrain-dream-cycle` installed; exam verified: version/pack/credential/trigger surfaces exact, autopilot still absent — on-demand design intact). A full live cycle ran clean end-to-end: EXIT 0, all 23 phases in the confirmed order, +124 pages (→2,010, 100% embedded), 90 atoms (backlog drained), 126 concepts, 34 takes, zero contradictions surfaced. The §4 phase-sequence and post-run verification procedure remain exactly as documented — the run confirmed them live.

**A standing maintenance rule (developer-stated, 2026-09-06):** this document is passed to every memory-component run and must be updated by each such session to reflect current verified state — it is the cross-component coordination point, and a stale version defeats its purpose. At that run's close-out, the update is committed and pushed to `mybizz-os-docs` as its own commit — standing authorization, not re-decided per run. Relatedly, the developer groups the surrounding tooling as six closely-related components — GBrain, gbrain-dream-cycle, gstack, memory-governor, PostgreSQL, and the installed `mb-submit-memory` skill — to be assessed together rather than in isolation (recorded with provenance; this document's own four-mechanism framing above remains the structural lens here, with Graphify among the mechanisms it covers).

---

*Companion documents: `gbrain-explainer.md`, `gbrain-dream-cycle-explainer.md`, `graphify-explainer.md`, `memory-governor-explainer.md` — each with its own config and reference.*

*End of document*
