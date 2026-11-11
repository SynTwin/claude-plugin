---
name: set-up-automation
description: Creates, tests, and maintains syntwin automations, webhooks that POST to the owner's own endpoint when a visitor conversation finishes. Use when the user wants syntwin to notify, trigger, or send conversation results to Zapier, Make, n8n, a CRM, or their own service.
---

# Set up an automation

An automation POSTs to a public HTTPS URL whenever an owner event happens. Today that event is `conversation.completed`.

## Process

1. Read `list_automations`. If one already serves the same endpoint, changing it happens in the syntwin app's Automations page; send the owner there instead of creating a second one.
2. Collect the endpoint URL and an optional JSON body template. Never ask for secrets: an endpoint that needs headers, such as an API key, gets them from the owner in the app after you create the automation.
3. Show the event, URL, and body template, then call `create_automation`. Headers, and every change to an existing automation, happen in the app.
4. Offer `test_automation`. It sends one sample event to the endpoint now, and the sample may be the owner's latest real visitor conversation, so say that and ask first. Report how the endpoint answered.
5. Delete with `delete_automation` only on explicit request.

Stored secret header values are never returned. Do not point an automation at a URL the owner did not give you.
