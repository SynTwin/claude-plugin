# SynTwin plugin for Claude Code and Cowork

Manage your [SynTwin](https://syntwin.ai) digital twin from Claude Code and Cowork. The plugin connects Claude to SynTwin's hosted MCP server and adds focused workflows for setup, conversation handoff, contact outreach, and health checks.

The plugin contains no local server, executable, hook, or credential. MCP access uses SynTwin OAuth and stays within the workspace selected during sign-in.

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

Open Customize, then Plugins. Add `https://github.com/SynTwin/claude-plugin` as a marketplace and install SynTwin.

See [SETUP.md](SETUP.md) for sign-in and workspace selection.

## Workflows

Claude can select these skills automatically, or you can invoke them directly:

| Skill                               | Purpose                                                 | May write to SynTwin |
| ----------------------------------- | ------------------------------------------------------- | -------------------- |
| `/syntwin:set-up-twin`              | Build or improve your twin from available context       | Yes                  |
| `/syntwin:remember-conversation`    | Save durable conversation context                       | Yes                  |
| `/syntwin:prepare-contact-outreach` | Create personalized twin links without sending messages | Yes                  |
| `/syntwin:check-twin-health`        | Find stale or missing twin context                      | Only with approval   |

Claude inspects existing records before writing and relies on SynTwin's tool approval annotations for consequential changes.

## Data and permissions

The MCP connection can read and manage your SynTwin profile, knowledge, people, rooms, and conversations. Authentication, billing, and organization administration remain in the SynTwin application. See SynTwin's [privacy policy](https://syntwin.ai/legal/privacy/) and [legal notice](https://syntwin.ai/legal/legal-notice/).

## Updates and source

This public repository is a generated mirror. The canonical plugin package lives with the SynTwin application and is published through a one-way workflow. Do not edit the mirror directly; the next release restores the canonical files while retaining the mirror's commit history.

Maintainers bump the version in both plugin manifests for every package change so installed copies receive the update.

## Support

Email [info@syntwin.ai](mailto:info@syntwin.ai). Report security problems as described in [SECURITY.md](SECURITY.md).
