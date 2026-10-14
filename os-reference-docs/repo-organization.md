---
title: "repo-organization"
doc-id: "mb-f38fd0aa"
updated: "2026-10-14"

---

# Repo organization

Correlation table of GBrain sources, GitHub repositories, and local paths.

**Cupcake readiness column:** every
folder is Cupcake-ready. "Initialized" = the folder carries a `.cupcake` skeleton
(the 2026-08-27 scaffolds, each guaranteed boot-safe by the input-gated
`cup-noop-fallback.rego` placeholder); "Fallback" = no skeleton — the plugin
evaluates global policies only via `~/.config/cupcake/fallback`. Initialized
folders with local policies run them; all folders always run the global rule set.

| Gbrain Source list                     | Github Repo list                                                                                | Local Repo path (Windows)                    | Local Repo path (WSL)                            | Cupcake readiness                    | Dream cycle eligible |
| -------------------------------------- | ----------------------------------------------------------------------------------------------- | -------------------------------------------- | ------------------------------------------------ | ------------------------------------ | -------------------- |
| anvil-agent-references-code (isolated) | anvil-works/anvil-agent-references (Anvil's public repo — not the developer's; read-only clone) | C:\mybizz\anvil-agent-references             | /mnt/c/mybizz/anvil-agent-references             | Ready — fallback (no skeleton)       | No                   |
| default (federated)                    | project-library-global (Public)                                                                | C:\dev\project-library-global                | /mnt/c/dev/project-library-global                | Ready — fallback (no skeleton)       | Yes                  |
| dev-root-hub (federated)               | dev-root-hub (Private)                                                                          | C:\dev\dev-root                              | /mnt/c/dev/dev-root                              | Ready — fallback (no skeleton)       | No                   |
| — (code: not in GBrain) | mb-3-cs-app1 (Public) | C:\dev\dev-mb-3-cs\mb-3-cs-app1 | /mnt/c/dev/dev-mb-3-cs/mb-3-cs-app1 | Ready — initialized (placeholder) | No |
| — (code: not in GBrain) | mb-3-cs-app2 (Public) | C:\dev\dev-mb-3-cs\mb-3-cs-app2 | /mnt/c/dev/dev-mb-3-cs/mb-3-cs-app2 | Ready — fallback (no skeleton) | No |
| — (code: not in GBrain) | mb-3-cs-app3 (Public) | C:\dev\dev-mb-3-cs\mb-3-cs-app3 | /mnt/c/dev/dev-mb-3-cs/mb-3-cs-app3 | Ready — fallback (no skeleton) | No |
| — (code: not in GBrain) | mb-3-cs-app5 (Public) | C:\dev\dev-mb-3-cs\mb-3-cs-app5 | /mnt/c/dev/dev-mb-3-cs/mb-3-cs-app5 | Ready — fallback (no skeleton) | No |
| mb-3-cs-project-library (federated)    | mb-3-cs-project-library (Private)                                                               | C:\dev\dev-mb-3-cs\mb-3-cs-project-library   | /mnt/c/dev/dev-mb-3-cs/mb-3-cs-project-library   | Ready — initialized (placeholder)    | Yes                  |
| mb-archify-scope (federated)           | mb-archify-scope (Private)                                                                      | C:\mybizz\mb-archify-scope                  | /mnt/c/mybizz/mb-archify-scope                   | Ready — fallback (no skeleton)       | No                   |
| mb-close-out (federated)               | mb-close-out (Public)                                                                          | C:\mybizz\mb-close-out                  | /mnt/c/mybizz/mb-close-out                   | Ready — fallback (no skeleton)       | No                   |
| mb-compile-rules (federated)           | mb-compile-rules (Public)                                                                      | C:\mybizz\mb-compile-rules                  | /mnt/c/mybizz/mb-compile-rules                   | Ready — fallback (no skeleton)       | No                   |
| mb-memory-system-diagnostic (federated) | mb-memory-system-diagnostic (Private)                                                          | C:\mybizz\mb-memory-system-diagnostic       | /mnt/c/mybizz/mb-memory-system-diagnostic        | Ready — fallback (no skeleton)       | No                   |
| mb-project-docs (federated)            | mb-project-docs (Public)                                                                       | C:\mybizz\mb-project-docs                   | /mnt/c/mybizz/mb-project-docs                    | Ready — fallback (no skeleton)       | No                   |
| mb-sentinel-review (federated)         | mb-sentinel-review (Private)                                                                    | C:\mybizz\mb-sentinel-review                | /mnt/c/mybizz/mb-sentinel-review                 | Ready — fallback (no skeleton)       | No                   |
| mb-submit-memory (federated)           | mb-submit-memory (Private)                                                                      | C:\mybizz\mb-submit-memory                  | /mnt/c/mybizz/mb-submit-memory                   | Ready — fallback (no skeleton)       | No                   |
| mb-sysdoc-creator (federated)          | mb-sysdoc-creator (Public)                                                                     | C:\mybizz\mb-sysdoc-creator                 | /mnt/c/mybizz/mb-sysdoc-creator                  | Ready — fallback (no skeleton)       | No                   |
| — (code: not in GBrain) | mb-4-ecom-app1 (Public) | C:\dev\dev-mb-4-ecom\mb-4-ecom-app1 | /mnt/c/dev/dev-mb-4-ecom/mb-4-ecom-app1 | Ready — initialized (placeholder) | No |
| — (code: not in GBrain) | mb-4-ecom-app2 (Public) | C:\dev\dev-mb-4-ecom\mb-4-ecom-app2 | /mnt/c/dev/dev-mb-4-ecom/mb-4-ecom-app2 | Ready — fallback (no skeleton) | No |
| — (code: not in GBrain) | mb-4-ecom-app3 (Public) | C:\dev\dev-mb-4-ecom\mb-4-ecom-app3 | /mnt/c/dev/dev-mb-4-ecom/mb-4-ecom-app3 | Ready — fallback (no skeleton) | No |
| — (code: not in GBrain) | mb-4-ecom-app5 (Public) | C:\dev\dev-mb-4-ecom\mb-4-ecom-app5 | /mnt/c/dev/dev-mb-4-ecom/mb-4-ecom-app5 | Ready — fallback (no skeleton) | No |
| mb-4-ecom-project-library (federated) | mb-4-ecom-project-library (Private) | C:\dev\dev-mb-4-ecom\mb-4-ecom-project-library | /mnt/c/dev/dev-mb-4-ecom/mb-4-ecom-project-library | Ready — initialized (placeholder) | No |
| — (code: not in GBrain) | mb-5-pdlf-app1 (Public) | C:\dev\dev-mb-5-pdlf\mb-5-pdlf-app1 | /mnt/c/dev/dev-mb-5-pdlf/mb-5-pdlf-app1 | Ready — initialized (placeholder) | No |
| — (code: not in GBrain) | mb-5-pdlf-app2 (Public) | C:\dev\dev-mb-5-pdlf\mb-5-pdlf-app2 | /mnt/c/dev/dev-mb-5-pdlf/mb-5-pdlf-app2 | Ready — fallback (no skeleton) | No |
| — (code: not in GBrain) | mb-5-pdlf-app3 (Public) | C:\dev\dev-mb-5-pdlf\mb-5-pdlf-app3 | /mnt/c/dev/dev-mb-5-pdlf/mb-5-pdlf-app3 | Ready — fallback (no skeleton) | No |
| — (code: not in GBrain) | mb-5-pdlf-app5 (Public) | C:\dev\dev-mb-5-pdlf\mb-5-pdlf-app5 | /mnt/c/dev/dev-mb-5-pdlf/mb-5-pdlf-app5 | Ready — fallback (no skeleton) | No |
| mb-5-pdlf-project-library (federated) | mb-5-pdlf-project-library (Private) | C:\dev\dev-mb-5-pdlf\mb-5-pdlf-project-library | /mnt/c/dev/dev-mb-5-pdlf/mb-5-pdlf-project-library | Ready — initialized (placeholder) | No |
| memory-governor-code (unset)           | memory-governor (Private)                                                                       | No windows path (Only WSL path by decision)  | /home/dev-p/memory-governor                      | Ready — fallback (no skeleton)       | No                   |
| mybizz-config-docs (federated)         | mybizz-config-docs (Public)                                                                    | C:\mybizz\mybizz-config-docs                 | /mnt/c/mybizz/mybizz-config-docs                 | Ready — initialized (placeholder)    | yes                  |
| mybizz-os-docs (federated)             | mybizz-os-docs (Public)                                                                        | C:\mybizz\mybizz-os-docs                     | /mnt/c/mybizz/mybizz-os-docs                     | Ready — fallback (no skeleton)       | Yes                  |
| mybizz-scripts-code (unset)            | mybizz-scripts (Public)                                                                        | C:\mybizz\scripts                            | /mnt/c/mybizz/scripts                            | Ready — fallback (no skeleton)       | No                   |
| data-mybizz-mgt (federated)            | data-mybizz-mgt (Private)                                                                       | C:\data-mybizz-mgt                           | /mnt/c/data-mybizz-mgt                           | Ready — fallback (no skeleton)       | No                   |
| pdlf (federated)                       | pdlf (Public)                                                                                  | C:\pdlf                                      | /mnt/c/pdlf                                      | Ready — fallback (no skeleton)       | Yes                  |

