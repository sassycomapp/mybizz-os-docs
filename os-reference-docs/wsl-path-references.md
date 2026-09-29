---
title: "wsl-path-references"
doc-id: "mb-8904e69d"

---

## Skills

\\wsl.localhost\Ubuntu\home\dev-p\.config\opencode\skills

## AGENTS.md

\\\wsl.localhost\Ubuntu\home\dev-p\.config\opencode\AGENTS.md

## Cupcake Policies (.rego)

\\\wsl.localhost\Ubuntu\home\dev-p\.config\cupcake\policies\opencode

## Cupcake Plugin

\\\wsl.localhost\Ubuntu\home\dev-p\.config\opencode\plugin

## i-have-adhd

\\\wsl.localhost\Ubuntu\home\dev-p\.agents\skills\i-have-adhd\SKILL.md  
\\\wsl.localhost\Ubuntu\home\dev-p\.config\opencode\plugin\i-have-adhd.mjs" 
\\\wsl.localhost\Ubuntu\home\dev-p\.agents\skills\i-have-adhd\agents\openai.yaml  
\\\wsl.localhost\Ubuntu\home\dev-p\.agents\skills\i-have-adhd\agents\gemini.toml

## Cupcake - Enable, disable

documented in [[cupcake-reference|Usage, Invocation, and Worked Examples]] (mybizz-config-docs), line 275 (Disabling Cupcake — immediate, fully reversible):

```
mv ~/.config/opencode/plugin/cupcake.js ~/.config/opencode/plugin/cupcake.js.disabled
```

Restart OpenCode afterwards (plugin changes only load at process start). To re-enable:

```
mv ~/.config/opencode/plugin/cupcake.js.disabled ~/.config/opencode/plugin/cupcake.js
```

This matches the prior suspension described in [[cupcake-explainer|Mechanical Policy Enforcement for AI Coding Agents]] (mybizz-config-docs), line 260 — renaming the plugin out of its discoverable path, nothing else touched, and the `.bak-*` copies in the plugin folder confirm earlier uses.