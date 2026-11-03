---
name: set-up-automation
description: Creates, tests, and maintains syntwin automations, webhooks that POST to the owner's own endpoint when a visitor conversation finishes. Use when the user wants syntwin to notify, trigger, or send conversation results to Zapier, Make, n8n, a CRM, or their own service.
---

# Set up an automation

An automation POSTs to a public HTTPS URL whenever an owner event happens. Today that event is `conversation.completed`.

## Process

1. Read `list_automations`. Update an existing automation for the same endpoint instead of creating a second one.
2. Collect the endpoint URL, any headers the endpoint needs, and an optional JSON body template. The owner pastes header secrets themselves; never print, guess, or repeat a secret value.
3. Show the event, URL, header names, and body template, then call `create_automation` or `update_automation`. An update replaces the whole automation; send a secret header without a value to keep its stored value.
4. Offer `test_automation`. It sends one sample event to the endpoint now, and the sample may be the owner's latest real visitor conversation, so say that and ask first. Report how the endpoint answered.
5. Delete with `delete_automation` only on explicit request.

Stored secret header values are never returned. Do not point an automation at a URL the owner did not give you.
