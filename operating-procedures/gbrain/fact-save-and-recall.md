---
document: "Fact Save and Recall — Operating Procedure"
doc-id: fact-save-and-recall
state: Live
date-created: 2026-09-13
---

# Fact Save and Recall — Operating Procedure

This procedure is for you, the developer. It explains what a fact is, how
you get facts saved, how you look them up later, and how you retire one
that is out of date. The AI does the work; you say the trigger, review one
list, and approve once.

A fact is a single short instruction or piece of knowledge the AI must
trust later without re-checking it — for example, "write in complete
sentences" or "grant web access with net-grant". Every fact lives inside
GBrain memory, and every fact appears as one row in your facts log, so you
always have a readable list of everything the AI permanently believes.

**Your file:**

- Facts log (the index): `C:\_data-mybizz-mgt\_desktop\Facts-log.md`

The two kinds of facts, and the difference that matters:

- **Governed facts** are things that must be believed later without
  re-checking: rules, decisions, procedures. These are saved through the
  Memory Governor (via the `mb-submit-memory` skill), which verifies who
  sent them and checks them for contradictions. Slower, deliberate — that
  is the point.
- **Advisory facts** are context and pointers that just need to be findable
  again, not officially trusted. These are saved directly. Fast.

You do not need to decide which kind a fact is; the AI sorts them and
tells you.

---

## Saving facts

At any point in a session, say:

> save these as facts

(or close out a session — the close-out routine does the same thing
automatically). The AI writes the candidate facts as one short list,
checks them against what GBrain already knows, and shows you the list with
a flag on anything that contradicts an existing fact. You read the list
once, cross out or reword anything, and give one approval.

The AI then saves the approved facts the correct way for each kind, adds a
row to the facts log for every fact saved, and confirms to you what was
saved, with each fact's reference number (for example fact-47).

## Recalling facts

You never need to remember fact numbers. In any session:

- Ask naturally: "what do you know about communications?" or "what are
  the net-grant rules?" The AI searches GBrain and answers, citing the
  fact numbers it used.
- Ask to see the whole list: "show me the facts log." The table shows
  every durable fact with its number, date, subject, and text.

## Retiring an outdated fact

When a fact in the log is no longer true or no longer wanted, point at its
row and say so — for example: "remove fact 003 from the facts log" (or
"facts 003 and 005"). The AI expires the fact inside GBrain — nothing is
silently erased; an audit trail remains — and removes the row from your
log. If the fact is not wrong but has been replaced by a better one, the
AI records the replacement first, then retires the old one, and tells you
it did so in that order.
