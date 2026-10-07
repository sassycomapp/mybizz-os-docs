---
title: "opening-and-closing"
doc-id: "mb-d8690fef"
updated: "2026-10-07"
---
# Session Opening and Closing — Operating Procedure

This procedure is for you, the developer. It says how to close a session so that nothing is lost, and how to bring a past session back. The AI does the work; you start it, approve once, and paste one terminal block.

Everything important is stored outside the conversation: the session summary becomes a page in GBrain, and the durable facts are saved into GBrain memory. The conversation itself is never needed again, which is why every new session starts fast.

## Closing a session

Run `/mb-close-out`. In one run, the skill writes the session page in GBrain, lists the facts worth keeping, updates the project documents when you worked in a project, and shows you all of it once. You approve once, correcting anything you want first. The skill then saves the facts and gives you one terminal block, which ends with commit all. A second block appears only when a governed fact needs your promotion.

The skill's own file is the authoritative procedure: `C:\mybizz\mb-close-out\mb-close-out.md`.

## Opening a past session

In any new session, say "open the last session about <topic>". The AI searches GBrain's `sessions/` pages, loads the page, and tells you what that session did, what was left open, and what matters for your current work.

## Deleting an old session

Tell the AI which session page you no longer need. It deletes the GBrain page; a deleted page can be restored for three days. Facts from that session are not deleted with it; facts are retired only through [[[mybizz-os-docs:operating-procedures/fact-save-and-recall]]](C:/mybizz/mybizz-os-docs/operating-procedures/fact-save-and-recall.md).