## Notes

**Verified against `gbrain sources list`.**

**Federation states** (the mark after the source id): `federated` — appears in every other source's default search and may write links into other sources; `unset` — never set: the source stays out of other sources' searches, but a session working inside it can still read the federated sources; `isolated` — set explicitly with `gbrain sources unfederate`: cut off in both directions. Every documentation source is federated. The two system code sources, `memory-governor-code` and `mybizz-scripts-code`, are deliberately left `unset`.

**Names:** every business has one project folder, `C:\dev\dev-{slug}\`, holding four code repositories and one project library: `{slug}-app1`, `{slug}-app2`, `{slug}-app3`, `{slug}-app5` and `{slug}-project-library`. The slugs are `mb-3-cs`, `mb-4-ecom` and `mb-5-pdlf`, all lowercase. App 4 (a client instance) lives on the client's own Anvil account and has no folder here. Each code repository is an Anvil app of the same name. `-code` appears only in some GBrain source ids.

**Application code is not in GBrain.** The app code repositories are not GBrain sources. Agents read application code directly from the app folder, which is always current; an index of it would go stale between syncs.

**Note on `anvil-agent-references-code`:** this source is a read-only clone of Anvil's own public repository, `github.com/anvil-works/anvil-agent-references`, at `C:\mybizz\anvil-agent-references`. Anvil publishes it. It is not one of the developer's repositories, and it has no GitHub repository of the developer's own. It is separate from `anvil-docs` in the source `default` (`project-library-global`), which is the developer's own compilation of Anvil documentation. The developer's stated reason for the `-code` in its source id is to distinguish it from the developer's own documentation folder `anvil-agent-references/` in `mybizz-config-docs`. The source is isolated, so it is searched only when named explicitly. It is updated by hand (`git -C /mnt/c/mybizz/anvil-agent-references pull`); the standing sync script does not pull.

**Commit hook and `commit-all.sh`:** they apply to every documentation repository: `default`, `dev-root-hub`, `mb-3-cs-project-library`, `mb-4-ecom-project-library`, `mb-5-pdlf-project-library`, `mb-archify-scope`, `mb-close-out`, `mb-compile-rules`, `mb-memory-system-diagnostic`, `mb-project-docs`, `mb-sentinel-review`, `mb-submit-memory`, `mb-sysdoc-creator`, `mybizz-config-docs`, `mybizz-os-docs`, `data-mybizz-mgt` and `pdlf`. They also cover the scripts repository, `mybizz-scripts`. They do not cover the app code repositories, the Anvil clone or the memory governor's folder.
