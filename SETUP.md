# Set up SynTwin

The plugin connects to SynTwin's hosted MCP server using OAuth. It ships no credentials, API keys, or authorization headers.

## Before you start

- Create or join a workspace at [my.syntwin.ai](https://my.syntwin.ai).
- Install the plugin as described in [README.md](README.md#install).

## Claude Code

1. Run `/mcp`.
2. Select the `syntwin` server and authenticate.
3. Complete SynTwin sign-in in the browser.
4. Select the workspace you intend to manage when prompted.

## Cowork

1. Install SynTwin under Customize, then Plugins.
2. Complete the SynTwin sign-in shown during installation.
3. Select the workspace you intend to manage when prompted.

## Verify

Ask Claude to list your SynTwin rooms. Confirm the result belongs to the expected workspace before asking Claude to change anything.

The OAuth connection binds to one workspace on first use. Claude cannot switch that binding from a prompt or infer another workspace from your current folder.

## Change workspace or reconnect

- Disconnect SynTwin from Claude or Cowork, then create a new connection and select the intended workspace.
- Clearing local authentication alone does not change an existing connection's workspace binding.

If a credential was revoked in SynTwin, create a new connection. A revoked credential cannot be rebound to another workspace.
