---
title: "environment-explainer"
doc-id: "mb-849fc961"
updated: "2026-10-01"
---
# The Environment Explainer — the pre-makePDLF, makePDLF, PDLF, and shipped layers

*Created 2026-09-09 from the Sentinel scoping side conversation. This is the
orientation document for the layered environment: what each layer is, who
each layer serves, how they connect, and the trip hazards that have
actually caught us. Read this before any work that spans more than one
layer.*

---

## 1. Why this document exists

Work on this environment kept producing the same confusion: a proposal
valid in one layer was justified in another layer's language, and the error
only surfaced when reality contradicted it. The fix is a fixed vocabulary
and a fixed map, agreed between the developer and the agents, so that every
decision names its layer first.

**The naming discipline.** The layers are called by what they are:

- **the pre-makePDLF layer** — where we are now
- **the makePDLF layer** — the dev project that is building the PDLF application
- **the PDLF layer** — the finished, working PDLF application
- **the shipped estate** — the Anvil applications that PDLF builds

Never "level one/two/three", never "world one/two", and never "the
foundation" as a layer name — every layer is foundational to the next one
up, so the word carries no information and invites drift. If a sentence
needs "foundational" to make sense, rewrite it to name the actual layer.

---

## 2. The pre-makePDLF layer — where we are now

**What it is.** The layer of supporting systems built before and beneath
the PDLF application. It exists to make the building of PDLF (and, later,
of web applications through PDLF) trustworthy and efficient.

**What it contains (all verified live 2026-09-08/09):**

