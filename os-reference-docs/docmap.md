---
title: "docmap"
updated: "2026-11-14"
doc-id: "mb-8a86af5b"
---
# Mybizz Division — Docmap

**Purpose:** the navigation map of the Mybizz division: where every folder lives, what it is for, and where new files go. Agents read this to understand the territory.

**Verified:** 2026-10-29

**Kept current:** `build-docmap.py` rebuilds the folder trees from the real folders (`python3 /mnt/c/mybizz/scripts/build-docmap.py --write`); see [[[mybizz-os-docs:operating-procedures/daily-ops|Daily Ops]]](C:/mybizz/mybizz-os-docs/operating-procedures/daily-ops.md). Repositories, their GitHub names and GBrain sources are listed in [[[mybizz-os-docs:os-reference-docs/repo-organization|Repo Organization]]](C:/mybizz/mybizz-os-docs/os-reference-docs/repo-organization.md); this map does not repeat them.

---

## 1. Division Root — `C:\mybizz\`

The MyBizz operating system: its documentation, its own skills, its scripts and the third-party clones it uses.

```
C:\mybizz\
├── anvil-agent-references/       ← Anvil skills source clone (read-only)
├── archify/                      ← archify source clone (read-only)
├── archive/                      ← Long-term reference store (never referenced)
├── gstack/                       ← GStack source clone (read-only)
├── logs/                         ← System and tool logs
│   ├── cupcake/                  ← Cupcake logs
│   ├── gbrain-logs/              ← GBrain sync logs
│   ├── github-logs/              ← Commit and push reports, git-status snapshots
│   ├── mb-submit-memory/         ← mb-submit-memory logs
│   ├── morning-tasks/            ← MorningTasks log and state
│   └── weekly-backup/            ← Weekly backup logs
├── matt-pocock-skills-source/    ← Matt Pocock skills source clone (read-only)
├── mb-archify-scope/             ← Skill source: mb-archify-scope
├── mb-close-out/                 ← Skill source: mb-close-out
├── mb-compile-rules/             ← Skill source: mb-compile-rules
├── mb-gbrain-searches/           ← Record of GBrain searches and their results
├── mb-memory-system-diagnostic/  ← Skill source: mb-memory-system-diagnostic
├── mb-project-docs/              ← Skill source: mb-project-docs
├── mb-sentinel-review/           ← Skill source: mb-sentinel-review
├── mb-submit-memory/             ← Skill source: mb-submit-memory
├── mb-sysdoc-creator/            ← Skill source: mb-sysdoc-creator
├── minimax-skills/               ← MiniMax skills source clone (read-only)
├── mybizz-config-docs/           ← One document suite per component
├── mybizz-os-docs/               ← Operating procedures and system-wide reference
├── scripts/                      ← MyBizz scripts (mybizz-scripts repository)
├── README.md                     ← Division workspace overview
└── memory-audit-reports/         ← Memory audit reports
```

---

## 2. Division Management — `C:\data-mybizz-mgt\`

The desktop-level operating documents of the division, and the developer's model and tool working files. Not an Anvil project and not a code repository. Kept apart from `C:\pdlf\`.

```
C:\data-mybizz-mgt\
├── archive/                ← Archived material (never referenced)
├── assets/                 ← Images and logos
├── desktop/                ← Operating documents: developers-duties, master-task-list
├── Models/                 ← Model selection and model inventory
├── temp/                   ← Temporary files
├── Tools and apps/         ← Notes on tools and apps
├── WIP/                    ← Working files
├── README.md               ← What this folder is
└── creative/               ← Creative work: images, video and design material
```

---

## 3. Development Hub — `C:\dev\`

Where applications are developed. Each business has one project folder, `dev-{slug}`, holding four code repositories and one project library. Folders without the `dev-` prefix support every project.

