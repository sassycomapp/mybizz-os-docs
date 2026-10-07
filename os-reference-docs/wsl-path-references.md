---
title: "wsl-path-references"
doc-id: "mb-8904e69d"
updated: "2026-10-07"

---

## Skills

> \\wsl.localhost\Ubuntu\home\dev-p\.config\opencode\skills

## AGENTS.md

```
\\\wsl.localhost\Ubuntu\home\dev-p\.config\opencode\AGENTS.md
```

## Cupcake Policies (.rego)

```
\\\wsl.localhost\Ubuntu\home\dev-p\.config\cupcake\policies\opencode
```

## Cupcake Plugin

\\\wsl.localhost\Ubuntu\home\dev-p\.config\opencode\plugin

## i-have-adhd

\\\wsl.localhost\Ubuntu\home\dev-p\.agents\skills\i-have-adhd\SKILL.md  
\\\wsl.localhost\Ubuntu\home\dev-p\.config\opencode\plugin\i-have-adhd.mjs" 
\\\wsl.localhost\Ubuntu\home\dev-p\.agents\skills\i-have-adhd\agents\openai.yaml  
\\\wsl.localhost\Ubuntu\home\dev-p\.agents\skills\i-have-adhd\agents\gemini.toml

## Cupcake - Enable, disable

The procedure is in [[[mybizz-config-docs:cupcake/cupcake-reference|Cupcake reference]]](C:/mybizz/mybizz-config-docs/cupcake/cupcake-reference.md), section "Disabling Cupcake".

## GBrain and the memory system (WSL)

`\\wsl.localhost\Ubuntu\home\dev-p\gbrain` — the live GBrain install (a Git checkout)  
`\\wsl.localhost\Ubuntu\home\dev-p\.gbrain` — GBrain settings (contains keys — private)  
`\\wsl.localhost\Ubuntu\home\dev-p\backups` — pre-upgrade backups and the logs of `commit-all.sh` (private)  
`\\wsl.localhost\Ubuntu\home\dev-p\memory-governor` — the memory governor  
`\\wsl.localhost\Ubuntu\home\dev-p\.config\systemd\user` — the governor's two service unit files

## Scripts and logs (on C:\)

`C:\mybizz\scripts\` (WSL: `/mnt/c/mybizz/scripts/`): `commit-all.sh`, `sync-gbrain.sh`, `mb-doc-check.py`, `mb-doc-check.sources`, `rollout-repo.sh`, `rollout-all.sh`, `cutover-block2.sh`, `cutover-block3.sh`, `cutover-rollback.sh`  
`C:\mybizz\logs\gbrain-logs\` — sync logs  
`C:\mybizz\logs\mb-submit-memory\` — submission logs

Commit, update GBrain and push everything: `bash /mnt/c/mybizz/scripts/commit-all.sh "message"`  
Run only the GBrain update: `bash /mnt/c/mybizz/scripts/sync-gbrain.sh`
