---
title: "commit-all-repositories"
doc-id: "mb-313772ce"
updated: "2026-10-04"
---
# Commit, update GBrain and push all repositories

The one procedure to follow every time you finish work on any of the fifteen documentation repositories. It keeps every repository committed, GBrain current and GitHub up to date at the same time. It is the "after doing work" step of [[[mybizz-os-docs:operating-procedures/daily-ops]]](C:/mybizz/mybizz-os-docs/operating-procedures/daily-ops.md).

The fifteen repositories: `mybizz-os-docs`, `mybizz-config-docs`, `data-mybizz-mgt`, `pdlf`, `dev-root`, `makepdlf-project-library`, `mb-3-cs-project-library`, `project-library-global`, `mb-archify-scope`, `mb-compile-rules`, `mb-memory-system-diagnostic`, `mb-project-docs`, `mb-sentinel-review`, `mb-submit-memory`, `mb-sysdoc-creator`. The full list with paths and GBrain sources is in [[[mybizz-os-docs:os-reference-docs/repo-organization]]](C:/mybizz/mybizz-os-docs/os-reference-docs/repo-organization.md).

## Every time

1. Open a WSL terminal. Always commit from WSL, never from Windows.
2. Optional: see what would be committed. This changes nothing.

```
bash /mnt/c/mybizz/scripts/commit-all.sh --status
```

3. Run the whole procedure with one message of your own:

```
bash /mnt/c/mybizz/scripts/commit-all.sh "what you did"
```

4. Wait. It can take several minutes, because GBrain re-reads the changed documents. Do not press Ctrl+C.
5. Read the summary at the end. Every repository should say `committed` or `nothing to commit`, and `pushed` or `already up to date`. The last line says `All done.`

The command does three things in this order: it commits every repository that has changes, with your message; it updates GBrain with `sync-gbrain.sh`; it pushes every repository to GitHub. The full output is saved to a log file in `~/backups/`.

## If a repository says BLOCKED

The commit hook (`mb-doc-check`) stopped that repository and printed the reason. Nothing is lost. Fix the cause, then run the same command again. Repositories that are already done say `nothing to commit`.

| The message says | What to do |
|---|---|
| a link `points to no document` | The link names a document that does not exist. Correct the name, or write the words without the link. |
| a link `is a name shared by several documents` | Write the one you mean in the form the message suggests, for example `[[source-id:name]]`. |
| `NOTHING TO COMMIT` | Only the date line differed, so the hook put the file back. No action needed. |
| damaged frontmatter | Fix the lines between the two `---` lines at the top. A value with a colon inside needs quotes. |

Never use `git commit --no-verify`. It switches the hook off and leaves the document in the wrong form.

## If a push says FAILED

GitHub has commits that your computer does not. Do not force the push. Ask Claude for help and paste the summary.

## Rules the hook enforces, so nobody has to remember them

- **Links.** Anyone, human or AI, writes a link as `[[name]]` or `[[name|shown text]]`. The hook turns it into the standard form `[[[source-id:name]]](C:/full/path.md)`, which GBrain follows and MarkText opens. Never write the long form by hand.
- **Paths.** A document path pasted into running text becomes a link. A path written in `backticks` stays plain text. Put a path in backticks when it only states where something is.
- **Frontmatter.** Every document has `title`, `doc-id` and `updated`. The hook adds what is missing. `doc-id` never changes. `updated` changes only when the document really changed.
- **Line endings.** Documents are stored with LF. MarkText is set to LF.
- **Never touched.** `SKILL.md` files and `obsolete` folders.

## Checks you can run

```
python3 /mnt/c/mybizz/scripts/mb-doc-check.py check --root /mnt/c/mybizz/mybizz-os-docs
python3 /mnt/c/mybizz/scripts/mb-doc-check.py sources-check
gbrain doctor
```

`check` lists links that point to nothing in one repository. `sources-check` must say `everything matches`. `gbrain doctor` should show 30/100 with one failure, `cycle_freshness`. That is the accepted baseline.

## Adding a new documentation repository

Do this once for each new repository, in this order:

1. Create the GitHub repository and clone it under `C:\`.
2. Add it to GBrain and make it federated: `gbrain sources add <id> --path <folder>` and `gbrain sources federate <id>`.
3. Add one line `<id> = <folder>` to `mb-doc-check.sources`, and the folder to `mb-doc-check.roots`, both in `C:\mybizz\scripts\`.
4. Install the hook: `python3 /mnt/c/mybizz/scripts/mb-doc-check.py install-hook --repo <folder>`.
5. Run `sources-check`. It must say `everything matches`.
6. Bring the existing documents under the rules: `bash /mnt/c/mybizz/scripts/rollout-repo.sh <folder>`.
7. Add the folder to the list in `commit-all.sh`, and a row to [[[mybizz-os-docs:os-reference-docs/repo-organization]]](C:/mybizz/mybizz-os-docs/os-reference-docs/repo-organization.md).

The repositories `mb4ecom-project-library` and `mb5pdlf-project-library` are not covered yet. Code repositories are outside these operations; this procedure is about documentation only.

## After a GBrain upgrade

Follow [[[mybizz-config-docs:gbrain/gbrain-upgrade-checklist]]](C:/mybizz/mybizz-config-docs/gbrain/gbrain-upgrade-checklist.md). Never run `gbrain upgrade` or `gbrain self-upgrade` on this installation.

## Where the tools are

All in `C:\mybizz\scripts\` (WSL: `/mnt/c/mybizz/scripts/`):

| File | Purpose |
|---|---|
| `commit-all.sh` | This procedure. |
| `sync-gbrain.sh` | Updates GBrain from all sources. |
| `mb-doc-check.py`, `.sources`, `.roots` | The commit hook and its settings. |
| `rollout-repo.sh` | Brings one repository under the rules. |
