---
document: "disable-enable-cupcake"
doc-id: disable-enable-cupcake
state: Live
date-created: 2026-09-17
---
documented in [[cupcake-reference|Usage, Invocation, and Worked Examples]] (mybizz-config-docs), line 275 (Disabling Cupcake — immediate, fully reversible):

```
mv ~/.config/opencode/plugin/cupcake.js ~/.config/opencode/plugin/cupcake.js.disabled
```

Restart OpenCode afterwards (plugin changes only load at process start). To re-enable:

```
mv ~/.config/opencode/plugin/cupcake.js.disabled ~/.config/opencode/plugin/cupcake.js
```

This matches the prior suspension described in [[cupcake-explainer|Mechanical Policy Enforcement for AI Coding Agents]] (mybizz-config-docs), line 260 — renaming the plugin out of its discoverable path, nothing else touched, and the `.bak-*` copies in the plugin folder confirm earlier uses.
