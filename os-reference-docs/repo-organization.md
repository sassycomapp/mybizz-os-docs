---

title: "repo-organization"
doc-id: "mb-f38fd0aa"

---

# Repo organization

Correlation table of GBrain sources, GitHub repositories, and local paths.
Replaces `Repo organization.xlsx` (deleted 2026-09-14 by the developer).

**Cupcake readiness column (added 2026-09-22, set-and-forget amendment):** every
folder is Cupcake-ready. "Initialized" = the folder carries a `.cupcake` skeleton
(the 2026-08-27 scaffolds, each guaranteed boot-safe by the input-gated
`cup-noop-fallback.rego` placeholder); "Fallback" = no skeleton — the plugin
evaluates global policies only via `~/.config/cupcake/fallback`. Initialized
folders with local policies run them; all folders always run the global rule set.

| Gbrain Source list             | Github Repo list                               | Local Repo path (Windows)                    | Local Repo path (WSL)                            | Cupcake readiness                    | Dream cycle eligible |
| ------------------------------ | ---------------------------------------------- | -------------------------------------------- | ------------------------------------------------ | ------------------------------------ | -------------------- |
| default (federated)            | project-library-global (public)                | C:\dev\project-library-global                | /mnt/c/dev/project-library-global                | Ready — fallback (no skeleton)       | Yes                  |
| dev-root-hub (federated)       | dev-root-hub (Public)                          | C:\dev\dev-root                              | /mnt/c/dev/dev-root                              | Ready — fallback (no skeleton)       | No                   |
| makepdlf-project-library       | makepdlf-project-library (Private)             | C:\dev\dev-makepdlf\makepdlf-project-library | /mnt/c/dev/dev-makepdlf/makepdlf-project-library | Ready — initialized (local policies) | Yes                  |
| mb-3-cs-code                   | mb-3-cs (Public)                               | C:\dev\dev-mb-3-cs\mb-3-cs                   | /mnt/c/dev/dev-mb-3-cs/mb-3-cs                   | Ready — initialized (placeholder)    | No                   |
| mb-3-cs-project-library        | mb-3-cs-project-library (Private)              | C:\dev\dev-mb-3-cs\mb-3-cs-project-library   | /mnt/c/dev/dev-mb-3-cs/mb-3-cs-project-library   | Ready — initialized (placeholder)    | Yes                  |
| mb4ecom-code                   | mb4ecom (Public)                               | C:\dev\dev-mb4ecom\mb4ecom                   | /mnt/c/dev/dev-mb4ecom/mb4ecom                   | Ready — initialized (placeholder)    | No                   |
| mb4ecom-project-library        | mb4ecom-project-library (Private)              | C:\dev\dev-mb4ecom\mb4ecom-project-library   | /mnt/c/dev/dev-mb4ecom/mb4ecom-project-library   | Ready — initialized (placeholder)    | No                   |
| mb5pdlf-code                   | mb5pdlf (Public)                               | C:\dev\dev-mb5pdlf\mb5pdlf                   | /mnt/c/dev/dev-mb5pdlf/mb5pdlf                   | Ready — initialized (placeholder)    | No                   |
| mb5pdlf-project-library        | mb5pdlf-project-library (Private)              | C:\dev\dev-mb5pdlf\mb5pdlf-project-library   | /mnt/c/dev/dev-mb5pdlf/mb5pdlf-project-library   | Ready — initialized (placeholder)    | No                   |
| memory-governor-code           | memory-governor (Private)                      | No windows path (Only WSL path by decision)  | /home/dev-p/memory-governor                      | Ready — fallback (no skeleton)       | No                   |
| mybizz-config-docs (federated) | mybizz-config-docs (Private)                   | C:\mybizz\mybizz-config-docs                 | /mnt/c/mybizz/mybizz-config-docs                 | Ready — initialized (placeholder)    | yes                  |
| mybizz-os-docs (federated)     | mybizz-os-docs (Private)                       | C:\mybizz\mybizz-os-docs                     | /mnt/c/mybizz/mybizz-os-docs                     | Ready — fallback (no skeleton)       | Yes                  |
| data-mybizz-mgt (federated)    | data-mybizz-mgt (Private)                      | C:\data-mybizz-mgt                           | /mnt/c/data-mybizz-mgt                           | Ready — fallback (no skeleton)       | No                   |
| pdlf (federated)               | pdlf (Private)                                 | C:\pdlf                                      | /mnt/c/pdlf                                      | Ready — fallback (no skeleton)       | Yes                  |
| mb-doc-maintainer              | mb-doc-maintainer (Private)                    | C:\mybizz\mb-doc-maintainer                  | /mnt/c/mybizz/mb-doc-maintainer                  | Ready — fallback (no skeleton)       | No                   |
| mb-doc-auditor                 | mb-doc-auditor (Private)                       | C:\mybizz\mb-doc-auditor                     | /mnt/c/mybizz/mb-doc-auditor                     | Ready — fallback (no skeleton)       | No                   |
| anvil-agent-references-code    | anvil-agent-references (upstream: anvil-works) | C:\mybizz\anvil-agent-references             | /mnt/c/mybizz/anvil-agent-references             | Ready — fallback (no skeleton)       | No                   |
