---
title: "mybizz-os-explainer"
doc-id: "mb-ddaef42c"
updated: "2026-10-10"
---
# MyBizz-OS — The Comprehensive Explainer

*What MyBizz-OS is, why it exists, how it works, what every part of it does, how it is kept healthy, and how you know when something has quietly stopped working. This is the top document of the MyBizz documentation estate. Every other explainer sits beneath it and holds the depth; this document holds the whole picture and points to them.*

---

## How to read this document

Each section answers one question, and each is written so that it can later become one diagram. The section "In one picture" at the end of a section names the elements and the main path that diagram will show. The full diagram plan is in Appendix A.

This document states what the system is and why. It does not repeat the detail held in each component's own documents, because a fact written twice is a fact that drifts. Where you need depth, follow the reference given. Every component of the system appears here, at overview level, and every one of them is listed in the component register (Section 11).

---

## 1. What MyBizz-OS is, and why it exists

**MyBizz-OS is the operating environment in which AI agents build real business software under the control of one non-technical developer.** It is not a single program. It is a set of cooperating systems on one computer — memory, enforcement, workflow, documentation, maintenance and a build framework — designed so that work done by an AI agent can be trusted without the developer having to read and verify every line of it personally.

**The problem it solves.** An AI coding agent produces plausible work quickly. Left to itself, it also forgets everything between sessions, follows written rules only when it remembers them, silently skips steps under pressure, reports what it believes it did rather than what it actually did, and drifts away from what was intended. For a developer who can judge whether a feature works but cannot audit the code, those failures are the entire risk. MyBizz-OS replaces trust in the agent with structure around the agent: memory that persists, rules that are enforced mechanically, steps that cannot be skipped silently, records that cannot diverge from what happened, and checks that catch what has stopped working.

**What it is meant to produce.** Working, secure web applications built on the Anvil.works platform — first among them the MyBizz platform itself, a complete online business system for consulting and services businesses, delivered to every subscriber as their own isolated application (Section 10). The production line that builds them is **PDLF**, the Project Development Lifecycle Framework (Section 3). Everything else in MyBizz-OS exists to make PDLF possible and trustworthy.

**The governing relationship.** One sentence holds the design together: **the developer decides, the agent executes, the records remember, and the machine enforces.** Every component below exists to keep one part of that sentence true — to stop the agent from becoming the decision-maker, to make sure nothing it does goes unrecorded, and to make sure no rule depends on the agent choosing to obey it.

**Its motto,** inherited from PDLF: *simple, secure, robust, fit for purpose.*

**In one picture:** developer → Claude (design) → OpenCode agent (execution) → MyBizz-OS systems (memory, enforcement, records) → PDLF → client applications on Anvil.

---

## 1A. What makes MyBizz-OS distinctive

Several frameworks now exist to keep AI coding agents disciplined; the most widely used ones organise work into stages with a human approval between planning and building. MyBizz-OS shares that idea and goes further in specific, checkable ways. The full assessment, including the comparison with named frameworks, is [[[pdlf:obsolete/pdlf-objective-review|the PDLF objective review]]](C:/pdlf/obsolete/pdlf-objective-review.md).

- **A real database ledger, not files.** Most comparable tools keep their working state in text files. PDLF records every step, gate and decision in a database whose structure enforces its own consistency.
- **A record that cannot differ from what happened.** The report the developer reads after each step is generated from the database row the step wrote, not composed afterwards by the agent.
- **Memory that compounds across projects.** GBrain is consulted at every step of every project, so each new project starts from what every earlier project learned. No comparable framework reviewed offers this.
- **Memory that is governed.** Facts that must be trusted later pass through a deterministic gate and the developer's approval; the rest is clearly marked advisory.
- **Rules that are enforced, not requested.** Cupcake checks every tool call mechanically, so a rule holds whatever the agent believes or intends.
- **Platform knowledge from the platform's own maker.** Anvil's own skill set keeps the agent's knowledge of the platform authoritative instead of guessed.
- **Security review built into the build.** Sentinel reviews features at the steps where security weight sits, and open findings block the gate.
- **A borrowed thinking discipline, not invented personas.** BFT forces specific answers about each step's output, scaled to how much judgement the step needs.
- **Selective skills, not a whole framework adopted wholesale.** Outside skills are used only where they do better than the agent alone; the orchestration stays MyBizz's own.

**The honest qualification.** The process is heavier than the field's typical tools, and it has not yet been proven across complete, real project builds. Its design reasons are specific and sound for a non-technical developer, but "simple, secure, robust, fit for purpose" remains a design intention until PDLF has carried real projects through to release.

**In one picture:** the field's common baseline (stages with approval) on one side; MyBizz-OS's additions (ledger, receipts, compounding memory, governed memory, enforcement, platform knowledge, security review, thinking discipline) on the other.

---

## 2. The four layers

The environment is described in four layers, always named by what they are. Every decision names its layer first, because a proposal that is correct in one layer is often wrong in another.

| Layer | What it is | Who it serves | Part of MyBizz-OS? |
|---|---|---|---|
| **The pre-makePDLF layer** | The supporting systems: memory, enforcement, security, workflow skills, documentation, maintenance | The developer and the agents, now | Yes — its foundation |
| **The makePDLF layer** | The development project that is building PDLF, step by step | PDLF, next | No — it is the project that builds a part of the system |
| **The PDLF layer** | The finished build framework: the harness in which client applications are made | The developer, during each client build | Yes — its purpose |
| **The shipped estate** | The Anvil applications PDLF produces, running for clients | Clients and their users | No — it is the system's output |

**MyBizz-OS is the pre-makePDLF layer plus the PDLF layer.** makePDLF is temporary scaffolding: when every PDLF step is built, its outputs are moved once into PDLF and the makePDLF project is archived. The shipped estate is what the system makes; nothing from MyBizz-OS travels into it.

**The standing caution.** None of MyBizz-OS exists at runtime inside a client application. Memory, enforcement and the documentation estate stay on this computer. Anything built into a client application must work entirely on its own.

**In one picture:** pre-makePDLF layer → makePDLF project → PDLF → shipped estate, with makePDLF drawn as the builder of PDLF rather than a stage of the system.

---

## 3. PDLF — the heart of the system

### What it is

PDLF is a **single, continuous, stage-gated build pipeline** that takes a client application from first idea to production and beyond. Every step's output becomes the next step's input; nothing can be silently skipped; every step leaves a permanent record. It is described in full in [[[pdlf:pdlf-library/pdlf-explainer-docs/pdlf-explainer|the PDLF explainer]]](C:/pdlf/pdlf-library/pdlf-explainer-docs/pdlf-explainer.md).

In outline:

- **Forty-six numbered steps** across **five phases** — Discover & Define, Design & Plan, Scaffold & Assemble, Build & Verify, Release. Step 1 is the orchestrator itself; the working pipeline runs from Step 11 to Step 46.
- **Seven gates**: hard stops that need the developer's explicit approval and cannot be quietly undone.
- **One execution sequence** that every step follows identically: consult memory, route the step, do the work, verify the result exists, record it, report, stop.
- **A deviation mechanism**: when a step cannot be done as written, the agent stops and offers exactly three options; it never decides alone.
- **BODY Frame Thinking (BFT)**: a thinking checkpoint, borrowed from Edward de Bono, applied heavily where real decisions are made and lightly where they are not. A blank or generic answer counts as no answer.
- **A feature build loop** that repeats design, build, review, QA and triage for every feature.
- **A learning junction** at the end (Step 46), where what the project taught is recorded for the next one.

### Why it exists

PDLF exists so that a non-technical developer can direct an AI agent to build software for paying clients and be justified in trusting the result. It provides continuity (nothing is lost between sessions), enforcement (no step runs before its dependencies), traceability (every step leaves a record), disciplined use of skills (a skill is used only where it genuinely does better than the agent alone), and compounding knowledge (each project starts from everything earlier projects learned). An independent assessment is in [[[pdlf:obsolete/pdlf-objective-review|the PDLF objective review]]](C:/pdlf/obsolete/pdlf-objective-review.md).

