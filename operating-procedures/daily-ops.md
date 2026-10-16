---
title: "daily-ops"
doc-id: "mb-2d7af6f7"
updated: "2026-10-16"
---
# Daily Operations — Quick Reference

Your manual, in the order you need it. The reference documents hold the detail.

---

## 1. Start a session

### Start OpenCode

```bash
opencode web --hostname 0.0.0.0
```

```
cd /mnt/c/pdlf && opencode
```

```bash
cd /mnt/c/dev/dev-mb-3-cs/mb-3-cs-project-library && opencode web --hostname 0.0.0.0
```

```
pkill -f opencode; sleep 3; opencode web --hostname 0.0.0.0
```

Open `http://localhost:3000`, choose the project inside OpenCode, and check the model.

### STOP / RESTART OPENCODE

```bash
pkill -f "opencode web"
```

Then run Start sequence above.

### CROSS-PROJECT FILE ACCESS — GRANT / REVOKE

```bash
mg-grant --path <folder> --count N
```

`<folder>`: WSL forward-slash form (`/mnt/c/...`) — required, not optional.
Windows backslash paths are consumed as escape characters and can never
match. `N`: writes to allow.
Reads open for 30 minutes, no count needed. Closes automatically at 30
minutes or when the write count hits zero, whichever's first.

```bash
mg-revoke --path <folder>
```

Check active grants: `cat ~/.config/cupcake/policies/opencode/cup-grants-data.rego`

Full mechanics: [[[mybizz-config-docs:cupcake/cupcake-reference|Usage, Invocation, and Worked Examples]]](C:/mybizz/mybizz-config-docs/cupcake/cupcake-reference.md), [[[mybizz-config-docs:cupcake/cupcake-config|Cupcake Configuration]]](C:/mybizz/mybizz-config-docs/cupcake/cupcake-config.md).

---

## 2. Finish a session

### Close out

Run `/mb-close-out`. It writes the session page, saves the facts, updates the project documents, asks for your approval once, and gives you one terminal block that ends with commit all. A second block appears only when a governed fact needs your promotion. Procedure: [[[mybizz-os-docs:operating-procedures/opening-and-closing]]](C:/mybizz/mybizz-os-docs/operating-procedures/opening-and-closing.md).

### Commit all, outside a close-out

```bash
bash /mnt/c/mybizz/scripts/commit-all.sh "what you did"
```

It commits every repository on the commit-all list, updates GBrain and pushes. The commit hook (`mb-doc-check`) runs inside every commit; never skip it with `--no-verify`. To see first what would be committed: `bash /mnt/c/mybizz/scripts/commit-all.sh --status`. Procedure: [[[mybizz-os-docs:operating-procedures/commit-all-repositories]]](C:/mybizz/mybizz-os-docs/operating-procedures/commit-all-repositories.md).

### Template projects

New projects start from `C:\dev\project-template`, copied by hand when required.

---

## 3. Sunday maintenance

1. **10:00 — runs by itself.** The task `SundayBackup` backs up both PostgreSQL instances and exports GBrain. A pop-up reports the result. If it failed, the pop-up says what to do.
2. **After it finishes — in OpenCode.** Run `/mb-memory-system-diagnostic`. At its end it asks whether to run the dream cycle: answer yes. If the backup failed, answer no until it has succeeded.

Missed or unfinished scheduled runs are reported in a message at logon (`MissedRunCheck`). Configuration exams are not on Sunday; they are in your Google Calendar task list, for when something drifts or changes.

---

## 4. Checks you can run

### GBRAIN HEALTH CHECK

```bash
gbrain doctor
```

Expected result since 2026-09-30: overall **30/100**, and the only FAIL is `cycle_freshness` (dream cycles have not run for most sources — by design). Scoring: minus 20 per FAIL, minus 5 per WARN. A different FAIL, or a lower score with new FAIL lines, needs attention. The baseline is saved as `~/backups/doctor-after-sync2.txt`.

Never upgrade GBrain with `gbrain self-upgrade` or `gbrain upgrade` — the procedure is in [[[mybizz-config-docs:gbrain/gbrain-reference|Reference: Commands, Mechanics, Incidents, Troubleshooting]]](C:/mybizz/mybizz-config-docs/gbrain/gbrain-reference.md) → Upgrading.

### GBRAIN TEST — confirm search is working

```bash
gbrain query "client instance architecture"
```

Should return results.

### CHECK WHAT'S NOT COMMITTED YET

```bash
bash /mnt/c/mybizz/scripts/commit-all.sh --status
```

