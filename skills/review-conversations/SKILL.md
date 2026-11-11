---
name: review-conversations
description: Reviews conversations visitors had with the owner's syntwin rooms, summarizes them, closes knowledge gaps, gives the twin feedback, and tests rooms in preview. Use when the user asks what happened in their rooms, wants to work through their inbox, improve the twin's answers, share a transcript, or try a room as a visitor would.
---

# Review conversations

Tell the owner what happened in plain words: who talked with their twin, what they wanted, and what needs them. Say "places where your twin talks for you" for rooms and "what your twin knows" for sources, and leave out tool names and the workspace unless they ask.

## Work through the inbox

1. Find conversations with `list_sessions` (filter by room with `deploymentId` or by person with `personId`). Read each one with `get_session_transcript`.
2. Summarize per conversation: who, what they wanted, outcome, and follow-ups. Group recurring questions.
3. For what the twin could not answer, read `list_open_knowledge_gaps`. Propose an answer from existing sources or ask the owner; answer with `answer_question` only with the owner's approval of the wording.
4. To correct the twin's behavior, ask the owner for the feedback and pass it unchanged to `submit_session_feedback`.
5. Mark reviewed conversations read with `mark_sessions_read` once the owner agrees.

## Share a transcript

`share_conversation` returns a read-only link anyone with it can open. Create it only when the owner asks, and remind them `unshare_conversation` revokes it.

## Try a room

1. Start with `start_conversation` and `preview: true`, so the conversation counts as the owner's preview. Add `personToken` to preview as a specific person.
2. Send the owner's test messages unchanged with `send_conversation_message` and show the twin's replies as the twin's.
3. When the twin asks for an action, ask the owner; answer with `resolve_conversation_action` only with what they chose.
4. End with `end_conversation`.

Never start a conversation as a real visitor or invent visitor messages. Deleting conversations needs explicit confirmation.
