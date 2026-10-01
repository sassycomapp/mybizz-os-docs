---
title: "opencode-reset-management"
doc-id: "mb-d38a6f54"
updated: "2026-10-01"
---
# OpenCode Reset Management (WSL)

For **OpenCode installed and run inside WSL**, the normal cache is `~/.cache/opencode`. OpenCode also has separate persistent data, state, temporary files, logs, and configuration paths, so a “full cache reset” should clear cache/state/temp but **not** delete your authentication, configuration, or saved sessions unless you explicitly choose to. OpenCode’s own path command lets you resolve the exact directories used by your installed version.

First, fully exit any active OpenCode TUI, web client, Desktop connection, or background process.

## STOP / RESTART OPENCODE

```bash
pkill -f "opencode web"
opencode web --hostname 0.0.0.0 
# *****CHECK THE MODEL*****
```

## Safe cache reset

Paste this into the WSL terminal:

```bash
rm -rf -- "$(opencode debug paths cache)" "$(opencode debug paths state)" "$(opencode debug paths tmp)" && echo "OpenCode cache, state, and temporary files cleared."
```

This clears:

- `cache` — including dynamically installed provider packages; this is the official fix for stale or broken provider packages.
- `state` — transient application state.
- `tmp` — temporary files.

It preserves your OpenCode config, authentication, project/session data, and logs. The CLI documentation confirms that `opencode debug paths` supports the `cache`, `state`, and `tmp` selectors, allowing this command to use the correct paths rather than assuming locations.

## Minimal official command

If your issue is likely provider-package related, or you only want the officially documented cache cleanup:

```bash
rm -rf ~/.cache/opencode
```

Restart OpenCode afterwards; it will recreate the directory and redownload required provider packages as needed.

## Inspect locations first

To see every location OpenCode currently uses in your WSL environment before deleting anything:

```bash
opencode debug paths
```

The principal paths worth distinguishing are:

| Path type | Purpose                                                                   | Clear it?                                           |
| --------- | ------------------------------------------------------------------------- | --------------------------------------------------- |
| `cache`   | Cached provider packages and disposable caches                            | Yes, safe                                           |
| `state`   | Runtime/transient state                                                   | Usually safe                                        |
| `tmp`     | Temporary files                                                           | Safe                                                |
| `data`    | Sessions, project data, and often authentication-related application data | Only for a full reset                               |
| `config`  | `opencode.json(c)`, plugins, settings                                     | Do not clear for cache troubleshooting              |
| `log`     | Diagnostic logs                                                           | Optional; retain until you have diagnosed the issue |

## Full destructive reset

Use this only if OpenCode still fails after the safe reset and you deliberately want to remove its local data, saved sessions, stored authentication, cache, config, and state:

```bash
opencode uninstall
```

OpenCode will show what it intends to remove and ask for confirmation. Its official CLI documentation states that uninstall removes global data, cache, configuration, and state, after stopping registered background services and persistent terminals.

If you want a preview first:

```bash
opencode uninstall --dry-run
```

For most cases, start with the **safe cache reset** command, then run `opencode` again.
