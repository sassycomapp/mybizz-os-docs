---
title: "repo-organization"
doc-id: "mb-f38fd0aa"
updated: "2026-10-03"

---

# Repo organization

Correlation table of GBrain sources, GitHub repositories, and local paths.
Replaces `Repo organization.xlsx` (deleted 2026-09-14 by the developer).
Updated 2026-09-30: 15 sources, federation states verified against `gbrain sources list`.

**Cupcake readiness column (added 2026-09-22, set-and-forget amendment):** every
folder is Cupcake-ready. "Initialized" = the folder carries a `.cupcake` skeleton
(the 2026-08-27 scaffolds, each guaranteed boot-safe by the input-gated
`cup-noop-fallback.rego` placeholder); "Fallback" = no skeleton — the plugin
evaluates global policies only via `~/.config/cupcake/fallback`. Initialized
folders with local policies run them; all folders always run the global rule set.

| Gbrain Source list                     | Github Repo list                                                                                | Local Repo path (Windows)                    | Local Repo path (WSL)                            | Cupcake readiness                    | Dream cycle eligible |
| -------------------------------------- | ----------------------------------------------------------------------------------------------- | -------------------------------------------- | ------------------------------------------------ | ------------------------------------ | -------------------- |
| anvil-agent-references-code (isolated) | anvil-works/anvil-agent-references (Anvil's public repo — not the developer's; read-only clone) | C:\mybizz\anvil-agent-references             | /mnt/c/mybizz/anvil-agent-references             | **open**                             | **open**             |
| default (federated)                    | project-library-global (private)                                                                | C:\dev\project-library-global                | /mnt/c/dev/project-library-global                | Ready — fallback (no skeleton)       | Yes                  |
| dev-root-hub (federated)               | dev-root-hub (Private)                                                                          | C:\dev\dev-root                              | /mnt/c/dev/dev-root                              | Ready — fallback (no skeleton)       | No                   |
| makepdlf-project-library (federated)   | makepdlf-project-library (Private)                                                              | C:\dev\dev-makepdlf\makepdlf-project-library | /mnt/c/dev/dev-makepdlf/makepdlf-project-library | Ready — initialized (local policies) | Yes                  |
| mb-3-cs-code (unset)                   | mb-3-cs (Public)                                                                                | C:\dev\dev-mb-3-cs\mb-3-cs                   | /mnt/c/dev/dev-mb-3-cs/mb-3-cs                   | Ready — initialized (placeholder)    | No                   |
| mb-3-cs-project-library (federated)    | mb-3-cs-project-library (Private)                                                               | C:\dev\dev-mb-3-cs\mb-3-cs-project-library   | /mnt/c/dev/dev-mb-3-cs/mb-3-cs-project-library   | Ready — initialized (placeholder)    | Yes                  |
| mb4ecom-code (unset)                   | mb4ecom (Public)                                                                                | C:\dev\dev-mb4ecom\mb4ecom                   | /mnt/c/dev/dev-mb4ecom/mb4ecom                   | Ready — initialized (placeholder)    | No                   |
| mb4ecom-project-library (federated)    | mb4ecom-project-library (Private)                                                               | C:\dev\dev-mb4ecom\mb4ecom-project-library   | /mnt/c/dev/dev-mb4ecom/mb4ecom-project-library   | Ready — initialized (placeholder)    | No                   |
| mb5pdlf-code (unset)                   | mb5pdlf (Public)                                                                                | C:\dev\dev-mb5pdlf\mb5pdlf                   | /mnt/c/dev/dev-mb5pdlf/mb5pdlf                   | Ready — initialized (placeholder)    | No                   |
| mb5pdlf-project-library (federated)    | mb5pdlf-project-library (Private)                                                               | C:\dev\dev-mb5pdlf\mb5pdlf-project-library   | /mnt/c/dev/dev-mb5pdlf/mb5pdlf-project-library   | Ready — initialized (placeholder)    | No                   |
| memory-governor-code (unset)           | memory-governor (Private)                                                                       | No windows path (Only WSL path by decision)  | /home/dev-p/memory-governor                      | Ready — fallback (no skeleton)       | No                   |
| mybizz-config-docs (federated)         | mybizz-config-docs (Private)                                                                    | C:\mybizz\mybizz-config-docs                 | /mnt/c/mybizz/mybizz-config-docs                 | Ready — initialized (placeholder)    | yes                  |
| mybizz-os-docs (federated)             | mybizz-os-docs (Private)                                                                        | C:\mybizz\mybizz-os-docs                     | /mnt/c/mybizz/mybizz-os-docs                     | Ready — fallback (no skeleton)       | Yes                  |
| data-mybizz-mgt (federated)            | data-mybizz-mgt (Private)                                                                       | C:\data-mybizz-mgt                           | /mnt/c/data-mybizz-mgt                           | Ready — fallback (no skeleton)       | No                   |
| pdlf (federated)                       | pdlf (Private)                                                                                  | C:\pdlf                                      | /mnt/c/pdlf                                      | Ready — fallback (no skeleton)       | Yes                  |

## Notes

**Verified 2026-09-30** against `gbrain sources list` (15 sources).

**Federation states** (the mark after the source id): `federated` — appears in every other source's default search and may write links into other sources; `unset` — never set: the source stays out of other sources' searches, but a session working inside it can still read the federated sources; `isolated` — set explicitly with `gbrain sources unfederate`: cut off in both directions. Every documentation source is federated (developer, 2026-09-30). The four code sources are deliberately left `unset`.

**Names:** the Anvil project names are `mb-3-cs`, `mb4ecom` and `mb5pdlf`; they do not contain `-code`. `-code` appears only in some GBrain source ids.

**Note on `anvil-agent-references-code`:** this source is a read-only clone of Anvil's own public repository, `github.com/anvil-works/anvil-agent-references`, at `C:\mybizz\anvil-agent-references`. Anvil publishes it. It is not one of the developer's repositories, and it has no GitHub repository of the developer's own. It is separate from `anvil-docs` in the source `default` (`project-library-global`), which is the developer's own compilation of Anvil documentation. The developer's stated reason for the `-code` in its source id is to distinguish it from the developer's own documentation folder `anvil-agent-references/` in `mybizz-config-docs`. The source is isolated, so it is searched only when named explicitly. It is updated by hand (`git -C /mnt/c/mybizz/anvil-agent-references pull`); the standing sync script does not pull.

**Commit hook and `commit-all.sh`:** they apply to the eight documentation repositories — `default`, `dev-root-hub`, `makepdlf-project-library`, `mb-3-cs-project-library`, `mybizz-config-docs`, `mybizz-os-docs`, `data-mybizz-mgt` and `pdlf`. They do not cover the two dormant libraries (`mb4ecom-project-library`, `mb5pdlf-project-library`), the code repositories, the Anvil clone or the memory governor's folder.

**Open cells:** the Cupcake readiness and dream cycle eligibility of `anvil-agent-references-code` are left open by the developer's decision (2026-09-30), to be completed later.
