# Matt Pocock Skills Reference

## What This Is

Matt Pocock's skill set for AI coding agents — `github.com/mattpocock/skills`. Independent of GStack and GBrain.

---

## Install Architecture

**Source clone (authoritative):** `C:\mybizz\matt-pocock-skills-source` (WSL: `/mnt/c/mybizz/matt-pocock-skills-source`).

**Live install:** `~/.agents/skills/` — the fixed, default skill directory this tool reads from. Shared with Anvil's 5 skills (see `anvil-skills-reference.md`); not shared with GStack, which uses its own separate skill directory.

**Mechanism:** every skill folder under `~/.agents/skills/` is a symbolic link pointing into the source clone above. Nothing is copied. Updating the source clone updates every live skill automatically.

**Browsable via Windows Explorer:** `C:\mybizz\skills-collective\matt-pocock-skills` — a Windows symbolic link into `~/.agents/skills/`. Editing through this path edits the real, live skill.

**Total: 35 skills installed.**

---

## Mandatory Per-Project Setup

**`/setup-matt-pocock-skills` must be run once in every project before any other engineering skill in this set is used in that project.** This is a hard requirement, not optional — skills like `to-tickets` depend on its output and will not function correctly without it.

**What it configures, per project:**
- Issue tracker — where issues live (GitHub by default; local markdown also supported)
- Triage labels — the label vocabulary used for the five canonical triage roles
- Domain doc layout — where `CONTEXT.md` and ADRs live, and how they're consumed

**Files it produces, inside that project's own `agents/` folder:**
- `agents/domain.md`
- `agents/instructions.md`
- `agents/issue-tracker.md`
- `agents/triage-labels.md`

**How to check if it's already been run in a given project:**

```bash
ls <project-path>/agents/
```

If all four files above are present, it has been run. If the folder is missing or incomplete, run `/setup-matt-pocock-skills` in that project before using any other engineering skill there.

**Currently confirmed run in:** `makepdlf-project-library`.

---

## Full Skill List, by Category

### Engineering (18)
`ask-matt`, `code-review`, `codebase-design`, `diagnosing-bugs`, `domain-modeling`, `grill-with-docs`, `implement`, `improve-codebase-architecture`, `prototype`, `research`, `resolving-merge-conflicts`, `setup-matt-pocock-skills`, `tdd`, `to-spec`, `to-tickets`, `triage`, `wayfinder`, `wizard`

### In Progress (6)
`claude-handoff`, `loop-me`, `setup-ts-deep-modules`, `writing-beats`, `writing-fragments`, `writing-shape`

### Misc (4)
`git-guardrails-claude-code`, `migrate-to-shoehorn`, `scaffold-exercises`, `setup-pre-commit`

### Productivity (7)
`grill-me`, `grilling`, `handoff`, `teach`, `to-questionnaire`, `wait-what`, `writing-for-agents`

---

## OpenCode Integration

Skills are invoked as slash commands in OpenCode chat (e.g. `/domain-modeling`, `/triage`). OpenCode reads directly from `~/.agents/skills/`; no separate registration step beyond the symlink existing.

**Verify a skill is reachable:**

```bash
cat ~/.agents/skills/<skill-name>/SKILL.md | head -5
```

Should show real `name:`/`description:` frontmatter. If the command fails to run in the picker despite the file existing, check that the symlink target still resolves:

```bash
ls -la ~/.agents/skills/<skill-name>
```

A broken symlink (target shown in red, or `ls` reporting "No such file or directory") means the source clone was moved or deleted without updating the link.

---

## Skills Used in PDLF's Pipeline

| Skill | PDLF step |
|---|---|
| `setup-matt-pocock-skills` | Step 11 |
| `domain-modeling` | Step 16 |
| `to-tickets` | Step 19 |
| `triage` | Step 39 |
| `improve-codebase-architecture` | Step 40 (optional) |

All other skills in the full 35 are installed and available but not referenced in PDLF's current pipeline.

`to-spec` and `prototype` are installed but deliberately excluded from PDLF's pipeline — PDLF uses GStack's own `/spec` skill instead of `to-spec`; `prototype` is not part of the pipeline.

---

## Dependencies Between Skills

- `/implement` invokes `/code-review` automatically when finishing a task. Both are installed.
- `/to-tickets` requires `/setup-matt-pocock-skills` to have already run in the project.
- Any skill under "Engineering" requires `/setup-matt-pocock-skills` to have run in that project first (see "Mandatory Per-Project Setup" above).

---

## Update Procedure

```bash
cd /mnt/c/mybizz/matt-pocock-skills-source
git pull
```

Symlinked skills update automatically — no re-linking needed for existing skills.

**If the update adds new skill folders, symlink them:**

```bash
for f in /mnt/c/mybizz/matt-pocock-skills-source/skills/*/*/; do
  name=$(basename "$f")
  [ -e ~/.agents/skills/"$name" ] || ln -s "$f" ~/.agents/skills/"$name"
done
```

**Verify the total count matches after any update:**

```bash
find /mnt/c/mybizz/matt-pocock-skills-source -name "SKILL.md" | wc -l
ls ~/.agents/skills/ | wc -l
```

The second number should equal the first, plus 5 (for the separately-managed Anvil skills).

---

*End of document*
