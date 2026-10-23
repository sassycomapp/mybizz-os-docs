---
title: "grant-paths"
doc-id: "mb-f8fd29c2"
updated: "2026-10-23"
---
# Grant Paths

When an agent is blocked by Cupcake, it stops and asks the developer for permission. Its request names the exact folder (or domain) it needs and the number of writes (or requests) it needs. The developer then grants exactly that, with one of these commands:

```
mg-grant --path <folder> --count N
mg-revoke --path <folder>

net-grant --domain <domain> --count N
net-revoke --domain <domain>
```

Why: a grant that names its folder and count gives the agent only what the task needs, and the developer can see exactly what is being opened.