```
C:\dev\
├── dev-mb-3-cs\                               ← Business mb-3-cs
│   ├── mb-3-cs-app1/                          ← App 1 code repository
│   ├── mb-3-cs-app2/                          ← App 2 code repository (a release of App 1)
│   ├── mb-3-cs-app3/                          ← App 3 code repository
│   ├── mb-3-cs-app5/                          ← App 5 code repository
│   ├── mb-3-cs-project-library/               ← Project library
│   ├── wip/                                   ← Project working files
│   ├── README.md                              ← What this project folder is
│   └── mb-3-cs-project-context/               ← Context of mb-3-cs, read-only
├── dev-mb-4-ecom\                             ← Business mb-4-ecom
│   ├── mb-4-ecom-app1/                        ← App 1 code repository
│   ├── mb-4-ecom-app2/                        ← App 2 code repository (a release of App 1)
│   ├── mb-4-ecom-app3/                        ← App 3 code repository
│   ├── mb-4-ecom-app5/                        ← App 5 code repository
│   ├── mb-4-ecom-project-library/             ← Project library
│   ├── mb4ecom-config.yaml                    ← Project configuration file
│   └── README.md                              ← What this project folder is
├── project-library-global\                    ← Universal context library for every Anvil app
│   ├── adr-global/                            ← Architectural decision records
│   ├── anvil-docs/                            ← Anvil documentation compiled for agents
│   ├── docs-standard-global/                  ← Document standards
│   ├── guides-global/                         ← How-to guides
│   ├── policy-global/                         ← Policies
│   ├── rules-cupcake-global/                  ← Source rules for Cupcake
│   ├── security-global/                       ← Security documents
│   ├── specifications-global/                 ← Specifications
│   ├── standard-operating-procedures-global/  ← Standard operating procedures
│   ├── templates-global/                      ← Templates
│   ├── test-suite/                            ← Test method, catalogue and conformance checks
│   └── README.md                              ← What this library is
└── project-template\                          ← Skeleton copied into a new project at Step 11
```

---

## 4. PDLF — `C:\pdlf\`

