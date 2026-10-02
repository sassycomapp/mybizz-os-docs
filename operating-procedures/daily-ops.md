---
title: "daily-ops"
doc-id: "mb-2d7af6f7"
updated: "2026-10-02"
---
# Daily Operations — Quick Reference

---

## START OPENCODE

```bash
cd /mnt/c/dev/dev-makepdlf/makepdlf-project-library
opencode web --hostname 0.0.0.0  <<Check the model>>
```

Then open browser: `http://localhost:3000`

---

## STOP / RESTART OPENCODE

```bash
pkill -f "opencode web"
```

Then run Start sequence above.

---

## Kill WSL:

In PowerShell:

PS C:\Users\dev-p> wsl --shutdown

## CROSS-PROJECT FILE ACCESS — GRANT / REVOKE

```bash
mg-grant --path <folder> --count N
```

`<folder>`: WSL forward-slash form (`/mnt/c/...`) — required, not optional.
Windows backslash paths are consumed as escape characters and can never
match (verified 2026-09-11). `N`: writes to allow.
Reads open for 30 minutes, no count needed. Closes automatically at 30
minutes or when the write count hits zero, whichever's first.

```bash
mg-revoke --path <folder>
```

Check active grants: `cat ~/.config/cupcake/policies/opencode/cup-grants-data.rego`

Full mechanics: [[[mybizz-config-docs:cupcake/cupcake-reference|Usage, Invocation, and Worked Examples]]](C:/mybizz/mybizz-config-docs/cupcake/cupcake-reference.md), [[[mybizz-config-docs:cupcake/cupcake-config|Cupcake Configuration]]](C:/mybizz/mybizz-config-docs/cupcake/cupcake-config.md).

---

## END OF A SESSION — CLOSE OUT

Say "close out" when you finish working in a project. The AI then:

1. Writes a short session page for GBrain and lists the durable facts of the session.
2. Runs `/mb-project-docs` for the project you are in. It creates or updates the project's config, its `AGENTS.md` (through a reference file) and its explainer, and creates or amends the README that points to them.
3. Shows everything once, for one approval: the facts, the sessions-log row, the changed files, the changes to the project documents and the changed lines of `AGENTS.md`.

After you approve, the AI gives you one block to paste. It adds the log rows, copies the project's reference file over its `AGENTS.md` (`agents-from-reference.sh`), and runs `commit-all.sh`. The AI never commits, never pushes and never writes `AGENTS.md` itself. Procedures: [[[mybizz-os-docs:operating-procedures/close-out-process]]](C:/mybizz/mybizz-os-docs/operating-procedures/close-out-process.md) and [[[mybizz-config-docs:mb-project-docs/project-docs-reference]]](C:/mybizz/mybizz-config-docs/mb-project-docs/project-docs-reference.md). In a code repository the three files are in `theme\docs` and you commit them yourself. If the Anvil app does not exist yet, the skill stops and tells you to create it.

---

## AFTER DOING WORK — run every time

At the end of a session in a project, use close out (above); it ends with this command. For other documentation work, one command commits, updates GBrain and pushes all eight documentation repositories:

```bash
bash /mnt/c/mybizz/scripts/commit-all.sh "what you did"
```

Run it from WSL after any documentation work, in any repository. It takes a few minutes, because GBrain re-reads what changed. The full procedure, what to do when a repository is blocked, and how to add a new repository: [[[mybizz-os-docs:operating-procedures/commit-all-repositories]]](C:/mybizz/mybizz-os-docs/operating-procedures/commit-all-repositories.md). To see first what would be committed: `bash /mnt/c/mybizz/scripts/commit-all.sh --status`.

**The commit hook (`mb-doc-check`) runs inside every commit.** It gives every document its title, permanent `doc-id` and `updated` date, writes every link in the standard form, repairs links after a rename or move, turns links to a deleted document into plain words, and stores the files with LF line endings. It stops a commit only when a link you just typed points to nothing or to a name several documents share, or when the frontmatter is damaged; it prints the line. Never skip it with `--no-verify`. How it works and why: [[[mybizz-os-docs:os-reference-docs/documentation-linking-explainer]]](C:/mybizz/mybizz-os-docs/os-reference-docs/documentation-linking-explainer.md).

As of 2026-08-05, every PDLF/dev-makepdlf-ecosystem source is GitHub-backed
(see the [[[default:adr-global/adr-dev-tooling-source-repos-must-be-github-backed|All Dev-Tooling Source Repositories Must Be GitHub-Backed]]](C:/dev/project-library-global/adr-global/adr-dev-tooling-source-repos-must-be-github-backed.md) global ADR).
There is one workflow — no more "does this repo have a remote?" check.
The only thing that varies per repo is the default branch name.

