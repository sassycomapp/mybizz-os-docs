---
title: "adr-build-artefact-timestamps"
doc-id: "mb-9e052b00"
updated: "2026-10-23"
---
# Build Artefact Timestamps

**Status:** Accepted

---

## Context

MyBizz's own tooling records timestamps in frontmatter, logs, learnings entries and backup folder names. All of them are created on one computer, in one place, in South Africa, where there is no daylight saving. Client data follows a different rule: every Mybizz application stores datetimes in UTC and displays them in each client's own timezone, held as an IANA string.

---

## Decision

Applies to every timestamp recorded by Mybizz's own tooling —
front matter (`date-created`), alignment logs, learnings entries, backup
folder names and any other build artefact. It does not apply to client
data.

**Format: Filename-Safe ISO 8601, UTC+2, with seconds.**

```
YYYY-MM-DDTHHMMSS+0200
```

Example: `2026-07-25T143045+0200`

- Date portion keeps hyphens (`YYYY-MM-DD`).
- Time portion drops colons (`HHMMSS`, not `HH:MM:SS`) — colons are invalid in
  Windows filenames and this format must work equally as a timestamp value and as
  part of a folder/file name.
- Timezone offset is always `+0200`, stated explicitly, never omitted — this system
  does not use UTC or "local time" as an implicit default; UTC+2 is stated on every
  timestamp so there is never ambiguity about which timezone a given value is in.
- Seconds are included even where minute-level precision would usually suffice, so
  that two artefacts created close together never collide on an identical timestamp.

### Required conversion mechanism

Any tool or agent generating this timestamp must use timezone-aware conversion —
never generate a UTC time and then append the literal string `+0200` onto it. That
produces a timestamp mislabeled by two hours: the value inside is UTC, the label
claims UTC+2, and the two disagree.

**This is the only permitted method. There is no fallback or alternative.**
```
TZ="Africa/Johannesburg" date -d "@<unix-timestamp>" '+%Y-%m-%dT%H%M%S%z'
```
`TZ="Africa/Johannesburg"` shifts the actual clock value to local time; `%z` then
computes and prints the real offset for that shifted value. Both the digits and the
offset label come from the same conversion — they cannot disagree.

If a tool or environment is ever encountered where this exact method cannot be used,
that is not a judgment call for the agent to resolve on its own — stop and ask the
developer for an approved method before generating any timestamp. Do not invent or
select an alternative conversion approach.

---

## Why

A fixed `+0200` offset is correct for one computer in one timezone with no daylight saving, and stating it on every timestamp removes any doubt about which timezone a value is in. Client data needs a region, not an offset, because a region's offset changes with daylight saving and clients are in many regions. The two rules never mix: an IANA string is never used for a build artefact, and a fixed offset is never used for client data.
