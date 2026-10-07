---
title: "fact-save-and-recall"
doc-id: "mb-fdee2c8e"
updated: "2026-10-07"
---
# Fact Save and Recall — Operating Procedure

This procedure is for you, the developer. It explains what a fact is, how facts are saved, how you look them up, and how you retire one that is out of date. The AI does the work; you review one list and approve once.

A fact is a single short instruction or piece of knowledge the AI must trust later without re-checking it, for example "write in complete sentences" or "grant web access with net-grant". Every fact lives inside GBrain memory and has a number, for example fact-47.

The two kinds of facts:

- **Governed facts** must be believed later without re-checking: rules, decisions, procedures. They are saved through the memory governor by the `mb-submit-memory` skill, which verifies who sent them and checks them for contradictions. Slower and deliberate, on purpose.
- **Advisory facts** are context and pointers that only need to be found again. They are saved directly. Fast.

You do not decide which kind a fact is; the AI sorts them and tells you.

## Saving facts

Facts are saved at close-out: run `/mb-close-out` (see [[[mybizz-os-docs:operating-procedures/opening-and-closing]]](C:/mybizz/mybizz-os-docs/operating-procedures/opening-and-closing.md)). At any other time, say "save these as facts" and the same steps run. The AI lists the candidate facts once, leaving out anything a document already says, and checks only the governed ones against what GBrain already knows, flagging anything that contradicts an existing fact. You read the list once, correct anything, and approve once.

The AI then saves the advisory facts directly and submits each governed fact through `mb-submit-memory`. It gives you one terminal block, which runs commit all. Only when a governed fact lands in review do you paste a second block, which promotes it. The AI confirms what was saved, with each fact's number.

Every governed fact is also saved as a fact file in the documentation of its own project, so an ordinary GBrain search finds it.

## Recalling facts

You never need to remember fact numbers. Ask naturally, for example "what do you know about communications?" or "what are the net-grant rules?". The AI searches GBrain and answers, citing the fact numbers it used.

## Retiring an outdated fact

Name the fact by its number, for example "retire fact-47". The AI expires it inside GBrain; nothing is silently erased, and an audit trail remains. If the fact has been replaced by a better one, the AI records the replacement first, then retires the old one, and tells you it did so in that order.
