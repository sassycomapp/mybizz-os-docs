---
title: "wsl-path-references"
doc-id: "mb-8904e69d"

---

## Skills

\\wsl.localhost\Ubuntu\home\dev-p\.config\opencode\skills

## AGENTS.md

\\\wsl.localhost\Ubuntu\home\dev-p\.config\opencode\AGENTS.md

## Cupcake Policies (.rego)

\\\wsl.localhost\Ubuntu\home\dev-p\.config\cupcake\policies\opencode

## Cupcake Plugin

\\\wsl.localhost\Ubuntu\home\dev-p\.config\opencode\plugin

## i-have-adhd

\\\wsl.localhost\Ubuntu\home\dev-p\.agents\skills\i-have-adhd\SKILL.md  
\\\wsl.localhost\Ubuntu\home\dev-p\.config\opencode\plugin\i-have-adhd.mjs" 
\\\wsl.localhost\Ubuntu\home\dev-p\.agents\skills\i-have-adhd\agents\openai.yaml  
\\\wsl.localhost\Ubuntu\home\dev-p\.agents\skills\i-have-adhd\agents\gemini.toml

## Cupcake - Enable, disable

documented in [[cupcake-reference|Usage, Invocation, and Worked Examples]] (mybizz-config-docs), line 275 (Disabling Cupcake — immediate, fully reversible):

```
mv ~/.config/opencode/plugin/cupcake.js ~/.config/opencode/plugin/cupcake.js.disabled
```

Restart OpenCode afterwards (plugin changes only load at process start). To re-enable:

```
mv ~/.config/opencode/plugin/cupcake.js.disabled ~/.config/opencode/plugin/cupcake.js
```

This matches the prior suspension described in [[cupcake-explainer|Mechanical Policy Enforcement for AI Coding Agents]] (mybizz-config-docs), line 260 — renaming the plugin out of its discoverable path, nothing else touched, and the `.bak-*` copies in the plugin folder confirm earlier uses.

## GBrain and the memory system (WSL)

`\\wsl.localhost\Ubuntu\home\dev-p\gbrain` — the live GBrain install (a Git checkout)  
`\\wsl.localhost\Ubuntu\home\dev-p\.gbrain` — GBrain settings (contains keys — private)  
`\\wsl.localhost\Ubuntu\home\dev-p\backups` — pre-upgrade backups (private)  
`\\wsl.localhost\Ubuntu\home\dev-p\memory-governor` — the memory governor  
`\\wsl.localhost\Ubuntu\home\dev-p\.config\systemd\user` — the governor's two service unit files

## Scripts and logs (on C:\)

`C:\mybizz\scripts\` (WSL: `/mnt/c/mybizz/scripts/`): `sync-gbrain.sh`, `mb-doc-check.py`, `mb-doc-check.roots`, `cutover-block2.sh`, `cutover-block3.sh`, `cutover-rollback.sh`  
`C:\mybizz\logs\gbrain-logs\` — sync logs  
`C:\mybizz\logs\mb-submit-memory\` — submission logs

Run the sync: `bash /mnt/c/mybizz/scripts/sync-gbrain.sh`
