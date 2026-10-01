---
title: "frontmatter conventions"
doc-id: "mb-f484bbdf"
updated: "2026-10-01"
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

This applies to every document in the eight documentation repositories, including documentation skills. Documentation skills are present on C:\ and serve only documentation purposes. They are not installed skills.

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

The GBrain side of these rules is in [[[mybizz-config-docs:gbrain/gbrain-doc-contract]]](C:/mybizz/mybizz-config-docs/gbrain/gbrain-doc-contract.md).

**Awaiting re-certification by the developer.** The last certification (2026/09/27) covered the text before the changes of 2026-10-01.
