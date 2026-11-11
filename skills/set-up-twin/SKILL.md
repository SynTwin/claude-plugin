---
name: set-up-twin
description: Connects the user's agent to the intended syntwin workspace, sets up their twin and rooms from context they choose to share, and agrees how handovers and future updates work. Use when the user asks to set up syntwin or their twin, or to get started, in whatever words, teach their twin, or establish a routine for keeping it up to date, or has just connected syntwin.
---

# Set up a syntwin

Help the owner use their own agent to set up and maintain syntwin. Resume existing work instead of starting the twin again. Talk with the owner in plain words: say what their twin will do and what you need from them, not which tool, step or record you use.

## Connect and verify

Use the syntwin connector provided by the host. The regular MCP endpoint is https://backend.syntwin.ai/api/mcp. The internal admin MCP is not a user connection.

If tools are missing, guide the owner through installation and OAuth using https://syntwin.ai/docs/how-to/connect-external-tools/. The plugin packages the workflow skills; a manual MCP connection can use this process too. Do not claim the marketplace listing is live without checking. Never ask for passwords, API keys, or tokens in chat.

Call `get_workspace`, `get_settings`, and `get_profile`. Name the workspace in one plain sentence, such as "I'm working in your workspace Test", and leave out roles, environments and permissions. Only if the owner says it's the wrong one, or it can't be the one they mean, explain that they reconnect syntwin in their app and pick the right workspace there. A prompt cannot switch the existing connection's workspace.

A successful read verifies read access only. Verify write access with the first approved setup change and read it back. If a call fails, report the failed step without claiming setup is complete.

## Set up in the app's order

Follow the same steps, in the same order, as onboarding in the syntwin app, so the owner ends with what the app would give them: a twin under their name and one room that does a clear job, has what it needs, and passed a test. Read `get_setup` for which of these steps are done, and resume at the first one still open; read before asking, and ask only for what the records don't answer. Where the owner shared material or asked you to use what you know about them, suggest a concrete answer for each step, such as their introduction, the room's job, or what it must never promise, and let them confirm or correct it instead of starting from a blank question. Mark what is a guess.

Use context the owner shares or explicitly asks to import. Do not search unrelated chats, files, memory, or accounts merely because the agent can reach them. Show the intended content and target records, and wait for approval before writing unless the owner already approved those changes.

1. **Identity.** The twin appears under the owner's name. Read `get_profile` and agree their name, role and company, a two-sentence introduction, and the languages the twin may speak. Save them with `update_profile`. A display picture comes from `create_avatar`.
2. **Calls**, optional. Without more the twin only chats. The owner's recording becomes its voice layer, so it can take audio calls; their portrait becomes its video layer, so with the voice layer it can take video calls too. See Voice and video layers. Offer it once in plain words, such as whether their twin should take calls, and move on if they skip it.
3. **The room's job.** Ask where the twin should talk for them, with whom, and what should come out of it, for example selling a product, guiding website visitors, networking, preparing a call, answering customers, screening applicants, investor questions, or answering as an expert. Read `list_deployments` and reuse a matching room; otherwise propose one with `create_deployment`. Agree how the twin should behave there and put that in the room's `instructions`. If it will appear on a website, read what the site offers so the instructions fit it.
4. **What the room needs.** From the job, name three to five things the room needs from the owner, such as a price list, a project overview, or answers to the typical questions. Read `get_twin_coverage` and relevant `list_sources` results first, and `get_source` before changing one. Fill each need with a file, link or a few sentences the owner provides, using `batch_upsert_sources`; the rest can wait. Leave out secrets, speculation presented as fact, and unrelated personal data. Report what each source fills.
5. **Room settings.** Agree what the twin must never promise and when it hands over to the owner, and add these rules to the room's `instructions` with `update_deployment`, merging with the current text because the tool replaces it. Agree which conversations it should highlight for the owner as `highlightCriteria`; they never change how the twin talks. Review `list_deployment_source_access` before changing access with `update_deployment_source_access` or `update_deployment_source_default`, and do not make private or person-specific material available to every visitor to finish setup.
6. **Test**, optional. Ask the questions typical visitors of this room would ask, with the review-conversations skill's preview flow (`start_conversation` with `preview: true`), or point the owner to the room's test in the app. Where the twin has to pass, name what is missing and go back to step 4. Never start a live visitor conversation as a substitute.

When the required steps are done, call `finish_setup`, so the app stops showing its guided setup; it completes there once the room's sources are processed. Report any step it still lists as open.

Read back changed records after each step. Check `search_knowledge` with a question the new material should answer, and report what was saved separately from what was retrieved; saving a source does not prove it is searchable yet.

Beyond onboarding: resolve people by name and email before creating them with `batch_upsert_people`, and ask when a match stays ambiguous. Set `externalProvider`, `externalId`, and `externalVersion` when the originating service provides stable values, so another run updates the same records. Use `personIds` for person-specific knowledge. Twin-wide behavior goes in `update_twin_instructions`, read first with `get_twin_instructions`; person context goes in `update_person`. A source never becomes an instruction.

## Voice and video layers

The twin always chats. Its voice layer, made from the owner's recording, lets it take audio calls; its video layer, a talking head from their portrait, lets it take video calls together with a voice layer. Both are optional, and setup can continue without them.

Read `list_video_replicas` and `list_voice_samples` before proposing new media. A display picture from `create_avatar` is different from the talking head created by `create_video_replica`.

Tell the owner about this as the voice layer and video layer, as the app calls them, the calls they enable, and their consent; don't say replica, sample or declaration to them. For new media, call `get_cloning_declaration` with `variant` `photo` or `voice` and the language of the recording, by default the language the owner writes in. Where the host shows cards, the owner accepts the declaration on the card themselves, picks their portrait or recording, and the card uploads it and tells you the new video layer or voice sample id. Never accept the declaration, call `record_cloning_release`, or invent a `cloningConsentId` on the owner's behalf; only the card may. Where the host shows no cards, give the owner the declaration's `releaseUrl`: the same step in the syntwin app.

`get_cloning_residence` and `get_media_release` read existing declarations and release status; they don't grant consent. Do not request internal IDs from the owner or bypass a missing marketplace tool through REST.

Check processing status before selecting ready media with `update_profile` and `update_deployment`. Report pending or failed processing honestly. Never claim that a recording was uploaded or a voice was cloned just because an upload URL or sample record exists.

## Hand work between the agent and the twin

When the owner names a person or conversation by its ID, as the buttons in the syntwin app do, read the person with `get_person` and the conversation with `get_session_transcript` before anything else.

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
