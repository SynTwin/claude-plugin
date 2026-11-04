---
name: find-lookalike-prospects
description: Uses syntwin Grow to find companies similar to a good conversation or contact, then people at those companies, and adds approved prospects to People. Use when the user asks for lookalike companies, prospects, leads, or who else to reach like a given customer.
---

# Find lookalike prospects with Grow

Grow research runs on the owner's own Clay or Ocean.io account, which bills each run. Confirm before each run.

## Process

1. Call `get_grow_overview`. It shows whether Grow is on, the connected research provider, result limits, suggested seeds, earlier results, and recent runs. Reuse earlier results before starting a new run.
2. If Grow is off or has no provider, tell the owner. Turning it on (`update_grow_settings`) needs their approval. A provider API key is entered by the owner in the syntwin app; never ask for it in chat.
3. Pick the seed: an ended visitor conversation (`sessionId`) or a person (`personId`), with its company name and domain. Show the seed, then call `start_grow_company_discovery` once the owner agrees. Pass a `requestId` so a retry does not start a second run.
4. Results appear in `get_grow_overview`. Review the companies with the owner and record decisions with `update_grow_company`.
5. For the companies the owner picks, confirm the run, then call `start_grow_prospect_discovery` with their ids and a `requestId`; those companies become approved. Record decisions on the people found with `update_grow_prospect`.
6. Add approved prospects with `import_grow_prospect`. Importing twice returns the same person.

Grow only researches and saves people. Contacting prospects is a separate step: use the prepare-contact-outreach skill, which never sends.
