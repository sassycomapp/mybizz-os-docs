---
document: "Active PDLF To-Do List"
doc-id: dev-makepdlf-todo
state: Live
date-created: 2026-09-05
---

# Active PDLF To-Do List

Action items only. Completed and closed items are not tracked here.

---

### 1. Rebuild Steps 42, 42A, 43, 44

These steps currently model a staging/production environment split that does not apply to this platform. Rebuild them around the actual publishing mechanism: a `develop` → `stable` GitHub merge, performed by the operator, outside PDLF's own pipeline. See `adr-local/no-staging-environment-five-app-isolation.md` for full reasoning. Deferred to Phase E.

### 2. Integrate Suite A and Suite B into Steps 35A, 36, 37, 38, 39

Both testing checklists (`chk-pdlf-self-test.md`, `the retired chk-anvil-app-testing.md`) exist and are referenced from `pol-testing-and-quality.md`, but their checks are not yet built into the actual stepwise files that do testing work. Deferred to Phase D, when those step files are rebuilt.

### 3. Build out `Instructions.md` files

Approximately 20 placeholder files in `C:\dev\project-template\`. Confirmed non-blocking and low priority — these files have no functional role in PDLF's operation. Revisit when convenient.

### 4. Define a recheck cadence for `project-library-global`

Only one file (`spec-testing-methodology-standards.md`) has been reviewed out of 98. Confirmed mostly out of scope for dev-makepdlf's own housekeeping — most of it concerns the Anvil application brand, not PDLF. No action planned unless a specific need surfaces (e.g., building Suite B's next iteration, or a step file that references a specific global file).

### 5. Build the global-content query-fetch-apply sequence into relevant step files

Rule and enforcement mechanism decided 2026-07-19 — see
`adr-local/global-content-discovery-and-application-discipline.md` and
`pol-memory-and-knowledge.md`. `project-library-global`'s GBrain registration is confirmed
working, so the mechanism is usable, not just designed. Remaining work: build the
five-step sequence and citation requirement into each relevant step file (17, 20, 22, 26,
27, 35, 35A, 37, 38, 41–45 at minimum) as that step is written or rebuilt. Deferred to
Phase B onward, same treatment as item 1.

### 6. Step 11 — include Cupcake setup for new projects

Step 11 (project init) needs a real addition: running the appropriate `cupcake init` sequence as part of setting up a new project, matching the real, live pattern already established for `makepdlf-project-library`. Applies once a new PDLF-managed project actually starts — see `cupcake-config.md` for current project-init scope and status.

### 7. Deterministic server-code dependency checker — hard gate

`GBrain`'s own symbol resolution only resolves same-file references by design — it cannot trace cross-file calls, the majority case in any real codebase. Decision already made: implement via `Graphify`, scoped to `server_code/` only (`client_code/` explicitly out of scope — Anvil's RPC/event-handler wiring is string-based and invisible to any static analyzer regardless). Build as a standing gate inside the existing `code-plan-build` workflow's "Mandatory checks before edits" step, authored once in PDLF so every future PDLF-managed project inherits it automatically. Not yet done: actual gate implementation and wiring into PDLF's step/gate definitions.

### 8. Retrofit each new PDLF step: governed memory writes + dream-cycle housekeeping

Two related, recurring per-step items, applied together as each new step file is built:

- **Governed memory writes.** Every step's own housekeeping question ("sync/embed GBrain?") must invoke the real, governed pipeline (`mb-submit-memory`, using the `pdlf_step_sync` credential) rather than the older, ungoverned raw sync. Already applied to Steps 11, 12, 14, 15, 16. Needs applying to every step from 17 through 46 (excluding the retired Step 13) as each is actually built.
- **Dream-cycle discipline.** At the end of every step, after the governed memory-write sequence, run: `gbrain dream` for real (never `--dry-run`), then `gbrain eval suspected-contradictions` immediately afterward (does not run automatically), then review the findings. Already applied to the same five steps (11, 12, 14, 15, 16). Same remaining scope: Steps 17–46 as each is built.

The final, real end-to-end proof that this all works together in practice is tracked separately — see `deferred-and-unresolved-matters.md`, Entry 2 — since it requires PDLF itself to be a real, running system before it can be tested.

### 9. Relook the testing suite implemented in PDLF

Connects directly to Item 2 above (Suite A/Suite B integration into the testing step files) — review together, not separately. Real, preserved seed material exists from an earlier MB-3-CS testing iteration (test taxonomy, a working Anvil-context test technique, real Anvil/M3-specific pass/fail criteria) — see `pdlf-testing-seed-material-README.md` for what's there and how to use it when this work is actually picked up.

---

## Deferred by explicit agreement, not tracked as action items

- Steps 17–46 (excluding 42/42A/43/44 above) — audited and rebuilt one at a time as each phase is reached, same rigor as Steps 11/12/14/15/16.
- Full cross-document consistency pass — complete for everything in this session's defined scope (stepwise shared references, specs, policies, docs-local, root config, templates). Not extended to `project-library-global` or Steps 17–46, both deliberately out of scope for now.