Shows the uncommitted changes of the documentation repositories listed in [[[mybizz-os-docs:operating-procedures/commit-all-repositories]]](C:/mybizz/mybizz-os-docs/operating-procedures/commit-all-repositories.md) and changes nothing. `(nothing to commit)` means that repository is clean. Any lines printed mean uncommitted changes — see the `sync_freshness` fix in section 5 for why this matters.

### REBUILD THE DOCMAP

```bash
python3 /mnt/c/mybizz/scripts/build-docmap.py           # shows what would change, changes nothing
python3 /mnt/c/mybizz/scripts/build-docmap.py --write   # rewrites the folder trees of docmap.md
```

Only the folder trees are rebuilt. Existing descriptions are kept; new folders and files are added marked "(new — add a description)"; entries that no longer exist are removed. Commit the result with `commit-all.sh`.

### TEST mb-doc-check

```bash
bash /mnt/c/mybizz/mybizz-config-docs/gbrain/tests/run-tests.sh
```

It must end with `OK`. Run it after any change to `mb-doc-check.py`, and with `slug-selftest` after a GBrain upgrade.

### Tell the Agent to Search GBrain

Add this to the start of any prompt:

```
Before starting, search GBrain for: [your topic here]
```

### OPENCODE — USEFUL KEYS

| Key           | What it does                |
| ------------- | --------------------------- |
| Tab           | Switch Plan / Build mode    |
| Ctrl+G        | Stop the agent mid-response |
| Ctrl+X then N | New session                 |

### Check documents with mb-doc-check

The hook runs it inside every commit, so day to day nothing is needed. To check one repository by hand:

```bash
python3 /mnt/c/mybizz/scripts/mb-doc-check.py check --root /mnt/c/mybizz/mybizz-os-docs
```

It must end with `all clear`. Replace the folder to check another repository. To confirm the hook's list of repositories matches GBrain: `python3 /mnt/c/mybizz/scripts/mb-doc-check.py sources-check`; it must say `everything matches`.

---

## 5. When something breaks

### Kill WSL:

1. In WSL: `systemctl --user stop memory-governor-api memory-governor-worker`
2. In WSL: `sudo pg_ctlcluster 16 main stop`
3. Confirm: `pg_lsclusters` shows the cluster `down`, and
   `systemctl --user is-active memory-governor-api memory-governor-worker`
   shows `inactive` for both.
4. In PowerShell: `wsl --shutdown`

`wsl --shutdown` stops Linux instantly; PostgreSQL then has to recover on
the next start — stopping the services first avoids that. Full procedure:
[[[mybizz-config-docs:postgresql/postgresql-reference|Reference: Commands, Procedures, and Troubleshooting]]](C:/mybizz/mybizz-config-docs/postgresql/postgresql-reference.md) → Clean shutdown.

### POSTGRESQL SURVIVING A WSL RESTART

Auto-starts with WSL (systemd, `postgresql.service`). If GBrain won't connect after a
restart, check this first:

```bash
pg_lsclusters
```

If `down`:

```bash
sudo pg_ctlcluster 16 main start
```

Details: [[[mybizz-config-docs:gbrain/gbrain-config|Configuration]]](C:/mybizz/mybizz-config-docs/gbrain/gbrain-config.md).

### FIX: `gbrain doctor` FAILS `sync_freshness` EVEN AFTER RUNNING `gbrain sync --all`

Usually means uncommitted local changes are sitting in a repo. Fix:

```bash
cd <repo-path>
git status --porcelain
```

If that shows output, commit and push it with commit all (section 2), then:

```bash
gbrain sync --source <source-id>
gbrain doctor
```

Full mechanic: [[[mybizz-config-docs:gbrain/gbrain-reference|Reference: Commands, Mechanics, Incidents, Troubleshooting]]](C:/mybizz/mybizz-config-docs/gbrain/gbrain-reference.md) → Troubleshooting.

### FIX: A SYNC ENDS WITH AN ERROR

Read the last run first:

```bash
LOG=$(ls -t /mnt/c/mybizz/logs/gbrain-logs/sync-*.log | head -1)
grep -E '^ERROR|not found|BLOCKED|Could not resolve' "$LOG" | cut -c1-200
```

- **`Source "<id>" not found or is archived`, for every source:** the brain default names a deleted source (seen 2026-09-30). `gbrain config get sources.default`, then `gbrain sources default default`.
- **`git pull failed … Could not resolve host`:** a passing network problem; the sync is incomplete and safe to retry. The script no longer pulls.
- **`Sync BLOCKED … 1 file(s) failed`:** one file has invalid frontmatter. `gbrain frontmatter validate <repo-path>` names it; quote the offending value (for example a title with a colon), commit, sync again.

