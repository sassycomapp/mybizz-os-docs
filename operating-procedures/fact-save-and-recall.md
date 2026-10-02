---
title: "fact-save-and-recall"
doc-id: "mb-fdee2c8e"
updated: "2026-10-02"
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

- Facts log (the index): `C:\mybizz\logs\gbrain-logs\facts-log\facts-log.md`

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

(or close out a session — see [[[mybizz-os-docs:operating-procedures/opening-and-closing]]](C:/mybizz/mybizz-os-docs/operating-procedures/opening-and-closing.md); the close-out routine
does the same thing automatically). The AI writes the candidate facts as one
short list. It leaves out anything a document already says, and it checks
only the governed ones — the rules and decisions — against what GBrain
already knows, with a flag on anything that contradicts an existing fact.
You read the list once, cross out or reword anything, and give one approval.

After your approval the AI saves the advisory facts directly and writes a
file for each governed fact, which goes through the Memory Governor. Then it
gives you one block to paste into the WSL terminal. The block adds one row to
the facts log for each fact (a small helper numbers the row, so you never
type a number) and runs `commit-all.sh`. Only when governed facts were
submitted do you paste a second block, which approves them. The AI then
confirms what was saved, with each fact's reference number (for example
fact-47).

## Recalling facts

You never need to remember fact numbers. In any session:

- Ask naturally: "what do you know about communications?" or "what are
  the net-grant rules?" The AI searches GBrain and answers, citing the
  fact numbers it used.
- Ask to see the whole list: "show me the facts log." The table shows
  every durable fact with its number, date, subject, and text.

Every governed fact is saved into the documentation of its own project and also exists as a fact file there, so an ordinary GBrain search finds it.

## Retiring an outdated fact

When a fact in the log is no longer true or no longer wanted, point at its
row and say so — for example: "remove fact 003 from the facts log" (or
"facts 003 and 005"). The AI expires the fact inside GBrain — nothing is
silently erased; an audit trail remains — and removes the row from your
log. If the fact is not wrong but has been replaced by a better one, the
AI records the replacement first, then retires the old one, and tells you
it did so in that order.