**Repositories `commit-all.sh` does not cover.** Commit these by hand: the template repository below and the two dormant libraries (`mb4ecom-project-library`, `mb5pdlf-project-library`).

| Repo                       | Local path                                                      | GitHub                                                                         | Default branch |
| -------------------------- | --------------------------------------------------------------- | ------------------------------------------------------------------------------ | -------------- |
| `template-project-library` | `/mnt/c/dev/project-template/dev-(SLUG)/(slug)-project-library` | `github.com/sassycomapp/template-project-library` (private, **template repo**) | `master`       |

```bash
cd "/mnt/c/dev/project-template/dev-(SLUG)/(slug)-project-library"
git add -A
git commit -m "your message here"
git push origin master
gbrain sync --source template-project-library
gbrain embed --stale
```

**`template-project-library` is a GitHub template repository, not a normal
project.** Never develop a new project inside this folder directly — use
GitHub's "Use this template" (or `gh repo create <new-project> --template
sassycomapp/template-project-library`) to spin off a new project with its
own independent history. Only edit this repo directly when you're
deliberately changing the scaffold itself.

**New dev-tooling repos:** any new source added to gbrain for this ecosystem
must get a GitHub remote at creation — see the
[[[default:adr-global/adr-dev-tooling-source-repos-must-be-github-backed|All Dev-Tooling Source Repositories Must Be GitHub-Backed]]](C:/dev/project-library-global/adr-global/adr-dev-tooling-source-repos-must-be-github-backed.md) ADR for the exact
`gh repo create` sequence. Don't let a new source sit local-only.

**Other sources** (`mb4ecom-project-library`, `mb5pdlf-project-library`, the code repos, etc.)
— add their local_path, remote, and branch here once confirmed. Check with:

```bash
gbrain sources status
```

---

## RUN MB-ALIGN-DOCS AFTER A DOCS MANAGER RUN THAT DID STRUCTURAL CHANGES

If Docs Manager renamed, moved, or retired files within a project-library
(or project-library-global), run mb-align-docs against that same project
next — before considering the documentation environment stable:

```
/mb-align-docs (project-name)
```

This catches register The use of registers has been discontinued. Please review or front-matter drift that Docs Manager's structural
changes may have introduced. Skip if the Docs Manager run was text-only
corrections (no renames, moves, or retires).

---

## GBRAIN HEALTH CHECK

```bash
gbrain doctor
```

Expected result since 2026-09-30: overall **30/100**, and the only FAIL is `cycle_freshness` (dream cycles have not run for most sources — by design). Scoring: minus 20 per FAIL, minus 5 per WARN. A different FAIL, or a lower score with new FAIL lines, needs attention. The baseline is saved as `~/backups/doctor-after-sync2.txt`.

Never upgrade GBrain with `gbrain self-upgrade` or `gbrain upgrade` — the procedure is in [[[mybizz-config-docs:gbrain/gbrain-reference|Reference: Commands, Mechanics, Incidents, Troubleshooting]]](C:/mybizz/mybizz-config-docs/gbrain/gbrain-reference.md) → Upgrading.

---

## GBRAIN TEST — confirm search is working

```bash
gbrain query "client instance architecture"
```

Should return results. If toast error appears — see Fix below.

---

## POSTGRESQL SURVIVING A WSL RESTART

Auto-starts on boot (`/etc/wsl.conf`). If GBrain won't connect after a
restart, check this first:

```bash
pg_lsclusters
```

If `down`:

```bash
sudo pg_ctlcluster 16 main start
```

Details: [[[mybizz-config-docs:gbrain/gbrain-config|Configuration]]](C:/mybizz/mybizz-config-docs/gbrain/gbrain-config.md).

---

## FIX: GBRAIN WASM ERROR / WON'T START

Close the WSL terminal completely. Open a fresh one. Retry.

If still broken:

```bash
rm -rf ~/.gbrain/brain.pglite
gbrain init
gbrain import ~/gbrain-backup-20260615/
gbrain embed --stale
```

---

## FIX: GBRAIN SYNC SAYS "NO ALLOWLISTED CHANGES"

This is normal — means nothing new to sync from GStack working state.
Your git push already backed up the project-library. Nothing to worry about.

---

## FIX: `gbrain doctor` FAILS `sync_freshness` EVEN AFTER RUNNING `gbrain sync --all`

Usually means uncommitted local changes are sitting in a repo. Fix:

```bash
cd <repo-path>
git status --porcelain
```

If that shows output, commit and push it (see "AFTER DOING WORK"
above), then:

```bash
gbrain sync --source <source-id>
gbrain doctor
```

Full mechanic: [[[mybizz-config-docs:gbrain/gbrain-reference|Reference: Commands, Mechanics, Incidents, Troubleshooting]]](C:/mybizz/mybizz-config-docs/gbrain/gbrain-reference.md) → Troubleshooting.

---

## FIX: A SYNC ENDS WITH AN ERROR

Read the last run first:

```bash
LOG=$(ls -t /mnt/c/mybizz/logs/gbrain-logs/sync-*.log | head -1)
grep -E '^ERROR|not found|BLOCKED|Could not resolve' "$LOG" | cut -c1-200
```

- **`Source "<id>" not found or is archived`, for every source:** the brain default names a deleted source (seen 2026-09-30). `gbrain config get sources.default`, then `gbrain sources default default`.
- **`git pull failed … Could not resolve host`:** a passing network problem; the sync is incomplete and safe to retry. The script no longer pulls.
- **`Sync BLOCKED … 1 file(s) failed`:** one file has invalid frontmatter. `gbrain frontmatter validate <repo-path>` names it; quote the offending value (for example a title with a colon), commit, sync again.

Full mechanics: [[[mybizz-config-docs:gbrain/gbrain-reference|Reference: Commands, Mechanics, Incidents, Troubleshooting]]](C:/mybizz/mybizz-config-docs/gbrain/gbrain-reference.md) → Resolved Incidents.

---

## CHECK WHAT'S NOT COMMITTED YET

```bash
bash /mnt/c/mybizz/scripts/commit-all.sh --status
```

Shows the uncommitted changes of all eight repositories and changes nothing. `(nothing to commit)` means that repository is clean. Any lines printed mean uncommitted changes — see the `sync_freshness` fix above for why this matters.

---

## REBUILD THE DOCMAP

```bash
python3 /mnt/c/mybizz/scripts/build-docmap.py           # shows what would change, changes nothing
python3 /mnt/c/mybizz/scripts/build-docmap.py --write   # rewrites the folder trees of docmap.md
```

Only the folder trees are rebuilt. Existing descriptions are kept; new folders and files are added marked "(new — add a description)"; entries that no longer exist are removed. Commit the result with `commit-all.sh`.

---

## TEST mb-doc-check

```bash
bash /mnt/c/mybizz/mybizz-config-docs/gbrain/tests/run-tests.sh
```

It must end with `OK` (143 tests). Run it after any change to `mb-doc-check.py`, and with `slug-selftest` after a GBrain upgrade.

---

## PATTERN: BULK RETROFIT / MASS-EDIT TASKS (e.g. wikilinks, frontmatter passes)

An agent's own summary count of a multi-file retrofit is not reliable
evidence the work is correct. Always recount against the itemized
detail, and treat a post-apply diff check against backups as required,
not optional — it's what actually catches a phantom candidate (a
proposed change for text that isn't really there).

**Reference paths (wikilink retrofit):** skill `mb-wikilinks`; backups
`C:\backup-mb-wikilinks\`; logs `C:\mybizz\logs\mb-wikilinks\`.

---

## FIX: OPENCODE OPENS THE WRONG / STALE PROJECT

**Run this proactively, right after renaming or moving any project's own
root folder — don't wait for the symptom below to show up:**

```bash
bash /mnt/c/mybizz/scripts/opencode-db-cleanup.sh
```

On-demand only, deliberately not scheduled — a false positive (e.g. a
temporarily unmounted drive) could otherwise delete real history
unattended.

**Symptom (if this wasn't run proactively):** starting `opencode web`
opens a project that no longer exists, or was renamed long ago (e.g.
showing an old `C:\projects\...` path after the machine's layout moved
to `C:\dev\...`).

**Cause:** OpenCode has no built-in cleanup for stale `project`/`session`
database rows (confirmed upstream gap — see
[[[mybizz-config-docs:opencode/opencode-project-session-data-integrity|Project & Session Data Integrity]]](C:/mybizz/mybizz-config-docs/opencode/opencode-project-session-data-integrity.md)).

**Fix:** run the cleanup script — see that same document for the exact
command and full path.

---

## FIX: GSTACK SKILLS MISSING, OUTDATED, OR ONLY PARTIALLY WORKING IN OPENCODE

**Symptom:** typing a known GStack skill name in OpenCode chat does
nothing, or only a handful of skills work while most don't.

**Fix — re-run GStack's own installer for the OpenCode host:**

```bash
cd /mnt/c/mybizz/gstack && ./setup --host opencode
```

This regenerates and re-links every skill from the source repo. Safe to
re-run any time; it does not touch project files.

**GSTACK COMMAND NAMING — READ BEFORE TYPING A SKILL NAME.** The
installed folder is prefixed (`gstack-qa/`), but the actual slash
command is the **bare name** from that skill's own frontmatter —
`/qa`, not `/gstack-qa`. Confirmed directly: the prefixed form returns
nothing. See [[[mybizz-config-docs:gstack/gstack-skills-reference|GStack Skills Reference]]](C:/mybizz/mybizz-config-docs/gstack/gstack-skills-reference.md) for the full catalog using
correct names.

---

## FIX: GSTACK BROWSER SKILLS WON'T LAUNCH

**Symptom:** setup hangs after "Installing Playwright Chromium..." or
completes but browser skills (`/browse`, `/qa`, `/scrape`, `/canary`)
fail.

**Check 1 — Node version** (known bad: `24.16.0`, playwright#41000):

```bash
node --version
```

If `24.16.0`:

```bash
nvm install 24.15.0
nvm alias default 24.15.0
```

Verify: `nvm alias default` → `default -> 24.15.0 (-> v24.15.0)`.

**Check 2 — missing system libraries** (error like
`libnspr4.so: cannot open shared object file`):

```bash
sudo apt-get update
sudo apt-get install -y libnss3 libnspr4 libatk1.0-0 libatk-bridge2.0-0 \
  libcups2 libdrm2 libxkbcommon0 libxcomposite1 libxdamage1 libxfixes3 \
  libxrandr2 libgbm1 libasound2t64