### The ledger — memory that cannot silently lie

PDLF's record is a PostgreSQL database, the **ledger**, holding projects, steps, gates, deviations, thinking checkpoints, ideal-state criteria and security findings. The ledger is the single source of truth: if it is not in the ledger, it did not happen. The report the developer reads after each step, the Step Receipt, is generated directly from the ledger row, so the agent's account and the actual record can never differ. The schema and connection rules are in [[[pdlf:pdlf-library/specifications-local/spec-postgres-ledger|the ledger specification]]](C:/pdlf/pdlf-library/specifications-local/spec-postgres-ledger.md).

### How MyBizz-OS underpins PDLF

| PDLF needs | Provided by |
|---|---|
| An execution engine that reads files, runs commands and invokes skills | OpenCode (Section 5.1) |
| Memory consulted before every step and fed after it | GBrain and the memory family (Section 5.2) |
| A record that survives every session | The ledger on PostgreSQL Instance A (Section 4) |
| Rules the agent cannot talk its way around | Cupcake (Section 5.3) |
| Security review at the steps where it matters | Sentinel (Section 5.3) |
| Platform knowledge of Anvil | anvil-agent-references and the anvil-docs library (Section 5.4) |
| Planning, review, QA and shipping disciplines | GStack and Matt Pocock skills (Section 5.5) |
| Wireframes and diagrams | Magdoub wireframe, Archify (Section 5.5) |
| Company-wide decisions and standards | project-library-global (Section 5.4) |
| Accurate, linked documentation | mb-doc-check, mb-project-docs, mb-sysdoc-creator (Section 5.6) |
| A clean end to every step | `/mb-close-out` and commit all (Section 5.7) |

### What PDLF delivers

A finished, tested, security-reviewed Anvil application, in the five-application structure that MyBizz uses for every client (Section 10), together with its full build record in the ledger and its learnings in GBrain.

### makePDLF — the project that builds PDLF

