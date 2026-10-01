---
title: "opening-and-closing"
doc-id: "mb-d8690fef"
updated: "2026-10-01"
---
# Session Opening and Closing — Operating Procedure

This procedure is for you, the developer. It explains what you do to close
a session so nothing is lost, and how to bring a past session back in a new
one. The AI does all the work; you only say the trigger words and approve
the result once.

The method works because everything important is stored outside the
conversation: the full session summary becomes a page in GBrain, the
durable facts are saved into GBrain memory, and a one-row entry is added to
your sessions log so you can always find the page again by number. The
conversation itself is never needed again and is not carried into the new
session — which is why every new session starts fast.

**Your files:**

- Sessions log (the index): [[[mybizz-config-docs:facts/2026-09-15-session-numbers-from-sessions-log|session numbers come from the sessions log]]](C:/mybizz/mybizz-config-docs/facts/2026-09-15-session-numbers-from-sessions-log.md)
- The session summaries themselves: GBrain pages named `sessions/...`
- The close-out prompt the AI follows: [[[mybizz-os-docs:operating-procedures/close-out-process]]](C:/mybizz/mybizz-os-docs/operating-procedures/close-out-process.md)
- The commit step that ends every close-out: [[[mybizz-os-docs:operating-procedures/commit-all-repositories]]](C:/mybizz/mybizz-os-docs/operating-procedures/commit-all-repositories.md)

---

## Closing a session

When you are done working, simply say:

> close out

The AI then does the following, in order: it writes a short session summary
as a GBrain page (the documents hold the detail; the page says what happened
and where to look); it draws up a short list of durable facts (rules,
decisions, gotchas), leaving out anything a document already says, and checks
only the important ones against what GBrain already knows; and it shows you
that list together with the sessions-log row and the files that changed. You
read it once. You can cross out or reword anything, then give one approval —
you do not approve each item separately.

After your approval the AI saves the advisory notes directly and, for any
rule or decision that must be trusted later, writes a file for the memory
governor. Then it gives you one block to paste into the WSL terminal. The
block adds the row to the sessions log (a small helper numbers it, so you
never type or look up a number) and runs `commit-all.sh`, which commits all
eight repositories, updates GBrain and pushes everything. Only when governed
facts were submitted do you paste a second block, which approves them for the
governor. The AI then confirms what was saved.

Most sessions have no rule or decision to protect, so they close with the
first block only.

That is your entire job at close-out: say "close out", review one list, say
yes, paste one block.

## Opening a past session in a new session

In any new session, say:

> open session 001

(replace 001 with whichever number you want). The AI reads the sessions
log, finds the page name for that number, loads the page, and tells you
what that session did, what was left open, and what matters for your
current work. You never need to remember page names or dates — the number
is enough, because the log is the record.

You can also ask to see the whole log at any time ("show me the sessions
log"), or ask for a group, for example "summarize all sessions about
makepdlf" — the AI reads the table and answers from it.

## Deleting an old session

When sessions become redundant, tell the AI which number or date range you
no longer need. The AI deletes the GBrain page and removes the row from
the log. Deletion is not immediate: a deleted page can be restored for
three days. Facts from that session are not deleted with it — facts are
removed only through the facts procedure (see
[[[mybizz-os-docs:operating-procedures/fact-save-and-recall|Operating Procedure]]](C:/mybizz/mybizz-os-docs/operating-procedures/fact-save-and-recall.md)).
