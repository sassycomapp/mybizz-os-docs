---
title: "frontmatter conventions"
doc-id: "mb-f484bbdf"
---
frontmatter conventions:



a) Documentation skills have this frontmatter:



\---

title:

doc-id:

\---



Documentation skills present on C:\\ and serve only documentation purposes. They are not installed skills. 



b) All Installed skills Have this frontmatter



\---

name:

description

\---



Installed skills are at wsl

\---

**This document is Certified Correct 2026/09/27**

**Addendum 2026-09-30 — not covered by the certification above (awaiting the developer's re-certification):**

c) Every documentation document — not only documentation skills — carries `title` and `doc-id`. The `doc-id` is a permanent random code, for example `doc-id: "mb-3fa91c2e"`. It is added by `mb-doc-check` when a document is first committed and it never changes when the file is renamed or moved: it is how the commit hook recognises a renamed document and repairs the links to it. Everything else in a document's frontmatter is left alone. `SKILL.md` files (installed skills) never receive one, and `obsolete` folders are never touched. The GBrain side of the rule is in [[gbrain-doc-contract]].
