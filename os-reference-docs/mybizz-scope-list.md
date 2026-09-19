**In scope:**

"C:\data-mybizz-mgt"

"C:\dev"

"C:\mybizz"

"C:\pdlf"

**Exceptions — one list, applies to every documentation skill (mb-align-docs, mb-wikilinks, mb-docs-manager):**

Criterion: an exception is content we never reference, in any form, at any time, ever.
Nothing else is excluded. Transient state (a project under development) is a scheduling
question — when to work on it — never a scope question. Transient projects stay in scope.

Not exceptions, by definition: version-control internals, dotfolders, and files with no
content are not documents; the skills' own treatment rules handle them. They are not
listed here. The same applies to treatment states inside scope (templates, quarantine,
registers) — they are handled by rules, not by this list.

This list is the single scope authority for the documentation skills. The skills read it
at Phase 0 of every run; they never carry their own scope lists.

Backups, archives, and retired material (snapshots, never live knowledge):

"C:\backup-general"
"C:\backup-mb-align-docs"
"C:\backup-mb-docs-manager"
"C:\backup-mb-wikilinks"
"C:\backup-wsl"
"C:\backup-daily"
"C:\backup-dream-cycle"
"C:\backup-gbrain"
"C:\mybizz\archive"
"C:\data-mybizz-mgt\archive"
"C:\dev\obsolete"

Private and exempt data:

"C:\_data-mybizz-exempt"
"C:\_data-not mybizz"

Vendor clones, live tooling internals, and executable plumbing (the installed skills are
the reference homes; the clones and scripts are supply):

"C:\mybizz\matt-pocock-skills-source"
"C:\mybizz\anvil-agent-references"
"C:\mybizz\archify"
"C:\mybizz\gstack"
"C:\mybizz\gbrain"
"C:\mybizz\scripts"

Transient working state (regenerated, wiped, or rewritten — never referenceable):

"C:\mybizz\logs"
"C:\pdlf\output"
any folder named WIP or wip, at any depth (includes "C:\data-mybizz-mgt\WIP")
any folder named temp, temp-*, workspace, pdlf-temp, or Frontmatter-prompts, at any
depth (includes "C:\data-mybizz-mgt\temp")

Disposable and template projects:

"C:\dev\dev-mb5pdlf" (disposable test project, reset repeatedly during PDLF development)
"C:\dev\project-template" (template source; real projects receive their content at Step 11 init)

Anvil code repositories — class rule: any folder containing an anvil.yaml, current or
future, is excepted (code is never documentation):

"C:\dev\dev-mb-3-cs\mb-3-cs"
"C:\dev\dev-mb4ecom\mb4ecom"

Notes:

- "C:\dev\dev-mb4ecom\mb4ecom-project-library" is IN scope. It is an empty placeholder
  today; membership is by content, so it activates when real content lands — no scope change needed.
- mb-3-cs-project-library, makepdlf-project-library, and all of C:\pdlf other than its
  exceptions above are IN scope despite being in development. Transient state schedules the work; it never removes scope.
