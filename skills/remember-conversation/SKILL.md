---
name: remember-conversation
description: Saves useful facts, decisions, commitments, and relationship context from a conversation, call, or meeting to syntwin as knowledge. Use when the user asks their twin to remember, learn from, or keep notes from a conversation or meeting.
---

# Remember a conversation

Store only context that improves a future conversation.

## Process

1. Extract durable facts, decisions, commitments, preferences, and relationship context. Leave out greetings, repetition, speculation presented as fact, credentials, and details with no likely future value.
2. Resolve people first: search `list_people` by name and email. Ask when identity stays ambiguous. Create a person only when none matches.
3. Look for an existing source of the same conversation with `list_sources` or `search_knowledge`. When the meeting has a stable id from its tool (calendar, notetaker, CRM), write through `batch_upsert_sources` with `externalProvider`, `externalId`, and `externalVersion`, so a second save updates the same source. Otherwise create one clearly titled source with `create_source`.
4. Link person-specific context to the matching people with `personIds`. Keep general knowledge unlinked.
5. Show the content and target records before writing.
6. Report the created or updated source and linked people. Never claim something was saved unless syntwin confirms it.

Do not send messages, change rooms, or widen the task beyond remembering the conversation.
