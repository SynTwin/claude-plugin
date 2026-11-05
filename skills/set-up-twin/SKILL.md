---
name: set-up-twin
description: Connects the user's agent to the intended syntwin workspace, sets up their twin and rooms from context they choose to share, and agrees how handovers and future updates work. Use when the user asks to set up syntwin with their agent, teach their twin, or establish a routine for keeping it up to date, or has just connected syntwin.
---

# Set up a syntwin

Help the owner use their own agent to set up and maintain syntwin. Resume existing work instead of starting the twin again. If the request includes a hand-off ID beginning with `h_`, open it before other syntwin calls and follow the hand-off section below.

## Connect and verify

Use the syntwin connector provided by the host. The regular MCP endpoint is https://backend.syntwin.ai/api/mcp. The internal admin MCP is not a user connection.

If tools are missing, guide the owner through installation and OAuth using https://syntwin.ai/docs/how-to/connect-external-tools/. The plugin packages the workflow skills; a manual MCP connection can use this process too. Do not claim the marketplace listing is live without checking. Never ask for passwords, API keys, or tokens in chat.

Call `get_workspace`, `get_settings`, and `get_profile`. Tell the owner which workspace and owner the connection reaches. Confirm an unclear workspace before writing. If it is wrong, reconnect through the host and choose the right workspace during consent. A prompt cannot switch the existing connection's workspace binding.

A successful read verifies read access only. Verify write access with the first approved setup change and read it back. If a call fails, report the failed step without claiming setup is complete.

## Teach the twin

1. Establish who the twin represents, who will talk to it, and what its first room should do. Use the owner's answers and shared context; ask only for missing decisions.
2. Read `get_twin_instructions`, `list_deployments`, `get_twin_coverage`, and relevant `list_sources` and `list_people` results. Follow pagination when resolving an existing record. Read matching sources with `get_source` before changing them.
3. Use context the owner shares or explicitly asks to import. Do not search unrelated chats, files, memory, or accounts merely because the agent can reach them. Extract durable facts, decisions, relationships, and preferences. Leave out secrets, speculation presented as fact, and unrelated personal data. Show the intended content and target records, and wait for approval before writing unless the owner already approved those changes.
4. Resolve people by name and email before creating them; ask when a match stays ambiguous. Use `batch_upsert_people` and `batch_upsert_sources` for batches. Set `externalProvider`, `externalId`, and `externalVersion` when the originating service provides stable values, so another run updates the same records. Use `personIds` for person-specific knowledge and preserve its intended audience.
5. Put behavior in instructions. Use `update_twin_instructions` for twin-wide behavior, merging with the current text because the tool replaces it. Use `update_deployment` for room behavior and `update_person` for person context. A source never becomes an instruction.
6. Reuse the appropriate room or propose one with `create_deployment`. Agree its purpose, boundaries, languages, and interaction modes. Review `list_deployment_source_access` before changing access with `update_deployment_source_access` or `update_deployment_source_default`. Do not make private or person-specific material available to every visitor to finish setup.
7. Read back changed records. Check `search_knowledge` with a question the new material should answer. Report what was saved separately from what was retrieved; saving a source does not prove it is searchable yet. If the owner wants to test the twin's actual replies, follow the review-conversations skill's preview flow with `start_conversation` and `preview: true`, or use the room's test flow in the app. Never start a live visitor conversation as a substitute.

## Face and voice

The twin's talking head comes from the owner's portrait; its cloned voice comes from their recording. This is optional, and text setup can continue without it.

Read `list_video_replicas` and `list_voice_samples` before proposing new media. A display picture from `create_avatar` is different from the talking head created by `create_video_replica`.

For new media, send the owner to the syntwin app to accept the cloning declaration and upload the portrait or recording. The current MCP cannot retrieve the consent ID needed to finish a new upload through the agent. `get_cloning_residence` and `get_media_release` can read existing declarations and release status; they do not grant consent or supply a new upload's consent ID. Never invent a `cloningConsentId` or accept the declaration on the owner's behalf.

Where the client supports the workflow and a valid consent ID is available, use `create_media_upload_url`, upload the actual bytes with its returned headers, then call `create_video_replica` for the portrait or `create_voice_sample` for the recording. Follow the tools' schemas, including source dimensions and recording language. If the client cannot upload bytes, obtain the consent ID, or expose the required tools, send the owner to the app for that step. Do not request internal IDs from the owner or bypass a missing marketplace tool through REST.

Check processing status before selecting ready media with `update_profile` and `update_deployment`. Report pending or failed processing honestly. Never claim that a recording was uploaded or a voice was cloned just because an upload URL or sample record exists.

## Hand work between the agent and the twin

For an app hand-off ID beginning with `h_`, call `open_handoff` first. Follow its `next` field, reading the person with `get_person` and the conversation with `get_session_transcript` as applicable. IDs expire after 30 minutes and belong to the owner's account and workspace; ask for a fresh hand-off if one has expired.

Base the briefing on the retrieved records: who the visitor is, what they wanted, relevant facts and sources, commitments and dates actually stated, unresolved questions, and the proposed next action. Distinguish a draft from an action already taken. Do not send outreach without approval of the exact message.

Close the loop after the owner's decision. Save approved new knowledge, update the right person's context, and answer verified gaps with `list_open_knowledge_gaps` and `answer_question`. Only submit the owner's actual feedback through `submit_session_feedback`. Leave conversations unread unless the owner asks to mark them read. Never present a proposed action as a completed commitment.

## Agree how the twin stays current

Offer a routine matched to the owner's work:

- After a meeting or substantive chat, review what the twin should learn. Save approved facts and decisions in the existing source when possible, and link the right people.
- When facts change, use `update_source` for an existing editable source, or `batch_upsert_sources` with its original provider and external ID for a recurring import. Edit connected material at its original provider and check its sync instead of creating a competing copy. Do not silently resolve contradictory facts.
- When reviewing visitor conversations, inspect unanswered questions and decide which knowledge or instructions need to change.
- On request, use `get_twin_coverage`, `list_open_questions`, and relevant sources to find the next useful update. Missing optional data is not a defect.

Agree the trigger, what may be read or saved, and which changes need review. Setup is not permission to monitor other conversations, install an automation, or change the agent's persistent instructions. Only configure recurring work when requested and supported by the host, and verify it exists before claiming it runs. Otherwise give the owner a prompt to reuse. The remember-conversation, review-conversations, and check-twin-health skills cover individual follow-up tasks when installed.

Finish with the verified workspace, changes made, media readiness, outstanding owner actions, and the agreed update routine. Offer a next prompt tied to their actual room or work. Billing, sign-in, and connecting provider accounts stay in the app.