The app factory: the Project Development Lifecycle Framework that takes an application from idea to release. A separate repository from `C:\data-mybizz-mgt\`; the two are never merged.

```
C:\pdlf\
├── app-factory/            ← One set of step documents per pipeline step
├── pdlf-library/           ← PDLF's own ADRs, specifications, policies, templates and explainers
├── AGENTS.md               ← Standing rules for this location
└── README.md               ← What PDLF is
```

---

## 5. Other Roots

| Folder | What it is |
|---|---|
| `C:\backup-daily\` | MorningTasks daily copies of the development folders, three most recent kept |
| `C:\backup-dream-cycle\` | GBrain exports made by the weekly backup |
| `C:\backup-gbrain\` | PostgreSQL backups made by the weekly backup |
| `C:\backup-general\` | General backups |
| `C:\backup-pre-pdlf-sanity-sweep\` | Full copies of the repositories changed by the PDLF sanity sweep |
| `C:\backup-wsl\` | Backups made by WSL scripts, one folder per script |
| `C:\temp\` | Temporary files outside PDLF |

Backup folders are snapshots: never tracked, never scanned, never referenced as knowledge.

Outside the division and never mapped: `C:\_data-not mybizz\`, `C:\_data-mybizz-exempt\` and `C:\projects-reference\`, which hold personal content.

---

## 6. File Placement Rules

| File type | Destination |
|---|---|
| Application code | `C:\dev\dev-{slug}\{slug}-app1\` (and `-app2`, `-app3`, `-app5`) |
| Project documents | `C:\dev\dev-{slug}\{slug}-project-library\` |
| Project config, reference and explainer | The project library, written by `/mb-project-docs` |
| Project working files | `C:\dev\dev-{slug}\wip\` |
| Universal standards (ADRs, policies, specifications, SOPs, templates) | `C:\dev\project-library-global\{category}\` |
| PDLF documents | `C:\pdlf\` |
| PDLF working files | `C:\pdlf\pdlf-library\wip\` |
| Component documents | `C:\mybizz\mybizz-config-docs\{component}\` |
| Operating procedures and system-wide reference | `C:\mybizz\mybizz-os-docs\` |
| Developer's operating documents | `C:\data-mybizz-mgt\desktop\` |
| Long-term reference store | `C:\mybizz\archive\` |
| Development trash | `C:\dev\obsolete\` |
| Temporary files outside PDLF | `C:\temp\` |
| Installed skills (OpenCode) | `~/.config/opencode/skills/` (WSL) |
| Backups made by WSL scripts | `C:\backup-wsl\{script}\<timestamp>\` |
| System and tool logs | `C:\mybizz\logs\{tool}\` |

### archive and obsolete

| | archive | obsolete |
|---|---|---|
| **Purpose** | Long-term reference store | Trash during a session |
| **Permanence** | Permanent | Purged by the developer when the work is stable |
| **Who decides** | Developer | Developer |
| **Location** | `C:\mybizz\archive\`, `C:\data-mybizz-mgt\archive\` | `C:\dev\obsolete\` |

---

## 7. Tool Locations

Repositories and third-party clones are in [[[mybizz-os-docs:os-reference-docs/repo-organization|Repo Organization]]](C:/mybizz/mybizz-os-docs/os-reference-docs/repo-organization.md). The locations below are not repositories of the developer's own.

| Tool | Location | Config documents |
|---|---|---|
| GStack runtime (OpenCode) | `~/.config/opencode/skills/gstack/` (WSL); `bin` links back to `C:\mybizz\gstack\` | `C:\mybizz\mybizz-config-docs\gstack\` |
| GBrain (live install) | `/home/dev-p/gbrain/` (WSL) | `C:\mybizz\mybizz-config-docs\gbrain\` |
| Matt Pocock skills (live) | `~/.agents/skills/` (WSL), linked into `C:\mybizz\matt-pocock-skills-source\` | `C:\mybizz\mybizz-config-docs\matt-pocock-skills\` |
| Magdoub wireframe skill | `~/.claude/skills/wireframe/` (WSL) | `C:\mybizz\mybizz-config-docs\magdoub-wireframe\` |
| Graphify | `/home/dev-p/.local/bin/graphify` (WSL) | `C:\mybizz\mybizz-config-docs\graphify\` |
| PostgreSQL for GBrain | WSL, port 5432 | `C:\mybizz\mybizz-config-docs\postgresql\` |
| PostgreSQL for the PDLF ledger | Windows PostgreSQL 15, port 5433 | `C:\pdlf\pdlf-library\specifications-local\spec-postgres-ledger.md` |

---

## 8. Key Companion Documents

| Document | Location | Purpose |
|---|---|---|
| `docmap.md` | `C:\mybizz\mybizz-os-docs\os-reference-docs\docmap.md` | This map |
| [[[mybizz-os-docs:os-reference-docs/repo-organization\|Repo Organization]]](C:/mybizz/mybizz-os-docs/os-reference-docs/repo-organization.md) | `C:\mybizz\mybizz-os-docs\os-reference-docs\repo-organization.md` | Every repository: GitHub name, local paths, GBrain source |
| [[[mybizz-os-docs:os-reference-docs/mybizz-scope-list\|Mybizz Scope List]]](C:/mybizz/mybizz-os-docs/os-reference-docs/mybizz-scope-list.md) | `C:\mybizz\mybizz-os-docs\os-reference-docs\mybizz-scope-list.md` | What is in scope and what is never referenced |
| [[[mybizz-os-docs:operating-procedures/daily-ops\|Daily Ops]]](C:/mybizz/mybizz-os-docs/operating-procedures/daily-ops.md) | `C:\mybizz\mybizz-os-docs\operating-procedures\daily-ops.md` | Daily operations |
| [[[data-mybizz-mgt:desktop/master-task-list\|Master Task List]]](C:/data-mybizz-mgt/desktop/master-task-list.md) | `C:\data-mybizz-mgt\desktop\master-task-list.md` | Current task list |
| [[[data-mybizz-mgt:desktop/developers-duties\|Developers Duties]]](C:/data-mybizz-mgt/desktop/developers-duties.md) | `C:\data-mybizz-mgt\desktop\developers-duties.md` | Duties the developer carries out by hand |
| Global `AGENTS.md` | `~/.config/opencode/AGENTS.md` (WSL) | Global agent rules |
| Project `AGENTS.md` | Each project's repositories, generated from the project's reference file by `/mb-project-docs` | Project agent rules |
