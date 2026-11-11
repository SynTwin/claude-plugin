# syntwin plugin for Claude Code and Cowork

Manage your [syntwin](https://syntwin.ai) digital twin from Claude Code and Cowork. The plugin connects Claude to syntwin's hosted MCP server and adds skills for setup, your 1:1 with your twin, conversation review, knowledge, outreach links, Grow, automations, connected tools, and health checks.

The plugin contains no local server, executable, hook, or credential. MCP access uses syntwin OAuth and stays within the workspace selected during sign-in.

## Install

### Claude Code

Add the public repository as a marketplace, then install the plugin:

```text
/plugin marketplace add SynTwin/claude-plugin
/plugin install syntwin@syntwin
```

For local development, run from this package directory:

```shell
claude --plugin-dir .
```

### Cowork

Open Customize, then Plugins. Add `https://github.com/SynTwin/claude-plugin` as a marketplace and install syntwin.

See [SETUP.md](SETUP.md) for sign-in and workspace selection.

## Start with your agent

Run `/syntwin:set-up-twin`, or ask: "Help me set up syntwin with you as my agent. Verify the connection, review what my twin should know, and agree how we hand over work and keep it up to date."

The skill checks the workspace before writing, proposes knowledge and room changes, and reads saved changes back. New portrait and voice uploads currently happen in the app. The agent can check existing media and select it once ready. See [SETUP.md](SETUP.md#set-up-with-your-agent) for the full prompt and the routine for future updates.

## Workflows

Claude can select these skills automatically, or you can invoke them directly:

| Skill                               | Use it to                                                                        | May write to syntwin                                     |
| ----------------------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------------- |
| `/syntwin:set-up-twin`              | Build or improve your twin from context the assistant can reach                  | Yes, after you review                                    |
| `/syntwin:talk-to-twin`             | Talk to your twin in your 1:1                                                    | Only what you confirm                                    |
| `/syntwin:review-conversations`     | Summarize visitor conversations, close knowledge gaps, give feedback, try a room | Yes, after you approve                                   |
| `/syntwin:remember-conversation`    | Save facts and decisions from a conversation or meeting                          | Yes, after you review                                    |
| `/syntwin:prepare-contact-outreach` | Create personal room links and drafts without sending them                       | Yes                                                      |
| `/syntwin:find-lookalike-prospects` | Find similar companies and people with Grow                                      | Yes, and runs on your provider account after you confirm |
| `/syntwin:set-up-automation`        | Send finished conversations to your own webhook                                  | Yes, after you review                                    |
| `/syntwin:manage-connected-tools`   | Check HubSpot, Google Drive, DocuWare, and room MCP tools                        | Only with approval                                       |
| `/syntwin:check-twin-health`        | Find stale, missing, or duplicate context                                        | Only with approval                                       |

Claude resolves existing records before writing and never sends messages or outreach unless you approve the exact message. syntwin's tool annotations mark consequential changes for approval.

The same skills ship in syntwin's plugin for ChatGPT and Codex.

## Data and permissions

The plugin connects to one remote MCP server, `https://backend.syntwin.ai/api/mcp`, and sends it only the tool calls Claude makes. The connection can read and manage your syntwin profile, knowledge, people, rooms, conversations, automations, integrations, and workspace data. Tools that delete, overwrite, send, share, or connect something are marked so Claude asks before running them. Sign-in, plans, and payment stay in the syntwin app. See syntwin's [privacy policy](https://syntwin.ai/legal/privacy/), [terms](https://syntwin.ai/legal/terms/), and [legal notice](https://syntwin.ai/legal/legal-notice/).

## Updates and source

This public repository is a generated mirror. The canonical plugin package lives with the syntwin application and is published through a one-way workflow. Do not edit the mirror directly; the next release restores the canonical files while retaining the mirror's commit history.

The skills in this repository are the single copy of syntwin's assistant skills; the ChatGPT and Codex plugin is built from them. Maintainers bump the version in both plugin manifests for every package change so installed copies receive the update.

## Support

Email [info@syntwin.ai](mailto:info@syntwin.ai). Report security problems as described in [SECURITY.md](SECURITY.md).
