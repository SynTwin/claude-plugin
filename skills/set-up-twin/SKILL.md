---
name: set-up-twin
description: Build or improve the user's SynTwin from profile details, writing, files, contacts, and other relevant context available to Claude. Use when the user asks to create, initialize, populate, or improve their digital twin.
---

# Set up a SynTwin

Build or improve the user's twin from relevant context already available to Claude.

## Process

1. Confirm the SynTwin MCP connection works. If it needs authentication, follow `${CLAUDE_PLUGIN_ROOT}/SETUP.md`.
2. Inspect the existing profile, settings, rules, sources, people, rooms, and open questions before proposing writes.
3. Identify useful profile facts, durable knowledge, relationships, communication preferences, and room behavior from connected sources. Do not import secrets, credentials, transient chatter, or unrelated personal data.
4. Show the user a concise summary of the intended changes. Resolve ambiguous identity matches before writing.
5. Use batch operations for multiple people or sources. Preserve external source identity and version when the originating service provides stable values.
6. Read back changed records and report what was added, updated, skipped, and still missing.

Never create duplicates to avoid resolving an ambiguous match. Do not alter authentication, billing, or organization settings.
