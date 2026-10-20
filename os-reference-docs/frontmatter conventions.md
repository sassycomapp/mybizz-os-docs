---
title: "frontmatter conventions"
doc-id: "mb-f484bbdf"
updated: "2026-10-20"
---
frontmatter conventions:

a) Documentation documents have this frontmatter:

```
---
title:
doc-id:
updated:
---
```

This applies to every document in the documentation repositories listed in [[[mybizz-os-docs:operating-procedures/commit-all-repositories]]](C:/mybizz/mybizz-os-docs/operating-procedures/commit-all-repositories.md), including documentation skills. The files and folders the program skips are listed in [[[mybizz-config-docs:mb-doc-check/doc-check-config]]](C:/mybizz/mybizz-config-docs/mb-doc-check/doc-check-config.md). Documentation skills are present on C:\ and serve only documentation purposes. They are not installed skills.

- `title`: the file name without `.md`, in double quotes.
- `doc-id`: a permanent random code such as `mb-3fa91c2e`. It never changes, not even when the file is renamed or moved.
- `updated`: the date of the last real change, for example `"2026-10-01"`.

The commit hook `mb-doc-check` adds and keeps all three. Nobody writes `doc-id` or `updated` by hand. Never name a field `date`: GBrain reads it as the page's date and it can change search ranking.

b) All installed skills have this frontmatter:

```
---
name:
description:
---
```

Installed skills are in WSL. `SKILL.md` files never get a `doc-id`, and the hook never touches them.

c) Documents are stored with LF line endings.

d) `obsolete` folders are never touched by the hook.

The GBrain side of these rules is in [[mybizz-config-docs:gbrain/gbrain-doc-contract]].