Full mechanics: [[[mybizz-config-docs:gbrain/gbrain-reference|Reference: Commands, Mechanics, Incidents, Troubleshooting]]](C:/mybizz/mybizz-config-docs/gbrain/gbrain-reference.md) → Resolved Incidents.

### FIX: OPENCODE OPENS THE WRONG / STALE PROJECT

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
`C:\mybizz\mybizz-os\opencode-project-session-data-integrity.md`).

**Fix:** run the cleanup script — see that same document for the exact
command and full path.

### FIX: GSTACK SKILLS MISSING, OUTDATED, OR ONLY PARTIALLY WORKING IN OPENCODE

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

### FIX: GSTACK BROWSER SKILLS WON'T LAUNCH

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

### FIX: GBRAIN CODE SOURCE HAS NO REAL .PY FILES DESPITE REAL CODE EXISTING

Symptom: dream-cycle symbol resolution finds nothing, or a code
source's page count is suspiciously low. Usually a missing `strategy`
field on the source (defaults to markdown-only, silently skips code).

Full diagnosis and 3-step fix: [[[mybizz-config-docs:gbrain/gbrain-reference|Reference: Commands, Mechanics, Incidents, Troubleshooting]]](C:/mybizz/mybizz-config-docs/gbrain/gbrain-reference.md) → Resolved
Incidents.

### FIX: A SCRIPT KILLS THE LIVE GBRAIN CONNECTION MID-SESSION

Symptom: running a GBrain sync wrapper mid-session unexpectedly breaks
search for the rest of that session. Usually a leftover PGLite-era
`pkill -f "gbrain serve"` step with no restart — no longer needed under
Postgres (multi-writer). Remove that step from any script.

Full story: [[[mybizz-config-docs:gbrain/gbrain-explainer|Explainer]]](C:/mybizz/mybizz-config-docs/gbrain/gbrain-explainer.md) §3.

### SCHEDULED TASKS RUNNING VIA WSL FROM WINDOWS TASK SCHEDULER

**Symptom:** a Task Scheduler job that calls `wsl.exe` runs against the
wrong Linux user/home directory, or fails outright with a
`WSL_E_USER_NOT_FOUND` error.

**Cause:** a bare `wsl.exe` call depends on which WSL installation is the
default and on that installation's default user. Naming both makes the
task independent of those settings. This machine has one WSL installation,
`Ubuntu`, and it is the default.

**Fix:** always specify both the installation and the user explicitly in any
Task Scheduler action that invokes WSL:

```
wsl.exe -d Ubuntu -u dev-p bash <full-script-path>
```

Never register a scheduled task with a bare `wsl.exe` call.

### MCP / GBrain Not Responding in OpenCode

Restart OpenCode from WSL:

```bash
pkill -f "opencode web"
opencode web --hostname 0.0.0.0
```

### Refresh OpenCode's cache

Use when OpenCode behaves strangely or a provider package is broken. It clears cache, state and temporary files, and keeps your settings, login and sessions. Stop OpenCode first.

```bash
pkill -f "opencode web"
rm -rf -- "$(opencode debug paths cache)" "$(opencode debug paths state)" "$(opencode debug paths tmp)" && echo "OpenCode cache, state, and temporary files cleared."
opencode web --hostname 0.0.0.0
```

Then start OpenCode again (section 1). Full options: `opencode-reset-management.md`.

### Tool details

- Cupcake on and off: [[[mybizz-config-docs:cupcake/cupcake-reference]]](C:/mybizz/mybizz-config-docs/cupcake/cupcake-reference.md), "Disabling Cupcake".
- i-have-adhd switches, update, verification and uninstall: [[[mybizz-config-docs:i-have-adhd/i-have-adhd-reference]]](C:/mybizz/mybizz-config-docs/i-have-adhd/i-have-adhd-reference.md).
- GBrain's AI providers: [[[mybizz-config-docs:gbrain/gbrain-config]]](C:/mybizz/mybizz-config-docs/gbrain/gbrain-config.md), Models.

---

## 6. Patterns to remember

### PATTERN: BULK RETROFIT / MASS-EDIT TASKS (e.g. frontmatter passes)

An agent's own summary count of a multi-file retrofit is not reliable
evidence the work is correct. Always recount against the itemized
detail, and treat a post-apply diff check against backups as required,
not optional — it's what actually catches a phantom candidate (a
proposed change for text that isn't really there).

### PATTERN: AI AGENT FILE EDITS MAY LAND BEFORE THE PERMISSION PROMPT SHOWS

Symptom: a chat log shows an edit applied *above* a permission popup
asking to view that same file — the write happened before you were
asked to approve it. Never trust the agent's own summary in this case;
verify independently, outside the session:

```bash
git diff <file>
```
