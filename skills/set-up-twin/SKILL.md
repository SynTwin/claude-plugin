---
name: set-up-twin
description: Builds or improves the user's syntwin from profile details, writing, files, contacts, and other context the assistant can reach. Use when the user asks to create, initialize, populate, teach, or improve their digital twin, or has just connected syntwin.
---

# Set up a syntwin

Build or improve the user's twin from relevant context already available in this chat.

## Process

1. Confirm the syntwin connection works by calling `get_profile`. If it needs sign-in, ask the user to authenticate the syntwin connector and pick the workspace they want to manage.
2. Read what exists before proposing writes: `get_profile`, `get_settings`, `list_rules`, `list_sources`, `list_people`, `list_deployments` (rooms), and `get_twin_coverage`, which shows which facet a new source would deepen most.
3. Pick useful profile facts, durable knowledge, relationships, communication preferences, and room behavior from the available context. Prefer what fills the weakest coverage facet. Leave out secrets, credentials, transient chatter, and unrelated personal data.
4. Show a concise summary of the intended changes. Resolve ambiguous identity matches with the user before writing.
5. Write in batches: `batch_upsert_sources` for several text sources, `batch_upsert_people` for contacts. Set `externalProvider`, `externalId`, and `externalVersion` when the originating service gives stable values, so a rerun updates instead of duplicating.
6. Read back changed records. Report what was added, updated, skipped, and which coverage facets or open questions (`list_open_questions`) still need the user.

Never create a duplicate to avoid resolving an ambiguous match. Billing, sign-in, and connecting accounts stay in the syntwin app.
