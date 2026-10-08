# APIsRouter plugins

Plugins and skills that connect agent hosts to the APIsRouter information
catalog: paid social, search and web data with a quote before every purchase.

## Codex

Install from this marketplace:

```bash
codex plugin marketplace add Narcooo/apisrouter-plugins
codex plugin add apisrouter@apisrouter
```

The plugin registers the `apisrouter` MCP server at
`https://api.apisrouter.com/mcp` and signs in through OAuth: the browser
opens https://apisrouter.com, you pick the spending limit the connection may
use, and you are done.

Check `codex plugin list --marketplace apisrouter --json` for both
`installed: true` and `enabled: true`. Marketplace discovery alone does not
install the plugin. Complete OAuth in the host before making a paid call.

To use an API key instead of OAuth:

```bash
codex mcp add apisrouter --url https://api.apisrouter.com/mcp --bearer-token-env-var APISROUTER_API_KEY
```

Set `APISROUTER_API_KEY` through your local secret manager or environment;
never paste its value into a prompt or commit it. The standalone MCP entry
and an installed plugin are distinct connection methods.

## Distribution status

This repository is a Git-based marketplace. Publication here is separate from
review or approval in OpenAI's public plugin directory. A public-directory
submission must meet that directory's commerce rules; transactional recharge
links and credit purchases must not be included in its user flow.

Current package scope is information APIs through MCP and a usage Skill.
The website provides account history and connection management. Host
acceptance is recorded only after installation, authorization and a real
result have all been verified for that host.

## Layout

- `.agents/plugins/marketplace.json` lists the plugins in this repository.
- `plugins/apisrouter/plugin.json` is the plugin manifest.
- `plugins/apisrouter/mcp.json` points at the MCP server.
- `plugins/apisrouter/skills/apisrouter-information/SKILL.md` tells the agent
  how to search, quote, buy and read results.
