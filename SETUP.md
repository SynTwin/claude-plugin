# Set up syntwin

The plugin connects to syntwin's hosted MCP server using OAuth. It ships no credentials, API keys, or authorization headers.

## Before you start

- Create or join a workspace at [my.syntwin.ai](https://my.syntwin.ai).
- Install the plugin as described in [README.md](README.md#install).

## Claude Code

1. Run `/mcp`.
2. Select the `syntwin` server and authenticate.
3. Complete syntwin sign-in in the browser.
4. Select the workspace you intend to manage when prompted.

## Cowork

1. Install syntwin under Customize, then Plugins.
2. Complete the syntwin sign-in shown during installation.
3. Select the workspace you intend to manage when prompted.

## Verify

Ask Claude to read your syntwin workspace, owner profile, and rooms. Confirm the result belongs to the expected workspace before asking Claude to change anything. A successful read proves read access; the first setup change you approve should also be read back to verify write access.

The OAuth connection binds to one workspace on first use. Claude cannot switch that binding from a prompt or infer another workspace from your current folder.

## Change workspace or reconnect

- Disconnect syntwin from Claude or Cowork, then create a new connection and select the intended workspace.
- Clearing local authentication alone does not change an existing connection's workspace binding.

If a credential was revoked in syntwin, create a new connection. A revoked credential cannot be rebound to another workspace.

## Set up with your agent

Run `/syntwin:set-up-twin` in Claude Code, or send this prompt in Claude, ChatGPT, Codex, or another connected agent:

> Help me set up and maintain my syntwin with you as my agent. First verify which workspace and owner you are connected to. Inspect my existing twin, knowledge, people, and rooms. Use the context I choose to share, propose what to teach my twin and how its room should behave, and let me review changes before saving. Help me set up its talking head and cloned voice, telling me which steps need me in the syntwin app. Agree how we hand conversations between you and my twin and keep its knowledge current after meetings, decisions, and visitor questions. Read back what you save and tell me what still needs my attention. Do not start background monitoring or recurring work without my request.

The setup skill checks existing records, separates facts from behavioral instructions, and reviews who may use each source. It also helps your agent work through a person or conversation handed over from the app and save the updates you approve afterward.

New portrait and voice uploads currently happen in the app, where you personally accept the cloning declaration. MCP does not yet return the consent ID your agent would need to finish a new upload. Your agent can check existing media and its processing and release status, then select ready media for your twin and rooms. A created media record does not mean processing has finished.

## Keep your twin current

After a meeting or a decision, try:

> Review what changed in this conversation and what my syntwin should learn. Check existing sources and people, show me the proposed updates, and save only what I approve. Tell me which questions or contradictions still need an answer.

For work arriving from syntwin, choose **Hand over** on a person or conversation in the app and send its prepared prompt. Your agent reads the current records through MCP. After you decide the next action, ask it to save what the twin should know. Hand-off IDs expire after 30 minutes; create a fresh one if needed.

Agree when you want to do this. A connection or setup session alone does not create a background sync, a scheduled task, or permission to read your other chats or accounts.
