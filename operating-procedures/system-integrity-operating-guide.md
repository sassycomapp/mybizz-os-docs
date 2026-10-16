---
title: "system-integrity-operating-guide"
doc-id: "mb-3c318153"
updated: "2026-10-16"
---
# System Integrity — Operating Guide

How and when the developer runs each of the eight System Integrity systems. The depth is in [[[mybizz-os-docs:os-reference-docs/system-integrity-explainer]]](C:/mybizz/mybizz-os-docs/os-reference-docs/system-integrity-explainer.md).

## The daily rhythm

1. **While working:** nothing to run. Cupcake guards every agent action by itself.
2. **To save work at any time:** run commit-all in the WSL terminal.
3. **When a section of work is finished:** run `/mb-close-out` in OpenCode. It ends with commit-all.

Everything else runs only when it is needed.

## The eight systems

### Cupcake — guards every agent action
- **When:** always on. Nothing to run.
- **Check it works:** after any restart, upgrade or settings change of OpenCode or Cupcake.
- **How to check:** in OpenCode, Build agent, type: `Fetch https://example.org`. The fetch must be blocked.
- **If every action is blocked** with `Cupcake exited with code 1`: Cupcake is broken. Fix it from the WSL terminal using the Cupcake reference.

### mb-doc-check — corrects documents and links at each commit
- **When:** inside every commit. Nothing to run.
- **If commit-all says BLOCKED:** read the reason it prints, fix it, run commit-all again.

### commit-all — commits, updates GBrain and pushes
- **When:** whenever work should be saved, and at the end of every close-out.
- **Where:** WSL terminal.
- **How:** `bash /mnt/c/mybizz/scripts/commit-all.sh "short message"`
- **Preview only:** `bash /mnt/c/mybizz/scripts/commit-all.sh --status`
- **Good result:** it ends with `All done.`

### mb-close-out — ends a section of work
- **When:** a section of work is finished.
- **Where:** OpenCode, Build agent, in the project worked in.
- **How:** type `/mb-close-out`. Approve its list once. Paste the terminal lines it gives, in order.

### mb-project-docs — keeps a project's documents current
- **When:** mb-close-out runs it for you. Run it alone only to update a project's documents mid-session.
- **Where:** OpenCode, Build agent, in the project (a project library, or app 1, 3 or 5).
- **How:** type `/mb-project-docs`. Approve. Paste the line it gives.

### mb-config-manager — checks each system against its documents
- **When:** after a system is installed, upgraded or changed, and as a routine check.
- **Where:** OpenCode. Press `Tab` until the mb-config-manager agent is selected.
- **How:** type `/mb-config-manager` to see which systems are due, or `/mb-config-manager system-name` to check one.
- **Then:** approve or reject each finding. Finish with commit-all.

### mb-sysdoc-creator — writes a system's four documents
- **When:** a new system needs documents. mb-config-manager also calls it to refresh documents.
- **Where:** OpenCode, Build agent, in `mybizz-config-docs`.
- **How:** type `/mb-sysdoc-creator system-name`. Give it any links or files about the system. Approve the drafts. Finish with commit-all.

### mb-compile-rules — makes a new Cupcake rule live
- **When:** a rule is added or changed.
- **First:** write the rule in plain words as a file in `C:\dev\project-library-global\rules-cupcake-global\`.
- **Where:** OpenCode, Build agent.
- **How:** type `/mb-compile-rules rule-name` (the file name without `.md`). Approve. Paste the lines it gives. Finish with commit-all.

## Rules that always apply
- **The agent never commits.** The developer runs commit-all.
- **Grants come from the developer.** When an agent asks for a grant line, paste it in the WSL terminal.
- **A block is final.** When Cupcake blocks an agent, the agent stops and asks. Never work around a block.
