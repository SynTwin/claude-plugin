---
name: talk-to-twin
description: Relays a conversation between the owner and their own twin in syntwin's 1:1, where the twin can explain what it knows, take instructions, and make changes on the owner's behalf. Use when the user wants to talk to, ask, brief, or instruct their twin directly, or continue a 1:1 they started in the syntwin app, Telegram, or Slack.
---

# Talk to the twin in a 1:1

The 1:1 is the owner's private conversation with their twin. You relay; the twin answers.

## Process

1. Find the conversation: `list_one_on_ones` and `get_one_on_one` with `main` for the main 1:1. `start_one_on_one` without `parentId` returns the main 1:1; if there is none yet, it creates one and ends the owner's other open 1:1s, except those in Slack or Telegram. `topic` gives the twin something to take up in its opening. For a subthread, pass `parentId: "main"` with `topic` for an aside, or with `parentMessageId` to reply to one of the main 1:1's messages. Only the main 1:1 has subthreads.
2. Send the owner's words with `send_one_on_one_message` exactly as they wrote them. Do not add, rephrase, or answer on their behalf.
3. Show the twin's reply as the twin's words, without rewording. A Markdown link to `#reply` is a suggested reply: offer it, and send its label when the owner picks it.
4. When the twin asks a question or waits for an action, ask the owner. Answer with `resolve_one_on_one_action` only what the owner chose. Never confirm a deletion, merge, removal, or disconnect the owner has not explicitly confirmed to you.
5. If a reply fails, offer `retry_one_on_one_turn`. Use `cancel_one_on_one_turn` when the owner wants to stop a running reply.

Read older messages with `list_one_on_one_messages`. Do not send a message the owner did not write or approve.
