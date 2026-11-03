---
name: prepare-contact-outreach
description: Prepares personalized syntwin room links, share context, and draft messages for contacts without sending anything. Use when the user wants contact-specific twin links for email, a CRM, a campaign, or another outreach workflow.
---

# Prepare contact outreach

Produce a contact-to-link mapping and, if asked, drafts. Never send outreach.

## Process

1. Resolve the room with `list_deployments`. Ask when more than one room plausibly fits.
2. Normalize the contacts. Email is the stable identity. Keep only context relevant to each future conversation.
3. Search `list_people` before writing. Show the room, contacts, and proposed person changes, then use `batch_upsert_people` for several contacts. Never create a duplicate.
4. Get links: `get_or_create_person_link` per contact, or `batch_get_personalized_links` once person ids are known. Passing `context` to `get_or_create_person_link` replaces that person's saved share context, so read `get_person_share_context` first and confirm before replacing it.
5. For drafts, call `draft_person_share_message` and put the person's link where it says `{{link}}`. Present drafts for the user to edit and send themselves.
6. Return a compact table of contact, room, and link. List skipped or failed contacts separately.

The workflow ends at records, links, and drafts. Do not email, message, post, or otherwise contact anyone, even through another connected tool, unless the user explicitly approves that exact message and recipient.