```

**Verify both, without re-running full setup:**

```bash
node -e "const {chromium} = require('/mnt/c/mybizz/gstack/node_modules/playwright'); chromium.launch().then(b => { console.log('LAUNCHED OK'); b.close(); }).catch(e => console.error('LAUNCH FAILED:', e.message))"
```

**If a Chromium install was interrupted partway:**

```bash
rm -rf ~/.cache/ms-playwright/chromium-1208
```

(adjust version number to whatever the current attempt showed)

---

## FIX: GBRAIN CODE SOURCE HAS NO REAL .PY FILES DESPITE REAL CODE EXISTING

Symptom: dream-cycle symbol resolution finds nothing, or a code
source's page count is suspiciously low. Usually a missing `strategy`
field on the source (defaults to markdown-only, silently skips code).

Full diagnosis and 3-step fix: [[[mybizz-config-docs:gbrain/gbrain-reference|Reference: Commands, Mechanics, Incidents, Troubleshooting]]](C:/mybizz/mybizz-config-docs/gbrain/gbrain-reference.md) → Resolved
Incidents.

---

## PATTERN: AI AGENT FILE EDITS MAY LAND BEFORE THE PERMISSION PROMPT SHOWS

Symptom: a chat log shows an edit applied *above* a permission popup
asking to view that same file — the write happened before you were
asked to approve it. Never trust the agent's own summary in this case;
verify independently, outside the session:

```bash
git diff <file>
```

---

## FIX: A SCRIPT KILLS THE LIVE GBRAIN CONNECTION MID-SESSION

Symptom: running a GBrain sync wrapper mid-session unexpectedly breaks
search for the rest of that session. Usually a leftover PGLite-era
`pkill -f "gbrain serve"` step with no restart — no longer needed under
Postgres (multi-writer). Remove that step from any script.

Full story: [[[mybizz-config-docs:gbrain/gbrain-explainer|Explainer]]](C:/mybizz/mybizz-config-docs/gbrain/gbrain-explainer.md) §3.

---

## SCHEDULED TASKS RUNNING VIA WSL FROM WINDOWS TASK SCHEDULER

**Symptom:** a Task Scheduler job that calls `wsl.exe` runs against the
wrong Linux user/home directory, or fails outright with a
`WSL_E_USER_NOT_FOUND` error.

**Cause:** this machine has two WSL distros installed. A bare `wsl.exe`
call uses whichever distro is set as *default* — which is not
necessarily the one `dev-p` actually lives in.

**Fix:** always specify both the distro and the user explicitly in any
Task Scheduler action that invokes WSL:

```
wsl.exe -d Ubuntu -u dev-p bash <full-script-path>
```

Never register The use of registers has been discontinued. Please review a scheduled task with a bare `wsl.exe` call.

---

## OPENCODE SLASH COMMANDS & QUICK REFERENCE

### GBrain

| Command                | What it does                                               |
| ---------------------- | ---------------------------------------------------------- |
| `/setup-gbrain doctor` | GBrain health check (equivalent of `gbrain doctor` in WSL) |
| `/sync-gbrain`         | Sync GBrain state (equivalent of `gbrain sync` in WSL)     |

### Save and Restore Your Place

| Command            | What it does                                |
| ------------------ | ------------------------------------------- |
| `/context-save`    | Save where you are before closing OpenCode  |
| `/context-restore` | Pick up where you left off in a new session |

### Tell the Agent to Search GBrain

Add this to the start of any prompt:

```
Before starting, search GBrain for: [your topic here]
```

### MCP / GBrain Not Responding in OpenCode

Restart OpenCode from WSL:

```bash
pkill -f "opencode web"
cd /mnt/c/dev/dev-makepdlf/makepdlf-project-library
opencode web --hostname 0.0.0.0
```

---

## OPENCODE — USEFUL KEYS

| Key           | What it does                |
| ------------- | --------------------------- |
| Tab           | Switch Plan / Build mode    |
| Ctrl+G        | Stop the agent mid-response |
| Ctrl+X then N | New session                 |

---

## I-HAVE-ADHD SKILL (installed 2026-09-19, always-on)

The skill enforces action-first output: it leads with the next action, numbers
multi-step work, and suppresses tangents. Installed globally; the always-on
flag injects the ruleset into every new session.

| Action                             | Command                                                |
| ---------------------------------- | ------------------------------------------------------ |
| Invoke for one session             | `/i-have-adhd` in the OpenCode chat                    |
| Turn off mid-session               | type `stop adhd mode` or `normal mode` in the chat     |
| Turn always-on off for good        | `rm ~/.config/opencode/.i-have-adhd-always`            |
| Turn always-on back on             | `touch ~/.config/opencode/.i-have-adhd-always`         |
| One deep-design session without it | rename the flag file before starting, restore it after |
| Update the skill                   | `npx skills add ayghri/i-have-adhd -a opencode -y -g`  |
| Verify the whole wiring            | `node /mnt/c/mybizz/scripts/verify-i-have-adhd.mjs`    |

Install layout (do not move one piece without the others):

- Skill store: `~/.agents/skills/i-have-adhd/` (the `skills` CLI canonical copy).
- Discovery: `~/.config/opencode/skills/i-have-adhd` is a symlink to the store.
- Plugin: `~/.config/opencode/plugin/i-have-adhd.mjs`, registered in
  `~/.config/opencode/opencode.jsonc` under `"plugin"`.
- Command: `~/.config/opencode/command/i-have-adhd.md`.
- Path fix: `~/.config/skills` is a symlink to `~/.config/opencode/skills`;
  the plugin resolves its ruleset through it. Deleting it stops the always-on
  injection silently.
- Always-on flag: `~/.config/opencode/.i-have-adhd-always`.

Uninstall completely:

```bash
rm ~/.config/opencode/plugin/i-have-adhd.mjs
rm ~/.config/opencode/command/i-have-adhd.md
rm ~/.config/opencode/skills/i-have-adhd
rm ~/.config/skills
rm ~/.config/opencode/.i-have-adhd-always
rm -rf ~/.agents/skills/i-have-adhd
```

Then remove the `"plugin"` entry from `~/.config/opencode/opencode.jsonc` and
restart OpenCode. Config loads once at startup, so any wiring change needs a
restart to take effect.

---

| Role          | Provider                        | Status                           |
| ------------- | ------------------------------- | -------------------------------- |
| Embedding     | OpenAI (text-embedding-3-large) | ✓ active                         |
| Chat / Expand | OpenAI (gpt-5.2)                | ✓ active, will move to Anthropic |
| Reranker      | ZeroEntropy                     | ✓ confirmed active               |

## Cupcake (Enable/ Disable)

documented in `cupcake-reference.md:275` (Disabling Cupcake — immediate, fully reversible):

```
mv ~/.config/opencode/plugin/cupcake.js ~/.config/opencode/plugin/cupcake.js.disabled
```

Restart OpenCode afterwards (plugin changes only load at process start). To re-enable:

```
mv ~/.config/opencode/plugin/cupcake.js.disabled ~/.config/opencode/plugin/cupcake.js
```

This matches the prior suspension described in `cupcake-explainer.md:260` — renaming the plugin out of its discoverable path, nothing else touched, and the `.bak-*` copies in the plugin folder confirm earlier uses.

*That's it. Everything else lives in the reference docs.*
