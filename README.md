# APIsRouter plugins

Give your agent paid access to social, search and web data. The plugin includes
one usage Skill and a remote MCP connection. Your agent finds an operation,
gets a quote, uses your approved budget, and reads the result. Purchases appear
in your APIsRouter account.

## Codex desktop

Open **Plugins → Installed → APIsRouter** and select **Connect** when prompted.
If this marketplace is already registered, you can
[open APIsRouter in the desktop app](codex://plugins/install/apisrouter?marketplace=apisrouter).
If it is not registered, that link opens Plugins; it does not import this
repository automatically.

For a local marketplace, place this repository in a project opened in Codex.
The app discovers `.agents/plugins/marketplace.json`. Open Plugins, install
APIsRouter from the project marketplace, and complete its connection prompt.
Restart the desktop app after changing a local package so it reloads the files.
See [OpenAI's local marketplace instructions](https://developers.openai.com/plugins/build/plugins).

Sign in on `https://apisrouter.com` and review the allowed operations, spending
limit and expiry before approving. Start a new chat after installation. If a
tool returns `Authentication required`, open the installed plugin and reconnect.

## Claude desktop and Cowork

In **Customize → Plugins → Add → Add marketplace**, enter:

```text
https://github.com/Narcooo/apisrouter-plugins
```

Select APIsRouter and add it. Alternatively, use **Add → Upload plugin** with
a ZIP of `plugins/apisrouter`, including its hidden files. Open the plugin's
**Connectors** tab, add/connect APIsRouter, and complete OAuth. Its remote MCP
address is `https://api.apisrouter.com/mcp`.

Plugins are stored on the Claude account and available in desktop chat and
Cowork. Organization settings can restrict installation. Start a new task after
connecting. See [Claude's installation guide](https://claude.com/docs/plugins/overview).

## First task

Ask your agent:

> Find the APIsRouter operation for a TikTok account's region. Show me the
> required input and the price for one request before buying anything.

After you provide the account and approve a budget, the agent can execute the
request and report the result and final charge. Save the request reference if
you want to continue later. An expired connection requires reconnection; an
insufficient balance pauses the request until funding is resolved through the
flow the host permits. Continue with the same request to avoid buying twice.

## Grok Bot and Muse

Grok Bot installs connectors from its own Marketplace. This package has the
portable Agent Plugins manifest and MCP configuration supported by Cursor;
installation and OAuth in Grok Bot still need a host-specific acceptance run.
See [Grok Bot apps](https://docs.x.ai/grok-bot/computer-and-apps) and
[Cursor's plugin formats](https://cursor.com/docs/reference/plugins).

Muse distribution uses its [connector platform](https://muse.ai/platform/docs).
Its listing, authentication and tool permissions require a separate review.
An APIsRouter listing and a completed Muse account connection are pending.

## Distribution and acceptance

This repository is a Git marketplace. Public-directory review and approval are
separate. A directory release must comply with that host's commerce rules;
see [OpenAI's plugin guidelines](https://developers.openai.com/plugins/plugin-guidelines).

Desktop acceptance requires installation, authorization, a real result and an
account charge verified in that host. As of 2026-10-09, end-to-end desktop acceptance is in progress for
Codex, Claude / Cowork, Grok Bot and Muse. Package validation confirms file
structure; connected-account and paid-task checks are recorded separately.

## Package layout

- `.agents/plugins/marketplace.json`: Codex repository marketplace.
- `.claude-plugin/marketplace.json`: Claude repository marketplace.
- `plugins/apisrouter/plugin.json`: portable Agent Plugins identity.
- `plugins/apisrouter/.codex-plugin/plugin.json`: Codex compatibility manifest.
- `plugins/apisrouter/.claude-plugin/plugin.json`: Claude compatibility manifest.
- `plugins/apisrouter/mcp.json`: portable remote MCP configuration.
- `plugins/apisrouter/.mcp.json`: Claude's remote connector configuration.
- `plugins/apisrouter/skills/apisrouter-information/SKILL.md`: shared workflow.

Compatibility manifests keep the same identity and version. The two MCP files
use each format's HTTP transport name and the same server URL. No keys, tokens
or account-specific grants belong in this repository.
