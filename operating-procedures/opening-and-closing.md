---
document: "Session Opening and Closing — Operating Procedure"
doc-id: opening-and-closing
state: Live
date-created: 2026-09-13
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

- Sessions log (the index): [[2026-09-15-session-numbers-from-sessions-log|session numbers come from the sessions log]]
- The session summaries themselves: GBrain pages named `sessions/...`

---

## Closing a session

When you are done working, simply say:

> close out

The AI then does the following, in order: it writes the session summary as
a GBrain page; it picks the next free number for the log; it draws up a
short list of durable facts from the session (rules, decisions, gotchas)
and checks each one against what GBrain already knows; and it shows you
that list. You read the list once. You can cross out or reword anything,
then give one approval — you do not approve each item separately.

After your approval the AI finishes on its own: important facts are saved
the governed way (through `mb-submit-memory`, so they can be trusted
later), advisory notes are saved directly, one row is added to the sessions
log with the number, date, topic, subject, title, and the exact GBrain page
name, and the AI confirms to you what was saved.

That is your entire job at close-out: say "close out", review one list,
say yes.

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
[[fact-save-and-recall|Operating Procedure]]).
