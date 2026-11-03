---
name: open-handoff
description: Opens a syntwin hand-off, reads the person or conversation it points to, and helps the owner act on it. Use when the user's message contains a syntwin hand-off id starting with h_, or says they handed a person or conversation over from syntwin.
---

# Open a syntwin hand-off

The owner pressed **Hand over** on a person or a conversation in syntwin. The prompt holds only a hand-off id like `h_…`.

## Process

1. Call `open_handoff` with the id before anything else. It works for 30 minutes and only for the owner's account in this workspace. If it is not found or expired, ask the owner to hand it over again from syntwin. Never guess ids.
2. Follow its `next` field:
   - A conversation: read it with `get_session_transcript` using `sessionId`. When `personId` is set, read the person with `get_person` too.
   - A person: read them with `get_person`, then their conversations with `list_sessions` filtered by `personId`. Open the ones that matter with `get_session_transcript`.
3. Summarize in a few lines: who it is, what they wanted, open questions or commitments, and what the twin could not answer.
4. Ask the owner what they need. Typical next steps:
   - Draft a reply or follow-up for the owner to send.
   - Save new facts with the remember-conversation skill.
   - Answer what the twin could not with `list_open_knowledge_gaps` and `answer_question`.
   - Give the twin feedback on the conversation with `submit_session_feedback`, passing the owner's words unchanged.
   - Find similar companies with the find-lookalike-prospects skill.
5. Read records back after any write and report what changed.

Drafts stay drafts. Never send a message, email, or invitation to the person, through syntwin or another tool, unless the owner explicitly approves that exact message.
