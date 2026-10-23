---
title: "adr-openai-embedding-provider"
doc-id: "mb-340d26bd"
updated: "2026-10-23"
---
# OpenAI Is GBrain's Embedding Provider

Status: Accepted

---

## Context

GBrain's embedding pipeline runs on OpenAI's `text-embedding-3-large` model. This is the model that converts stored content into the vectors semantic search depends on.

---

## Decision

**OpenAI is GBrain's embedding provider.** This holds regardless of any change to which provider handles chat, reasoning, or subagent routing — those are separate, independent choices from embedding specifically.

---

## Rationale

1. **Anthropic cannot perform embeddings at all** — confirmed directly via `gbrain providers list`. Anthropic's `EMBED` column shows a structural gap (a dash), not a missing-key or misconfiguration state. This is not a preference between providers; it is the absence of a capability.
2. Switching embedding providers requires re-embedding the entire existing corpus — a real, non-trivial cost, not a simple config flip.
3. OpenAI's `text-embedding-3-large` works correctly across every GBrain source.

---

## Consequences

- Any future GBrain schema-pack upgrade or provider-migration decision must explicitly preserve OpenAI as the embedding provider.
- The chat/reasoning model GBrain uses may still change independently of this decision — this ADR governs embedding only.
- The current configuration is correct; this ADR binds it against drift.

