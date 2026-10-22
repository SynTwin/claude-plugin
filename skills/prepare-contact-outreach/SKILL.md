---
name: prepare-contact-outreach
description: Prepare personalized syntwin conversation links for contacts without sending outreach. Use when the user wants contact-specific twin links for email, CRM, or another outreach workflow.
---

# Prepare contact outreach

Create a concise contact-to-link mapping. Never send the outreach.

## Process

1. List existing rooms and resolve the target room. Ask the user when more than one room plausibly fits.
2. Normalize the supplied contacts. Email is the stable identity. Keep only context relevant to each future conversation.
3. Search existing people before writing. Use the batch upsert operation for multiple contacts and do not create duplicate records.
4. Show the room, contacts, and proposed person changes before writing.
5. Fetch personalized links in one batch once person ids are known.
6. Return a compact mapping containing contact, room, and link. Report skipped or failed contacts separately.

Preparing records and links is the end of this workflow. Do not email, message, publish, or otherwise contact anyone.
