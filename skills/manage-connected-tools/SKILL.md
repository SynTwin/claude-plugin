---
name: manage-connected-tools
description: Checks and manages what syntwin reads from HubSpot, Google Drive, and DocuWare, and which outside MCP tools a room's twin may call during visitor conversations. Use when the user asks about connected tools, syncing, which files or CRM records a room or person may use, or adding an MCP server such as a shop search to a room.
---

# Manage connected tools

## HubSpot, Google Drive, DocuWare

1. Read `list_integrations` for connection status, last sync, and what each mirrors.
2. Connecting HubSpot or Google Drive, and creating API keys, happen in the syntwin app. Send the owner there; never ask for passwords or keys in chat. `connect_docuware` takes credentials, so only use it when the owner enters them for that purpose.
3. To change what a room or person may use, read `get_integration_scope_rules` first. Browse Drive or DocuWare with `list_integration_items` (and `batch_get_drive_folder_paths` for folder locations). To give a room only part of a DocuWare archive, read the archive's fields with `list_docuware_archive_fields` and the values they hold with `list_docuware_archive_values`, then add `fieldConditions` to the archive's rule, such as Kunden-Nr = 12032; a condition's `match` can also be `startsWith`, `contains` or `empty`. Before saving, list the documents the new rules select with `list_docuware_filter_matches` and show them with the change; a document to leave out gets its own file rule with access `unavailable`. Show the change, then `set_integration_scope_rules`, which replaces the rules and syncs.
4. `refresh_integration` checks a connection; `sync_integration` mirrors now. If a connection needs reconnecting, send the owner to the app. `otherAccount: true` means the owner signed in to a different account than the one connected: they reconnect with the connected account, or add the other one in the app (HubSpot, Google Drive). Gmail takes one mailbox, so they disconnect it in the app before connecting another.
5. `disconnect_integration` removes the integration's rules, links, and mirrored sources from syntwin. Use it only on explicit request.

## MCP servers for rooms

These let a room's twin call another service, such as a product search, while it talks to visitors.

1. Read `list_mcp_servers` and `list_room_tools` before adding anything.
2. `add_mcp_server` connects to a server the owner names and stores its tool list. No room uses it yet.
3. Show which tools a room would get, then `update_room_tools`. Tools that aren't marked read-only must be confirmed with `update_mcp_server` first; confirm only with the owner's approval.
4. After `refresh_mcp_server`, changed tools stop working until accepted with `update_mcp_server`. Show the differences before accepting.
5. Remove servers with `delete_mcp_server` only on explicit request.