PDLF does not yet exist in finished form. It is being built by a development project, **makePDLF** (`C:\dev\dev-makepdlf\makepdlf-project-library`), one step at a time and never in bulk. Each completed step produces one module of PDLF's machinery. The modules have reserved places in PDLF's **app factory** (`C:\pdlf\app-factory\`, one module per step) and are moved there once, when all steps are complete; makePDLF is then archived. PDLF's own documentation lives in the PDLF library (`C:\pdlf\pdlf-library\`). Detail: [[[pdlf:pdlf-library/pdlf-explainer-docs/app-factory-explainer|the app factory]]](C:/pdlf/pdlf-library/pdlf-explainer-docs/app-factory-explainer.md), [[[pdlf:pdlf-library/pdlf-explainer-docs/orchestrator-agent-explainer|the orchestrator agent]]](C:/pdlf/pdlf-library/pdlf-explainer-docs/orchestrator-agent-explainer.md), [[[pdlf:obsolete/artifact-lifecycle-explainer|the life of an artifact]]](C:/pdlf/obsolete/artifact-lifecycle-explainer.md).

Two placeholder projects belong to this picture. **mb5pdlf** is the test project on which makePDLF and PDLF will be proven. **mb4ecom** is a future client-style project, to be modelled largely on mb-3-cs. Both are live, registered and in scope, though not yet scoped.

### Is PDLF only for Anvil.works?

**Yes, today it is.** PDLF is built specifically and exclusively for Anvil.works, a platform on which the whole application — interface, browser code, server code and database — is written in Python. Several parts of PDLF are Anvil-specific by design:

- the platform knowledge it builds against (Anvil's own `anvil-agent-references` skills);
- the steps that create Anvil data tables and scaffold forms in the Anvil editor;
- integration testing through Anvil Uplink, which connects test code to the running application;
- the Material 3 interface standard as Anvil implements it;
- the shipping boundary (only `anvil.yaml`, client code, server code and the theme travel to Anvil);
- the five-application client structure and the global decisions written for it.

**What is not platform-specific** is the method: the gates, the ledger, the deviation mechanism, the thinking checkpoints, the memory, the security review and the planning and review disciplines. Those would carry over to another framework. Adapting PDLF to another framework would therefore mean replacing its platform layer and rewriting the platform-bound steps — a substantial redesign, not a setting. Anvil's runtime is open source and can run outside Anvil's own cloud, so an application built through PDLF is not locked to Anvil's hosting, but it is built for Anvil's framework.

**In one picture:** idea → Discover & Define → Design & Plan → Scaffold & Assemble → Build & Verify (feature loop) → Release → learning junction, with the ledger beneath every step and GBrain consulted at every step.

---

## 4. The machine — Windows, WSL and the bridge between them

MyBizz-OS runs on one Windows computer that also hosts a Linux system through **WSL** (Windows Subsystem for Linux). WSL has one Linux installation, called `Ubuntu`, and it is the default. Each side holds what it is best at, and a deliberate set of bridges connects them. Paths on each side are listed in [[[mybizz-os-docs:os-reference-docs/wsl-path-references|the WSL path references]]](C:/mybizz/mybizz-os-docs/os-reference-docs/wsl-path-references.md).

### What lives on each side

| Windows | WSL (Linux) |
|---|---|
| All documentation and code repositories, on the `C:` drive | OpenCode, the agent harness, and its configuration |
| PostgreSQL Instance A — the PDLF ledger | PostgreSQL Instance B — GBrain's memory and the memory governor's records |
| Task Scheduler and its jobs | GBrain, the memory governor and the dream cycle |
| The browser in which OpenCode's interface is used | Cupcake, the enforcement layer, with its policies and grants |
| MarkText, the document editor | The installed skills |
| Backups on `C:` | Graphify, Git and the commit hook |
| Everything (`es.exe`), mgrep and n8n | The secrets: GBrain settings, the governor's credentials |

**Why the split.** The agent tooling — OpenCode, GBrain, Cupcake, the governor, Git — is Linux-native and most reliable in WSL. The repositories live on the Windows drive so that the developer's Windows tools (the editor, the browser, file search) see the same files the agent works on. The PDLF ledger runs on Windows for isolation (below).

### The bridges

- **The shared drive.** WSL sees the Windows drive as `/mnt/c/`; Windows sees the Linux files as `\\wsl.localhost\Ubuntu\`. One copy of every repository serves both sides.
- **The browser.** OpenCode runs in WSL and serves its interface, which the developer opens in a Windows browser.
- **The PowerShell bridge.** Agents in WSL reach the PDLF ledger on Windows only through PowerShell, because WSL does not pass Linux environment variables to Windows programs. This is the one permitted route to the ledger.
- **The port relay.** WSL relays Windows' port 5432 to the WSL database server. This is why the ledger listens on 5433 (below).
- **Task Scheduler into WSL.** Windows' scheduled jobs start Linux work by naming the installation and the user explicitly (`wsl.exe -d Ubuntu -u dev-p ...`).
- **Commits from WSL only.** The commit hook lives in WSL's Git; a commit made from a Windows program would bypass it.

### The two PostgreSQL instances — and why they are separate

| | Instance A | Instance B |
|---|---|---|
| Runs on | Windows | WSL |
| Program | PostgreSQL 15 | PostgreSQL 16 |
| Holds | The PDLF ledger (`pdlf`) | GBrain's memory (`gbrain`) and the governor's records (`memory_governor`) |
| Port | 5433 | 5432 |
| Character | Low churn, highest stakes | High churn, constant writing |

They are separate on purpose. GBrain and the governor write constantly; the ledger changes rarely but is the most consequential record in the system — if it is wrong, everything built from it is untrustworthy. On separate servers, a bad sync, a runaway process or a corrupted write on one side can never reach the other, and each server has its own narrowly scoped access roles. **The trap this creates:** port 5432 reaches Instance B from both sides, so a database command that does not name its instance and port silently connects to the wrong server. Every database command names both. Detail: [[[mybizz-config-docs:postgresql/postgresql-explainer|the PostgreSQL explainer]]](C:/mybizz/mybizz-config-docs/postgresql/postgresql-explainer.md).

**In one picture:** Windows (repositories, Instance A, Task Scheduler, browser, editor) ↔ bridges (shared drive, PowerShell bridge, port relay, `wsl.exe`) ↔ WSL (OpenCode, Cupcake, GBrain, governor, Instance B, skills, Git).

---

## 5. The components

The components fall into seven families. For each: what it does, why it is there, and where its depth lives.

### 5.1 The people and the harness

- **The developer** is the decision-maker. Every gate, every approval, every credentialed action and every live test of an enforcement rule is the developer's.
- **Claude, in chat, is the design partner.** It acts as the engineer: researching, designing, reviewing documents and writing the exact commands and prompts. It does not touch the machine directly; the developer carries its work into the terminal or into OpenCode.
- **OpenCode is the harness** — the program that turns an AI model into an agent able to read files, run commands and use skills. Everything else either runs inside it or is called by it. Its agents work in two modes: **plan** (reasoning, read-only) and **build** (making changes). It loads rules from `AGENTS.md` files, loads skills (installed in `~/.config/opencode/skills/` and `~/.agents/skills/`), connects to MCP servers (the plug-in interface through which GBrain and Graphify are reached), and applies a layered permission model. Detail: [[[mybizz-config-docs:opencode/opencode-explainer|the OpenCode explainer]]](C:/mybizz/mybizz-config-docs/opencode/opencode-explainer.md).
- **`AGENTS.md`** files carry the standing rules agents read: one global file (`~/.config/opencode/AGENTS.md`), and one per project, at each repository's root. A written rule is a request; only Cupcake (5.3) makes a rule mechanical.
- **AI providers** sit behind specific functions: the models the agents use, OpenAI for GBrain's embeddings, a reranker for search, and Anthropic for one dream-cycle phase. Current providers are recorded in [[[mybizz-config-docs:gbrain/gbrain-config|the GBrain configuration]]](C:/mybizz/mybizz-config-docs/gbrain/gbrain-config.md).

### 5.2 Memory — the system remembers

Memory is a family, not a tool: its parts act on each other. The authoritative map is [[[mybizz-os-docs:os-reference-docs/memory-master-explainer|the master memory explainer]]](C:/mybizz/mybizz-os-docs/os-reference-docs/memory-master-explainer.md).

- **GBrain** is the persistent, searchable memory outside any single conversation. It holds two layers: a **mirror** of every document and code repository (refreshed by sync, ungated, trusted because it mirrors real files) and **durable facts** meant to be believed later (gated). Each repository is a GBrain **source**; documentation sources are federated, so a search can cross repositories. GBrain is advisory: when it disagrees with a real file, the file wins. It is installed in `~/gbrain`, with its settings in `~/.gbrain`. Detail: [[[mybizz-config-docs:gbrain/gbrain-explainer|the GBrain explainer]]](C:/mybizz/mybizz-config-docs/gbrain/gbrain-explainer.md).
- **The dream cycle** is GBrain's deepening process: a long sequence of phases that extracts facts, synthesises concepts across documents and surfaces contradictions. It runs only when invoked, and every run is followed by a verification, because a phase can fail without saying so. Its output cannot pass through the governor — a permanent, accepted limit — so its findings are reviewed by hand. Detail: [[[mybizz-config-docs:gbrain-dream-cycle/gbrain-dream-cycle-explainer|the dream cycle explainer]]](C:/mybizz/mybizz-config-docs/gbrain-dream-cycle/gbrain-dream-cycle-explainer.md).
- **The memory governor** is the gate on durable memory: a small, strict service that decides whether a submitted fact may become permanent. Its decisions are deterministic, never AI judgment; it can only promote a fact or send it to the developer's review queue; and it identifies the writer from a credential, never from what the request claims. It runs permanently and restarts itself, from `~/memory-governor`. Detail: [[[mybizz-config-docs:memory-governor/memory-governor-explainer|the memory governor explainer]]](C:/mybizz/mybizz-config-docs/memory-governor/memory-governor-explainer.md).
- **`mb-submit-memory`** is the front door to that gate. It reads a real file, checks it against what GBrain already knows, forms an honest confidence and contradiction judgment, shows it to the developer, and submits only after an explicit yes. It exists because the quick, ungoverned way to write memory is always available; this skill makes the correct way the habitual one. Detail: [[[mybizz-config-docs:mb-submit-memory/mb-submit-memory-explainer|the mb-submit-memory explainer]]](C:/mybizz/mybizz-config-docs/mb-submit-memory/mb-submit-memory-explainer.md).
- **Graphify** builds a literal graph of a codebase — what calls what, what imports what — from the code's structure, with every connection labelled by how certain it is. It answers structural questions GBrain does not; GBrain answers the "why", Graphify the "where and how". Detail: [[[mybizz-config-docs:graphify/graphify-explainer|the Graphify explainer]]](C:/mybizz/mybizz-config-docs/graphify/graphify-explainer.md).
- **PostgreSQL** is the physical substrate: Instance B holds GBrain and the governor, Instance A holds the ledger (Section 4).

**Facts, advisory and governed.** Advisory facts (context and pointers) are saved directly. Governed facts (rules and decisions that must be trusted later) go through `mb-submit-memory` and the governor. The developer's guide is [[[mybizz-os-docs:operating-procedures/fact-save-and-recall|fact save and recall]]](C:/mybizz/mybizz-os-docs/operating-procedures/fact-save-and-recall.md).

### 5.3 Enforcement and security — the system protects

- **Cupcake** sits between the agent and every tool it tries to use, and mechanically allows or blocks each call against compiled rules. It does not ask the agent to comply and does not trust its account of what it is about to do. The agent can never run Cupcake itself. Its plug-in is `~/.config/opencode/plugin/cupcake.js`, and its compiled rules are in `~/.config/cupcake/policies/opencode/`. When the agent needs access it is not normally given — writing outside its project, or reaching a website — the developer grants it for a limited time and number of writes (`mg-grant`, `net-grant`; [[[mybizz-os-docs:operating-procedures/grant-paths|grant paths]]](C:/mybizz/mybizz-os-docs/operating-procedures/grant-paths.md)). If Cupcake's own evaluation ever breaks, it lets calls through and logs the fault, so a broken rule cannot stop all work; the standing exam catches that state. Detail: [[[mybizz-config-docs:cupcake/cupcake-explainer|the Cupcake explainer]]](C:/mybizz/mybizz-config-docs/cupcake/cupcake-explainer.md).
- **`mb-compile-rules`** is the only way a rule enters Cupcake. Rules are written in plain language in `C:\dev\project-library-global\rules-cupcake-global\`; this skill turns one into a compiled policy, places it, and has the developer run real block and allow tests against the live system. Nothing stays in place unless it passed. Detail: [[[mybizz-config-docs:mb-compile-rules/mb-compile-rules-explainer|the mb-compile-rules explainer]]](C:/mybizz/mybizz-config-docs/mb-compile-rules/mb-compile-rules-explainer.md).
- **The security-global library** (`C:\dev\project-library-global\security-global\`) defines what "secure" means for MyBizz: principles and practices across authentication, data, secrets, dependencies, infrastructure and more. It is the authority both Sentinel and the rule set draw on.
- **Sentinel** protects the applications PDLF builds. It is wired into the PDLF steps where security weight sits, reviews the actual feature against the security library, and returns Clear, Flag or Escalate. A flag is written to the ledger as a finding, and a gate cannot pass while findings remain open ("no row, no gate"). Detail: [[[mybizz-config-docs:mb-sentinel-review/sentinel-explainer|the Sentinel explainer]]](C:/mybizz/mybizz-config-docs/mb-sentinel-review/sentinel-explainer.md).
- **`secure-software-engineering`** is the engineering skill whose prevention-first method Sentinel's reasoning is borrowed from; it is also available directly for design and review work.

**Two kinds of protection, two targets.** Cupcake and its rules protect the *machine* from the agent. Sentinel and the security library protect the *client applications* from attackers.

### 5.4 Knowledge and standards — the system knows the rules of the business

- **project-library-global** (`C:\dev\project-library-global\`) is MyBizz's own definitive library of decisions and standards that apply to every application PDLF builds: global architecture decisions (ADRs), policies, specifications, standards, standard operating procedures, templates, guides, the Anvil documentation compilation (`anvil-docs`), the Cupcake rule sources and the security library. Some sections are still being written.
- **anvil-agent-references** is Anvil's own published skill set for AI agents: how forms, client modules, server code, data models and the `anvil.yaml` manifest actually work, generated from Anvil's source. It is the platform standard PDLF builds against, and it stops the agent guessing at a framework it was never trained to know in detail. It is a read-only copy of Anvil's repository, at `C:\mybizz\anvil-agent-references\`. Detail: [[[mybizz-config-docs:anvil-agent-references/anvil-agent-references-explainer|the anvil-agent-references explainer]]](C:/mybizz/mybizz-config-docs/anvil-agent-references/anvil-agent-references-explainer.md).
- **The project libraries** hold each project's own documentation: `makepdlf-project-library` (`C:\dev\dev-makepdlf\makepdlf-project-library\`), `mb-3-cs-project-library` (`C:\dev\dev-mb-3-cs\mb-3-cs-project-library\`), `mb4ecom-project-library` (`C:\dev\dev-mb4ecom\mb4ecom-project-library\`) and `mb5pdlf-project-library` (`C:\dev\dev-mb5pdlf\mb5pdlf-project-library\`), each paired with an Anvil code repository where one exists.
- **The system documentation libraries**: `mybizz-os-docs` (`C:\mybizz\mybizz-os-docs\`: operating procedures and system-wide reference, including this document), `mybizz-config-docs` (`C:\mybizz\mybizz-config-docs\`: one document suite per component), `pdlf` (`C:\pdlf\`: the PDLF library and app factory), `data-mybizz-mgt` (`C:\data-mybizz-mgt\`: management and planning working files) and `dev-root` (`C:\dev\dev-root\`: the hub of the development folder).
- **The scope list** records which folders are part of the documentation corpus and which are never referenced: [[[mybizz-os-docs:os-reference-docs/mybizz-scope-list|the scope list]]](C:/mybizz/mybizz-os-docs/os-reference-docs/mybizz-scope-list.md).

Every repository, its GitHub home and its GBrain source are listed in one place: [[[mybizz-os-docs:os-reference-docs/repo-organization|repository organisation]]](C:/mybizz/mybizz-os-docs/os-reference-docs/repo-organization.md).

### 5.5 Workflow and engineering skills — the system works with discipline

Skills are packaged instructions an agent loads to do one kind of work well. Every installed skill is listed in [[[mybizz-os-docs:os-reference-docs/skills-catalog|the skills catalogue]]](C:/mybizz/mybizz-os-docs/os-reference-docs/skills-catalog.md).

- **GStack** is a virtual engineering team as skills: product interrogation, plan reviews from several professional angles, code review, real-browser QA, security audits, shipping and canary monitoring. GBrain is memory; GStack is workflow. Detail: [[[mybizz-config-docs:gstack/gstack-explainer|the GStack explainer]]](C:/mybizz/mybizz-config-docs/gstack/gstack-explainer.md).
- **Matt Pocock skills** are small, composable engineering disciplines: aligning on intent before building, domain modelling, test-driven slices, ticketing, triage, architecture surveys. Together with `secure-software-engineering`, they are the engineering pool PDLF draws on. Detail: [[[mybizz-config-docs:matt-pocock-skills/matt-pocock-skills-explainer|the Matt Pocock skills explainer]]](C:/mybizz/mybizz-config-docs/matt-pocock-skills/matt-pocock-skills-explainer.md).
- **Magdoub wireframe** turns a one-line feature description into five alternative interactive wireframes, so a screen's structure is settled before any interface is built. PDLF uses it twice: rough structure first, refinement once the structure is approved. Detail: [[[mybizz-config-docs:magdoub-wireframe/magdoub-wireframe-explainer|the Magdoub wireframe explainer]]](C:/mybizz/mybizz-config-docs/magdoub-wireframe/magdoub-wireframe-explainer.md).
- **Archify** turns a typed description of a system into a validated, interactive diagram, every element traceable to its source. **`mb-archify-scope`** comes first: it agrees the diagram's scope — one main path, at most twelve nodes — before any drawing starts. Detail: [[[mybizz-config-docs:archify/archify-explainer|the Archify explainer]]](C:/mybizz/mybizz-config-docs/archify/archify-explainer.md), [[[mb-archify-scope:mb-archify-scope|the scoping skill]]](C:/mybizz/mb-archify-scope/mb-archify-scope.md).
- **i-have-adhd** reshapes the agents' response style for a reader with ADHD: action first, numbered steps, no tangents. Detail: [[[mybizz-config-docs:i-have-adhd/i-have-adhd-explainer|the i-have-adhd explainer]]](C:/mybizz/mybizz-config-docs/i-have-adhd/i-have-adhd-explainer.md).
- **MiniMax document skills** (`minimax-docx`, `minimax-xlsx`) let agents produce Word and Excel files.
- **Graphify's operational skill** is how agents query the code graphs (5.2).

### 5.6 Documentation integrity — the system keeps its own records true

The documents are written by AI agents, so their consistency cannot depend on anyone remembering a rule.

- **`mb-doc-check`** (`C:\mybizz\scripts\mb-doc-check.py`, with its source list `mb-doc-check.sources` beside it) runs inside every commit. It gives every document its identity (a permanent `doc-id`), its title and its date of last real change; writes every link in one standard form that both GBrain and the editor can follow; repairs links when documents move or are renamed; turns links to deleted documents into plain words; and stops a commit that introduces a link pointing to nothing. It is a program, not an AI: the same input always gives the same result. Detail: [[[mybizz-config-docs:mb-doc-check/doc-check-explainer|the mb-doc-check explainer]]](C:/mybizz/mybizz-config-docs/mb-doc-check/doc-check-explainer.md), [[[mybizz-os-docs:os-reference-docs/documentation-linking-explainer|the documentation linking explainer]]](C:/mybizz/mybizz-os-docs/os-reference-docs/documentation-linking-explainer.md), [[[mybizz-os-docs:os-reference-docs/frontmatter-conventions|the frontmatter conventions]]](C:/mybizz/mybizz-os-docs/os-reference-docs/frontmatter%20conventions.md).
- **MarkText** is the editor the documents are read in; the stored link form includes a full path so a link opens with a click.
- **GitHub** holds the off-machine copy of every repository, private by default. A commit pushed is a commit that survives the loss of this computer.
- **`mb-sysdoc-creator`** writes a component's three-document suite — configuration (what is set), reference (how to act) and explainer (what it is and why) — after researching the live system, never by transcribing supplied material. Detail: [[[mybizz-config-docs:mb-sysdoc-creator/sysdoc-creator-explainer|the mb-sysdoc-creator explainer]]](C:/mybizz/mybizz-config-docs/mb-sysdoc-creator/sysdoc-creator-explainer.md).
- **`mb-project-docs`** keeps each project's own three files and README current, at close-out. Detail: [[[mybizz-config-docs:mb-project-docs/project-docs-explainer|the mb-project-docs explainer]]](C:/mybizz/mybizz-config-docs/mb-project-docs/project-docs-explainer.md).

### 5.7 Operations and health — the system maintains itself, with the developer

- **Commit all** (`C:\mybizz\scripts\commit-all.sh`) commits every documentation repository (the hook runs inside each commit), updates GBrain from every source (`C:\mybizz\scripts\sync-gbrain.sh`), and pushes everything to GitHub, then prints one summary. Procedure: [[[mybizz-os-docs:operating-procedures/commit-all-repositories|commit all repositories]]](C:/mybizz/mybizz-os-docs/operating-procedures/commit-all-repositories.md).
- **Close out** (`/mb-close-out`) ends a session and its task in one run: the session page in GBrain, the facts worth keeping, the project documents, one approval, then one terminal block ending in commit all, and an empty review queue. Procedure: [[[mybizz-os-docs:operating-procedures/opening-and-closing|opening and closing]]](C:/mybizz/mybizz-os-docs/operating-procedures/opening-and-closing.md); skill: [[[mb-close-out:mb-close-out|mb-close-out]]](C:/mybizz/mb-close-out/mb-close-out.md).
- **Scheduled tasks** (Windows Task Scheduler):
  - **SundayBackup** — every Sunday: backs up both PostgreSQL instances to two locations (`C:\backup-gbrain\` and `/home/dev-p/backup-gbrain/`), checks every copy, keeps two cycles, exports GBrain to `C:\backup-dream-cycle\`, and reports the result in a pop-up. Script: `C:\mybizz\scripts\sunday-backup.sh`; pop-up: `C:\mybizz\scripts\show-sunday-result.ps1`; log: `C:\mybizz\logs\sunday-backup\`.
  - **DailyDevBackup** — every day: copies the development folders into `C:\backup-daily\`. Script: `C:\mybizz\scripts\daily-backup.ps1`.
  - **OpenCodeSessionPrune** — every day: removes OpenCode session history older than sixty days, after backing it up to `C:\backup-wsl\opencode-session-prune\`. Script: `C:\mybizz\scripts\opencode-session-prune.sh`.
  - **MissedRunCheck** — at every logon: reports any scheduled task that was missed or did not finish. Script: `C:\mybizz\scripts\missed-run-check.ps1`.
- **The memory system diagnostic** (`/mb-memory-system-diagnostic`) walks the whole memory family in one pass, with real checks, and repairs what it finds with the developer's approval. Its final section runs the dream cycle when the developer agrees. Detail: [[[mybizz-config-docs:mb-memory-system-diagnostic/mb-memory-system-diagnostic-explainer|the diagnostic explainer]]](C:/mybizz/mybizz-config-docs/mb-memory-system-diagnostic/mb-memory-system-diagnostic-explainer.md).
- **Configuration management** (`mb-config-manager` and one exam skill per system) compares each live system against its written specification and catches the gap between "present" and "working". Detection is automatic; every repair waits for the developer. Detail: [[[mybizz-config-docs:mb-config-manager/config-manager-explainer|the configuration management explainer]]](C:/mybizz/mybizz-config-docs/mb-config-manager/config-manager-explainer.md).
- **Supporting tools**: `C:\mybizz\scripts\opencode-db-cleanup.sh` (removes stale project records from OpenCode after a folder is renamed), the OpenCode cache reset ([[[mybizz-os-docs:operating-procedures/daily-ops|daily operations]]](C:/mybizz/mybizz-os-docs/operating-procedures/daily-ops.md), [[[mybizz-os-docs:os-reference-docs/opencode-reset-management|OpenCode reset management]]](C:/mybizz/mybizz-os-docs/os-reference-docs/opencode-reset-management.md)), and the clean WSL shutdown procedure ([[[mybizz-config-docs:postgresql/postgresql-reference|the PostgreSQL reference]]](C:/mybizz/mybizz-config-docs/postgresql/postgresql-reference.md), "Clean shutdown").
- **Search tools**: Everything (`es.exe`) for instant file search on Windows, and mgrep for searching file contents by meaning.
- **n8n**, a workflow-automation tool, is installed and available for linking processes, but deliberately unused, to keep the system lean.
- **Google Calendar** holds the reminders: the Sunday maintenance reminder and the task list for configuration exams.

**In one picture (component map):** OpenCode at the centre; memory family, enforcement family, knowledge library, skills, documentation integrity and operations around it; PDLF drawing on all of them.

---

## 6. How it works in motion

### A working session

1. The developer starts OpenCode and chooses the project.
2. The agent reads the global and project rules and, for project work, the project's three files.
3. The agent consults GBrain before acting, and Graphify for code structure.
4. Every tool call passes through Cupcake. Access outside the agent's normal reach needs a time-limited grant from the developer.
5. The work is done in plan or build mode; documents are written with short links, which the hook later completes.
6. The developer runs `/mb-close-out`. It records the session, sorts and saves facts, updates the project documents, and hands the developer one block that ends in commit all.
7. Commit all commits (the hook checks every document), refreshes GBrain and pushes to GitHub.

In PDLF, the same pattern runs at step scale: memory consulted, step executed, result verified and written to the ledger, Step Receipt shown, gate or close-out, stop.

### How knowledge moves into memory

| Path | What it writes | Guarded by |
|---|---|---|
| Sync (inside commit all) | The mirror of every repository | Nothing — fidelity to real files is its authority |
| `mb-submit-memory` | Governed facts | The memory governor and the developer's approval |
| PDLF's step writer | PDLF's own governed facts, under its own credential | The memory governor |
| The dream cycle | Deepened understanding | A manual review after every run |
| Quick writes | Advisory facts | Nothing — used only for advisory content |

Detail: [[[mybizz-os-docs:os-reference-docs/memory-master-explainer|the master memory explainer]]](C:/mybizz/mybizz-os-docs/os-reference-docs/memory-master-explainer.md), Section 3.

### How a question is answered — the order of authority

1. Current repository files, the developer's explicit instructions and passing tests.
2. Graphify's directly read code connections.
3. Governed facts.
4. Graphify's inferred connections and GBrain's mirrored documents.
5. GBrain's synthesised concept pages.

When two sources at the top of their own trust scales agree, the answer is settled. Neither memory nor graph ever replaces reading the real source.

**In one picture (session flow):** start → rules → memory → work through Cupcake → `/mb-close-out` → approval → commit all → GitHub and GBrain.
**In one picture (memory flow):** repositories → sync → mirror; fact file → `mb-submit-memory` → governor → durable facts; dream cycle → review.

---

## 7. The principles that hold it together

These are the rules every component obeys. Each one exists because its absence caused a real failure.

1. **The developer decides; the agent executes.** Gates, approvals, credentials, upgrades and live tests of enforcement belong to the developer.
2. **A written rule is a request; an enforced rule is a guarantee.** Anything that must hold every time is a Cupcake rule, not a sentence in `AGENTS.md`.
3. **Denials are final.** A blocked action is never retried, reshaped or routed around. It becomes a line the developer runs.
4. **Agents never commit, push or handle credentials.** Those are always the developer's terminal actions.
5. **Files outrank memory.** GBrain is advisory; a real file always wins.
6. **One fact, one home.** Every fact lives in exactly one document; others point to it. Duplicates drift.
7. **Verify; never assume.** A claim that cannot be checked is recorded as open, not written as true. A skill is read before it is trusted by its name. In a chain of commands, each step checks its result before the next one runs, so a failure stops the chain instead of writing silently into the wrong place.
8. **Present is not working, and completed is not worked.** Existence is never taken as proof of function.
9. **Nothing is skipped silently.** A step that cannot be done stops with options; it is never quietly passed over.
10. **Name the layer, the instance and the installation.** Every decision names its layer; every database command names its instance and port; every scheduled WSL command names the installation and user.
11. **Nothing runs in the background without being reported.** What is scheduled reports its result; what is not scheduled runs only when asked.
12. **The system may observe itself, never change itself.** A proposal to change how the system behaves passes through the same review as any other change.
13. **A false alarm is a fault in the detector.** An alarm with a real cause of little consequence is noted and set aside. An alarm with no cause at all means the check itself is wrong, and the check gets fixed.

---

## 7A. Nuances that only exist at system level

Some behaviours belong to no single component; they appear only when components act on each other. Each is listed with how the system handles it. The authoritative account is [[[mybizz-os-docs:os-reference-docs/memory-master-explainer|the master memory explainer]]](C:/mybizz/mybizz-os-docs/os-reference-docs/memory-master-explainer.md), Section 9.

- **Two servers, one port.** Port 5432 reaches the GBrain server from both sides, so an unnamed database command silently reaches the wrong server. Handled by naming instance and port every time (Section 4).
- **The name coupling.** The memory governor delivers an approved fact to GBrain under the owning project's name. If that name does not match a GBrain source exactly, the fact is approved but never arrives. Handled by close-out's verification against the governor's worker log, and by the memory diagnostic.
- **The startup coupling.** The governor reads its settings and credentials only at the moment it starts; started the wrong way, it runs but rejects everything. Handled by its permanent automatic start, which always starts it the correct way; the risk returns only if it is ever started by hand.
- **The default-source coupling.** With each governed write, the governor's worker changes GBrain's default source. A default that names a source that no longer exists stops every GBrain sync. Handled by `sync-gbrain.sh`, which resets the default before every sync; every sync and every governed write names its own source, so the reset is always safe.
- **Facts are stored per source.** A search for facts reads one project's facts, so an empty answer does not prove a fact does not exist. Handled by checking the governor's worker log as the record of what landed.
- **The quick path is always there.** Every session can write memory directly, ungoverned. Handled by discipline: governed facts go through `mb-submit-memory`; quick writes are advisory only.
- **Completed is not worked.** The dream cycle can finish while a phase did nothing. Handled by its mandatory post-run verification.
- **The dream cycle cannot be gated.** Its output reaches memory without passing the governor. Handled by reviewing its findings after every run.
- **Connections load once.** A changed MCP connection does nothing until OpenCode is fully restarted, and gives no warning. Handled by restarting after every such change.
- **The agent cannot test its own police.** Live tests of Cupcake rules are always run by the developer, so the agent can never weaken its own enforcement.
- **Local-only configuration.** OpenCode's settings, the installed skills, Cupcake's policies, GBrain's settings and the memory connections live only on this computer and travel with no repository. Handled today by daily and weekly backups on the same machine; protection against losing the whole machine is the subject of the planned backup plan.

---

## 8. Keeping it healthy

| When | What | Runs |
|---|---|---|
| Every session | Start OpenCode; work; `/mb-close-out` (ending in commit all) | Developer, with the agent |
| After other work | Commit all | Developer |
| Every day | DailyDevBackup, OpenCodeSessionPrune | By itself |
| Every logon | MissedRunCheck | By itself |
| Every Sunday, 10:00 | SundayBackup | By itself, with a pop-up |
| Every Sunday, after the backup | `/mb-memory-system-diagnostic`, then the dream cycle at its end | Developer, in OpenCode |
| On drift or after a significant change | Configuration exams through `mb-config-manager` | Developer (calendar task) |
| After a GBrain upgrade | The upgrade checklist, `slug-selftest`, `sources-check` | Developer |
| After renaming a project folder | `opencode-db-cleanup.sh` | Developer |
| Before shutting WSL down | The clean shutdown procedure | Developer |

The day-to-day manual is [[[mybizz-os-docs:operating-procedures/daily-ops|daily operations]]](C:/mybizz/mybizz-os-docs/operating-procedures/daily-ops.md).

**In one picture (maintenance cycle):** session → close-out → commit all; daily backups and prune; logon check; Sunday backup → diagnostic → dream cycle; exams on drift.

---

## 9. Silent failure — how you know

The system's main enemy is not failure but **silent** failure: something stops working while still looking present. Every mechanism below exists to turn a silent failure into a visible one.

| Silent failure | What catches it | Where you see it |
|---|---|---|
| A scheduled job did not run, or stopped part-way | MissedRunCheck | Message at logon |
| The weekly backup failed | SundayBackup's own check | Pop-up, with what to do |
| A backup exists but is damaged | Hash checks in the backup and in the diagnostic | Pop-up; diagnostic report |
| A document has a broken link or missing identity | `mb-doc-check` | The commit stops and names the line |
| A repository lost its commit hook | Commit all's hook check | Warning in the commit-all output |
| A repository was not committed or pushed | Commit all's summary | Anything other than "All done." |
| GBrain's sync failed for a source | `sync-gbrain.sh` error count | Commit-all summary |
| GBrain's health has degraded | `gbrain doctor` against its known baseline | A new failure line |
| GBrain's default source names a deleted source | `sync-gbrain.sh` resets it before every sync | Self-correcting |
| The memory governor is closed or no longer refuses what it should | The diagnostic's positive and negative write tests | Diagnostic report |
| An approved fact never arrived in GBrain | The governor worker log; the review queue | Close-out verification |
| The dream cycle "finished" but a phase did nothing | Its post-run verification | Dream-cycle verification output |
| A code graph is out of date | Graphify's provenance check | Diagnostic; exam report |
| An enforcement rule is installed but never fires | `mb-compile-rules`' live tests; the Cupcake exam | Test results; exam report |
| Cupcake has silently degraded to allowing everything | The Cupcake exam | Exam report |
| A component no longer matches its specification | Its configuration exam | Exam report |
| A command reached the wrong database | Naming instance and port every time | Prevented, not detected |
| A changed MCP connection is not loaded | Restarting OpenCode after any change | Prevented, not detected |
| A security finding is left open | The ledger's findings table | The gate cannot pass |

**The measure of the system** is not that nothing ever fails, but that nothing fails silently for long.

**In one picture (detection matrix):** failure classes on one side, detectors on the other, each detector pointing to the place the developer sees the result.

---

## 10. The shipped estate — what MyBizz delivers

### The product

**MyBizz is a complete online business platform for consulting and services businesses, sold as a subscription.** It replaces the separate tools such a business otherwise pays for and connects by hand — website builder, booking system, payment processing, customer database, email marketing, invoicing, blog and analytics — with one platform, one setup and one price. It serves any business that sells time or deliverables, from health practitioners and salons to consultants, trades and tutors, starting with solo practitioners and small practices. It handles payments internationally and in Africa, and multiple currencies.

Every subscriber receives their own application, with their own database. MyBizz is not a marketplace: it takes no commission, lists no competitors beside the subscriber, and every client record belongs to the subscribing business alone.

The project that builds it is **mb-3-cs** (MyBizz Consulting & Services). Its development began before PDLF existed and is on hold; it will be built through PDLF, as PDLF's first real product. Its strategy, positioning and full feature description were written before PDLF and describe the product, not the build method: [[[mb-3-cs-project-library:docs-local/positioning-strategy|positioning strategy]]](C:/dev/dev-mb-3-cs/mb-3-cs-project-library/docs-local/positioning-strategy.md), [[[mb-3-cs-project-library:docs-local/platform-architecture-narrative|platform architecture narrative]]](C:/dev/dev-mb-3-cs/mb-3-cs-project-library/docs-local/platform-architecture-narrative.md), [[[mb-3-cs-project-library:docs-local/founder-strategic-review|founder strategic review]]](C:/dev/dev-mb-3-cs/mb-3-cs-project-library/docs-local/founder-strategic-review.md). **mb4ecom** is a future product, to be modelled largely on mb-3-cs.

### The five-application structure

The applications PDLF builds run on Anvil.works in a fixed five-application structure, defined by MyBizz's global decisions:

1. **The development workspace** for the product, where all feature work happens.
2. **The master template**, the single source of all server logic and interface forms for every client.
3. **The blank client template**, from which each new client application is created.
4. **One application per paying client**, which inherits everything from the master.
5. **The management application** for MyBizz itself (planned).

Each client application has its own database ([[[default:adr-global/adr-dependency-based-not-multi-tenant|the isolation decision]]](C:/dev/project-library-global/adr-global/adr-dependency-based-not-multi-tenant.md), [[[default:adr-global/adr-client-instance-architecture|the client instance architecture]]](C:/dev/project-library-global/adr-global/adr-client-instance-architecture.md)), so one client's data is structurally unreachable from another's. Multi-tenant patterns — one shared database filtered by client — are forbidden by a sealed decision, because they would replace structural isolation with a filter that can fail. Only the application's own code and theme travel to Anvil; nothing from MyBizz-OS is present at runtime. Detail: the global decisions in `C:\dev\project-library-global\adr-global\`.

---

## 11. Component register

Every component, once. The family numbers refer to Section 5.

| Component | Family | Runs on | What it does | Read more |
|---|---|---|---|---|
| Developer | 5.1 | — | Decides, approves, runs credentialed and committing actions | This document |
| Claude (chat) | 5.1 | Browser | Design partner and engineer | This document |
| OpenCode | 5.1 | WSL | The agent harness | [[[mybizz-config-docs:opencode/opencode-explainer]]](C:/mybizz/mybizz-config-docs/opencode/opencode-explainer.md) |
| `AGENTS.md` files | 5.1 | WSL and repositories | Standing rules for agents | [[[mybizz-config-docs:opencode/opencode-explainer]]](C:/mybizz/mybizz-config-docs/opencode/opencode-explainer.md) |
| AI providers | 5.1 | External | Models, embeddings, reranking | [[[mybizz-config-docs:gbrain/gbrain-config]]](C:/mybizz/mybizz-config-docs/gbrain/gbrain-config.md) |
| GBrain | 5.2 | WSL | Persistent memory: mirror and facts | [[[mybizz-config-docs:gbrain/gbrain-explainer]]](C:/mybizz/mybizz-config-docs/gbrain/gbrain-explainer.md) |
| Dream cycle | 5.2 | WSL | Deepens memory; verified after every run | [[[mybizz-config-docs:gbrain-dream-cycle/gbrain-dream-cycle-explainer]]](C:/mybizz/mybizz-config-docs/gbrain-dream-cycle/gbrain-dream-cycle-explainer.md) |
| Memory governor | 5.2 | WSL | Gate on durable facts | [[[mybizz-config-docs:memory-governor/memory-governor-explainer]]](C:/mybizz/mybizz-config-docs/memory-governor/memory-governor-explainer.md) |
| `mb-submit-memory` | 5.2 | WSL | Front door to the governor | [[[mybizz-config-docs:mb-submit-memory/mb-submit-memory-explainer]]](C:/mybizz/mybizz-config-docs/mb-submit-memory/mb-submit-memory-explainer.md) |
| Graphify | 5.2 | WSL | Code structure graphs | [[[mybizz-config-docs:graphify/graphify-explainer]]](C:/mybizz/mybizz-config-docs/graphify/graphify-explainer.md) |
| PostgreSQL Instance A | 4 | Windows | The PDLF ledger | [[[mybizz-config-docs:postgresql/postgresql-explainer]]](C:/mybizz/mybizz-config-docs/postgresql/postgresql-explainer.md) |
| PostgreSQL Instance B | 4 | WSL | GBrain and governor data | [[[mybizz-config-docs:postgresql/postgresql-explainer]]](C:/mybizz/mybizz-config-docs/postgresql/postgresql-explainer.md) |
| Cupcake | 5.3 | WSL | Mechanical enforcement of rules | [[[mybizz-config-docs:cupcake/cupcake-explainer]]](C:/mybizz/mybizz-config-docs/cupcake/cupcake-explainer.md) |
| Grants (`mg-grant`, `net-grant`) | 5.3 | WSL | Time-limited extra access | [[[mybizz-os-docs:operating-procedures/grant-paths]]](C:/mybizz/mybizz-os-docs/operating-procedures/grant-paths.md) |
| `mb-compile-rules` | 5.3 | WSL | Turns written rules into tested enforcement | [[[mybizz-config-docs:mb-compile-rules/mb-compile-rules-explainer]]](C:/mybizz/mybizz-config-docs/mb-compile-rules/mb-compile-rules-explainer.md) |
| Security-global library | 5.3 | Windows (repository) | Defines "secure" for MyBizz | `C:\dev\project-library-global\security-global\` |
| Sentinel | 5.3 | WSL, inside PDLF | Security review of what PDLF builds | [[[mybizz-config-docs:mb-sentinel-review/sentinel-explainer]]](C:/mybizz/mybizz-config-docs/mb-sentinel-review/sentinel-explainer.md) |
| `secure-software-engineering` | 5.3 | WSL | Prevention-first security method | [[[mybizz-os-docs:os-reference-docs/skills-catalog]]](C:/mybizz/mybizz-os-docs/os-reference-docs/skills-catalog.md) |
| project-library-global | 5.4 | Windows (repository) | MyBizz's global decisions and standards | `C:\dev\project-library-global\` |
| anvil-agent-references | 5.4 | Windows clone, WSL skills | Anvil's own platform knowledge | [[[mybizz-config-docs:anvil-agent-references/anvil-agent-references-explainer]]](C:/mybizz/mybizz-config-docs/anvil-agent-references/anvil-agent-references-explainer.md) |
| Project libraries | 5.4 | Windows (repositories) | Each project's documentation | [[[mybizz-os-docs:os-reference-docs/repo-organization]]](C:/mybizz/mybizz-os-docs/os-reference-docs/repo-organization.md) |
| System documentation libraries | 5.4 | Windows (repositories) | Operating, component and PDLF documentation | [[[mybizz-os-docs:os-reference-docs/repo-organization]]](C:/mybizz/mybizz-os-docs/os-reference-docs/repo-organization.md) |
| Scope list | 5.4 | Windows (repository) | What the corpus includes and excludes | [[[mybizz-os-docs:os-reference-docs/mybizz-scope-list]]](C:/mybizz/mybizz-os-docs/os-reference-docs/mybizz-scope-list.md) |
| GStack | 5.5 | WSL | Workflow skills: plan, review, QA, ship | [[[mybizz-config-docs:gstack/gstack-explainer]]](C:/mybizz/mybizz-config-docs/gstack/gstack-explainer.md) |
| Matt Pocock skills | 5.5 | WSL | Engineering disciplines | [[[mybizz-config-docs:matt-pocock-skills/matt-pocock-skills-explainer]]](C:/mybizz/mybizz-config-docs/matt-pocock-skills/matt-pocock-skills-explainer.md) |
| Magdoub wireframe | 5.5 | WSL | Alternative wireframes before building | [[[mybizz-config-docs:magdoub-wireframe/magdoub-wireframe-explainer]]](C:/mybizz/mybizz-config-docs/magdoub-wireframe/magdoub-wireframe-explainer.md) |
| Archify | 5.5 | WSL | Validated interactive diagrams | [[[mybizz-config-docs:archify/archify-explainer]]](C:/mybizz/mybizz-config-docs/archify/archify-explainer.md) |
| `mb-archify-scope` | 5.5 | WSL | Scopes a diagram before drawing | [[[mb-archify-scope:mb-archify-scope]]](C:/mybizz/mb-archify-scope/mb-archify-scope.md) |
| i-have-adhd | 5.5 | WSL | Response style for an ADHD reader | [[[mybizz-config-docs:i-have-adhd/i-have-adhd-explainer]]](C:/mybizz/mybizz-config-docs/i-have-adhd/i-have-adhd-explainer.md) |
| MiniMax document skills | 5.5 | WSL | Word and Excel output | [[[mybizz-os-docs:os-reference-docs/skills-catalog]]](C:/mybizz/mybizz-os-docs/os-reference-docs/skills-catalog.md) |
| `mb-doc-check` | 5.6 | WSL (commit hook) | Keeps documents and links correct | [[[mybizz-config-docs:mb-doc-check/doc-check-explainer]]](C:/mybizz/mybizz-config-docs/mb-doc-check/doc-check-explainer.md) |
| MarkText | 5.6 | Windows | The document editor | [[[mybizz-os-docs:os-reference-docs/documentation-linking-explainer]]](C:/mybizz/mybizz-os-docs/os-reference-docs/documentation-linking-explainer.md) |
| GitHub | 5.6 | External | Off-machine copy of every repository | [[[mybizz-os-docs:os-reference-docs/repo-organization]]](C:/mybizz/mybizz-os-docs/os-reference-docs/repo-organization.md) |
| `mb-sysdoc-creator` | 5.6 | WSL | Writes component document suites | [[[mybizz-config-docs:mb-sysdoc-creator/sysdoc-creator-explainer]]](C:/mybizz/mybizz-config-docs/mb-sysdoc-creator/sysdoc-creator-explainer.md) |
| `mb-project-docs` | 5.6 | WSL | Keeps project documents current | [[[mybizz-config-docs:mb-project-docs/project-docs-explainer]]](C:/mybizz/mybizz-config-docs/mb-project-docs/project-docs-explainer.md) |
| Commit all (`commit-all.sh`, `sync-gbrain.sh`) | 5.7 | WSL | Commit, refresh GBrain, push | [[[mybizz-os-docs:operating-procedures/commit-all-repositories]]](C:/mybizz/mybizz-os-docs/operating-procedures/commit-all-repositories.md) |
| `/mb-close-out` | 5.7 | WSL | Ends a session and its task | [[[mb-close-out:mb-close-out]]](C:/mybizz/mb-close-out/mb-close-out.md) |
| SundayBackup | 5.7 | Windows → WSL | Weekly database backup and GBrain export | [[[mybizz-os-docs:operating-procedures/daily-ops]]](C:/mybizz/mybizz-os-docs/operating-procedures/daily-ops.md) |
| DailyDevBackup | 5.7 | Windows | Daily copy of development folders | [[[mybizz-os-docs:operating-procedures/daily-ops]]](C:/mybizz/mybizz-os-docs/operating-procedures/daily-ops.md) |
| OpenCodeSessionPrune | 5.7 | Windows → WSL | Keeps OpenCode's history bounded | [[[mybizz-os-docs:operating-procedures/daily-ops]]](C:/mybizz/mybizz-os-docs/operating-procedures/daily-ops.md) |
| MissedRunCheck | 5.7 | Windows | Reports missed or unfinished jobs | [[[mybizz-os-docs:operating-procedures/daily-ops]]](C:/mybizz/mybizz-os-docs/operating-procedures/daily-ops.md) |
| `mb-memory-system-diagnostic` | 5.7 | WSL | Weekly whole-memory health check | [[[mybizz-config-docs:mb-memory-system-diagnostic/mb-memory-system-diagnostic-explainer]]](C:/mybizz/mybizz-config-docs/mb-memory-system-diagnostic/mb-memory-system-diagnostic-explainer.md) |
| `mb-config-manager` and exam skills | 5.7 | WSL | Specification-against-reality exams | [[[mybizz-config-docs:mb-config-manager/config-manager-explainer]]](C:/mybizz/mybizz-config-docs/mb-config-manager/config-manager-explainer.md) |
| `opencode-db-cleanup.sh`, cache reset, clean shutdown | 5.7 | WSL | Maintenance procedures | [[[mybizz-os-docs:operating-procedures/daily-ops]]](C:/mybizz/mybizz-os-docs/operating-procedures/daily-ops.md) |
| Everything (`es.exe`), mgrep | 5.7 | Windows | File and content search | This document |
| n8n | 5.7 | Windows | Available for process linking; unused | This document |
| Google Calendar | 5.7 | External | Maintenance reminders | This document |
| PDLF | 3 | WSL and Windows | The build framework | [[[pdlf:pdlf-library/pdlf-explainer-docs/pdlf-explainer]]](C:/pdlf/pdlf-library/pdlf-explainer-docs/pdlf-explainer.md) |
| The PDLF ledger | 3 | Windows (Instance A) | PDLF's single source of truth | [[[pdlf:pdlf-library/specifications-local/spec-postgres-ledger]]](C:/pdlf/pdlf-library/specifications-local/spec-postgres-ledger.md) |
| mb-3-cs (MyBizz CS) | 10 (output, not a component) | Anvil | The MyBizz product, to be built through PDLF | [[[mb-3-cs-project-library:docs-local/positioning-strategy]]](C:/dev/dev-mb-3-cs/mb-3-cs-project-library/docs-local/positioning-strategy.md) |
| makePDLF | 3 (builder, not a component) | Windows (repository) | The project that builds PDLF | [[[pdlf:pdlf-library/pdlf-explainer-docs/app-factory-explainer]]](C:/pdlf/pdlf-library/pdlf-explainer-docs/app-factory-explainer.md) |

---

## 12. Where to read more

| To understand | Read |
|---|---|
| PDLF in full | [[[pdlf:pdlf-library/pdlf-explainer-docs/pdlf-explainer]]](C:/pdlf/pdlf-library/pdlf-explainer-docs/pdlf-explainer.md) |
| The memory family as a whole | [[[mybizz-os-docs:os-reference-docs/memory-master-explainer]]](C:/mybizz/mybizz-os-docs/os-reference-docs/memory-master-explainer.md) |
| Daily practice | [[[mybizz-os-docs:operating-procedures/daily-ops]]](C:/mybizz/mybizz-os-docs/operating-procedures/daily-ops.md) |
| Closing a session | [[[mybizz-os-docs:operating-procedures/opening-and-closing]]](C:/mybizz/mybizz-os-docs/operating-procedures/opening-and-closing.md) |
| Facts | [[[mybizz-os-docs:operating-procedures/fact-save-and-recall]]](C:/mybizz/mybizz-os-docs/operating-procedures/fact-save-and-recall.md) |
| Committing | [[[mybizz-os-docs:operating-procedures/commit-all-repositories]]](C:/mybizz/mybizz-os-docs/operating-procedures/commit-all-repositories.md) |
| Repositories and sources | [[[mybizz-os-docs:os-reference-docs/repo-organization]]](C:/mybizz/mybizz-os-docs/os-reference-docs/repo-organization.md) |
| Installed skills | [[[mybizz-os-docs:os-reference-docs/skills-catalog]]](C:/mybizz/mybizz-os-docs/os-reference-docs/skills-catalog.md) |
| Configuration management | [[[mybizz-config-docs:mb-config-manager/config-manager-explainer]]](C:/mybizz/mybizz-config-docs/mb-config-manager/config-manager-explainer.md) |

---

## Appendix A — Diagram plan

Each diagram is one scope for `mb-archify-scope`: one main path, at most twelve primary nodes.

| # | Diagram | Source section | Main path | Primary nodes |
|---|---|---|---|---|
| D1 | The purpose | 1 | developer → Claude → OpenCode → MyBizz-OS → PDLF → client applications | Developer, Claude, OpenCode, memory, enforcement, records, PDLF, Anvil applications |
| D1A | What makes it distinctive | 1A | baseline → MyBizz-OS additions | Stages with approval, ledger, receipts, compounding memory, governed memory, enforcement, platform knowledge, security review, BFT |
| D2 | The four layers | 2 | pre-makePDLF → makePDLF → PDLF → shipped estate | The four layers, with makePDLF marked as builder |
| D3 | The PDLF pipeline | 3 | five phases in order, gates between them | Five phases, seven gates, ledger, GBrain, learning junction |
| D4 | The machine and the bridge | 4 | Windows ↔ bridges ↔ WSL | Repositories, Instance A, Task Scheduler, browser, shared drive, PowerShell bridge, port relay, OpenCode, Cupcake, GBrain, governor, Instance B |
| D5 | The component families | 5 | OpenCode at the centre, families around it | Harness, memory, enforcement, knowledge, skills, documentation, operations, PDLF |
| D6 | A working session | 6 | start → rules → memory → work → close-out → commit all | Developer, OpenCode, AGENTS.md, GBrain, Cupcake, close-out, commit hook, GitHub |
| D7 | How knowledge moves | 6 | file → `mb-submit-memory` → governor → durable facts | Repositories, sync, mirror, fact file, `mb-submit-memory`, governor, review queue, durable facts, dream cycle |
| D8 | The maintenance cycle | 8 | session → daily → weekly → on demand | Close-out, commit all, daily jobs, logon check, Sunday backup, diagnostic, dream cycle, exams |
| D9 | Silent-failure detection | 9 | failure → detector → where it shows | Failure classes, detectors, the developer's view |
| D9A | System-level nuances | 7A | coupling → how it is handled | Two servers, name, startup, default source, facts per source, quick path, dream cycle, MCP load, Cupcake tests, local-only configuration |
| D10 | The shipped estate | 10 | workspace → master → blank template → client applications | The five applications, isolated client databases |