- **The memory system**: GBrain (semantic memory, Postgres, MCP), the
  memory governor (the gate for durable-memory writes, two processes: API
  + worker), `mb-submit-memory` (the governed submission path),
  memory-functionality-audit (the health-check procedure, reports to
  `C:\mybizz\memory-audit-reports\`).
- **The security systems**: the security-global library (54 documents, seven
  categories, `sec-` prefix — the authority on what "secure" means here);
  Cupcake (mechanical policy enforcement, reactivated 2026-09-08); Sentinel
  (step-triggered security review — see §5 for its cross-layer position);
  `mb-compile-rules` (the rule-authoring pipeline).
- **The supporting tooling**: Graphify (code/document structure knowledge),
  GStack and the other installed skill sets, the documentation suites in
  `mybizz-config-docs`, the master explainer in `mybizz-os-docs`, and this
  document in `mybizz-os-docs`.

**Who it serves.** The developer and the agents, now. Nothing in this
layer ships to clients or to Anvil. Its outputs flow *into* higher layers
through the PDLF process — as reviewed code, as built-in checks, as
configuration.

**The standing caution.** Systems in this layer must not be assumed present
anywhere else. GBrain, the governor, Cupcake, and the library do not travel
to Anvil, do not exist inside client instances, and are not available at
runtime. Anything built for a higher layer must be self-sufficient.

---

## 3. The makePDLF layer — building the PDLF application

**What it is.** The dev project (`makePDLF`, at
`C:\dev\dev-makepdlf\makepdlf-project-library`) whose entire purpose is to
produce the PDLF application. When it is done, makePDLF ceases to exist as
a project — PDLF is what remains.

**What it contains.** The PDLF application's own materials: the stepwise
step files (46 steps, 36 real — the first ten are empty), the gates, the
Postgres ledger, the specifications, checklists, registers. Some step files
are built (e.g. [[[makepdlf-project-library:stepwise/step-16-domain-modeling|Step 16 Domain Modeling]]](C:/dev/dev-makepdlf/makepdlf-project-library/stepwise/step-16-domain-modeling.md)); the rest are built one at a
time as each phase is reached — never in bulk.

**Who it serves.** The PDLF layer, next. Everything in this layer exists to
become part of PDLF or to be consumed by it.

---

## 4. The PDLF layer — the working harness

**What it is.** The finished PDLF application: the Project Development
Lifecycle Framework through which web applications get built, step by step,
with gates, reviews, QA, triage, and verification. It runs on this PC. It
does not "go live" or travel anywhere — it is the harness in which the
shipped estate gets created.

**What it contains.** The 46-step pipeline across five phases (Discover &
Define → Design & Plan → Scaffold & Assemble → Build & Verify → Release),
the seven gates, the thirteen-point execution sequence that every step
follows, the Postgres ledger, and — once the steps are built — the
Sentinel review checkpoints wired into the steps that need them.

**Who it serves.** The developer, during the building of each web
application. PDLF is a development tool; the client has no use for it and
never sees it.

---

## 5. The shipped estate — the Anvil applications

**What it is.** The five-app structure on Anvil.works, per
[[[default:adr-global/adr-client-instance-architecture|Client Instance Architecture]]](C:/dev/project-library-global/adr-global/adr-client-instance-architecture.md) and the related ADRs:

1. **`{slug}`** — the development workspace (all feature work).
2. **`master_template`** — the sole source of all server-side logic and UI
   forms for every client instance; tracks the `stable` GitHub branch.
3. **`blank_client_template`** — the provisioning clone source.
4. **`[client-name]-cs`** — one per paying client.
5. **`Mybizz_management`** — platform-operator tooling (deferred, post-launch).

**Who it serves.** The clients and their users. This is the only layer that
exists for them.

**What actually travels.** Verified against the real `mb-3-cs` scaffold
(2026-09-09): of everything in the dev workspace, only these go to Anvil —

- `anvil.yaml`, `client_code`, `server_code`, `theme` (with `theme/assets`
  the only file channel that travels), and possibly `__init__.py` and
  `.anvil_editor.yaml`.

Everything else — `.cupcake`, `.git`, `.opencode`, `graphify-out`, `docs`,
`tests`, `agents.md`, the whole dev estate — stays on disk. **The shipping
boundary is itself a review responsibility**: anything the code references
must actually ship in `theme/assets`, and anything that must not ship
(credentials, dev files) must not be inside the four traveling items.

**Structural isolation.** Per [[[default:adr-global/adr-dependency-based-not-multi-tenant]]](C:/dev/project-library-global/adr-global/adr-dependency-based-not-multi-tenant.md)
(Sealed): each client is a separate Anvil app with its own database —
cross-client data access is **structurally impossible**, not enforced by
code. `app_tables` in a dependency server module resolves to the calling
client instance's own tables (live-tested 2026-06-13). Multi-tenancy
patterns (tenant discriminator columns, tenant-filtered queries, tenant
session context) are forbidden — they would replace structural isolation
with a failure-prone filter, and AI assistants default to them, so the ADR
must be given as context before any code generation.

---

## 6. How the layers connect

```
the pre-makePDLF layer                the makePDLF layer            the PDLF layer
(supporting systems,                  (building the PDLF            (the working harness —
 docs, knowledge, gates)   ──────►    application, step by step) ─► builds web applications)
                                                                        │
                                                                        ▼
                                                              the shipped estate
                                                              (Anvil apps, per client)
```

- The pre-makePDLF layer is buildable now, in full, independently.
- The makePDLF layer consumes the pre-makePDLF layer (the memory system,
  the library, the methodology, the review pipeline) and produces PDLF.
- The PDLF layer consumes makePDLF's output and produces the shipped
  estate, step by step.
- The shipped estate inherits everything through `master_template` — build
  once there, every client instance receives it automatically.

---

## 7. Sentinel's position — spanning three layers

Sentinel is the clearest example of a system that crosses layers, which is
why it caused the flux confusion:

- **Defined and scoped in the pre-makePDLF layer** (now): the skill, the
  security-global library it reads, the reasoning methodology, the findings
  database. All built, verified, usable.
- **Developed and wired in the makePDLF layer**: the step-intensity wiring,
  the step-file retrofits (invocation + the gate-proof check), replicated
  into every Sentinel-covered step as each is built. Step 16 is done as the
  pattern; Steps 17–46 are pending as each is built.
- **Fires in the PDLF layer**: the reviews run when real projects go
  through real steps — the point at which Sentinel can actually be tested
  against a real feature.

Every Sentinel-covered step file must make the review of Sentinel's own
operation a canonical part of itself: the verified `triage_findings` row
before the gate ("no row, no gate"), and the three-kind flag triage (see
§8).

---

## 8. Trip hazards — the potholes actually encountered

These are not hypothetical. Each one was hit, or nearly hit, in real work:

**The estate-audit pothole.** Proposing a "periodic structural audit of the
deployed five-app system" run from the pre-makePDLF layer. Wrong: the
deployed instances are reachable only through Anvil, not through disk. The
correct split: dev-side structural checks (pre-makePDLF/makePDLF — review
and gate what gets built) and runtime self-checks built *into* the apps
(shipped estate — e.g. the client-instance startup module verifying its own
structural emptiness). Never propose an audit of a layer from outside it
without naming which layer runs the audit.

**The shipping-boundary pothole.** Assuming that what we build is what
ships. Only four items travel; `theme/assets` is the only file channel;
anything code references must actually be inside it. A support file left on
disk does not ship, and a credential accidentally inside the traveling four
does.

**The runtime self-sufficiency pothole.** Assuming GBrain, the memory
system, or any pre-makePDLF support is available at runtime. It is not:
anything built into `master_template` must run with no GBrain, no disk, no
support stack. The runtime layer is self-sufficient by design constraint —
simple, secure, robust, fit for purpose, in isolation.

**The two-databases pothole.** `localhost:5432` is a different server on
each side: WSL runs PostgreSQL 16 (GBrain's `gbrain` database, the memory
governor's `memory_governor` database); Windows runs PostgreSQL 15 (the
PDLF `pdlf` ledger). A psql call made from WSL without the documented
PowerShell bridge **silently connects to the wrong server** — it does not
fail visibly. All ledger access goes through the bridge, always.

**The multi-tenancy default.** AI assistants default to multi-tenancy
patterns when building SaaS-style code. In this platform they are
forbidden by sealed ADR — and they are *less secure* here, because they
would replace structural isolation with a failure-prone filter. The ADR
must be supplied as context before code generation.

**The silent-cleanup pothole.** Chained shell commands that do not verify
each result before the next runs. A failed step that does not halt produces
silent writes into the wrong places (the recorded real incident: a
permanent, factually false fact written into live memory). In Python:
`subprocess.run(..., check=True)`; unconditional `try`/`finally`
restoration; standing regression tests for cleanup.

**The blanket-sync pothole.** `gbrain sync` can report success while
individual sources remain stale — verify per source (`gbrain sources
status`) and run targeted `gbrain sync --source <id>` for anything lagging.

**The false-alarm doctrine.** Over-warning is preferred to silence. When an
alarm fires: a real event of minimal consequence is legitimate (triage and
set aside); an alarm raised with no cause is a **defect in the detection
logic itself** and must be corrected through the rule pipeline. The two are
never conflated, and repeated no-cause alarms mean the detector gets fixed.

**The flag taxonomy.** Every flag is triaged into exactly one of three
kinds: legitimate-minimal (set aside), illegitimate (a detector defect —
fix the detector), legitimate-requires-attention (decide and implement).
The flag is a snake in the grass: misclassified flags corrupt trust in the
whole system.

---

## 9. Standing rules

1. **Name the layer before the decision.** Every proposal, fix, or new
   idea states which layer it belongs to.
2. **The shipped estate runs self-sufficient.** Anything built into
   `master_template` or the client instances depends on nothing outside
   the four traveling items.
3. **The ledger is bridge-only.** Never direct WSL psql to the PDLF
   database.
4. **The ADR travels as context.** No code generation for the five-app
   platform without [[[default:adr-global/adr-dependency-based-not-multi-tenant]]](C:/dev/project-library-global/adr-global/adr-dependency-based-not-multi-tenant.md) supplied
   first.
5. **Every governed system has a health procedure** — e.g.
   memory-functionality-audit for the memory system — and the audit
   reports are dated, saved, and appended with their remediation.
6. **Nothing ships silently.** The shipping boundary is checked, not
   assumed — in both directions.

---

## 10. Companion documents

- [[[mybizz-os-docs:os-reference-docs/memory-master-explainer]]](C:/mybizz/mybizz-os-docs/os-reference-docs/memory-master-explainer.md) (mybizz-os-docs repo: `C:\mybizz\mybizz-os-docs\`) — the memory system's four
  mechanisms and the governor's processes.
- memory-functionality-audit (mybizz-os-docs repo: `C:\mybizz\mybizz-os-docs\`) — the memory-system health
  procedure.
- `mybizz-config-docs/` — the component suites: `memory-governor/`,
  `mb-submit-memory/`, `mb-compile-rules/`, `mb-sentinel-review/`,
  `graphify/`, `gstack/`, `cupcake/`, and the rest.
- [[[default:adr-global/adr-dependency-based-not-multi-tenant]]](C:/dev/project-library-global/adr-global/adr-dependency-based-not-multi-tenant.md) and
  [[[default:adr-global/adr-client-instance-readme-five-app-system]]](C:/dev/project-library-global/adr-global/adr-client-instance-readme-five-app-system.md)
  (project-library-global/adr-global) — the structural-isolation and
  client-instance documentation ADRs.
- [[[makepdlf-project-library:specifications-local/spec-postgres-ledger]]](C:/dev/dev-makepdlf/makepdlf-project-library/specifications-local/spec-postgres-ledger.md) (makepdlf-project-library) — the ledger
  schema and the bridge connection method.

*End of document.*
