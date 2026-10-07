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

To use an API key instead of OAuth:

```bash
codex mcp add apisrouter --url https://api.apisrouter.com/mcp --bearer-token-env-var APISROUTER_API_KEY
```

## Layout

- `.agents/plugins/marketplace.json` lists the plugins in this repository.
- `plugins/apisrouter/plugin.json` is the plugin manifest.
- `plugins/apisrouter/mcp.json` points at the MCP server.
- `plugins/apisrouter/skills/apisrouter-information/SKILL.md` tells the agent
  how to search, quote, buy and read results.
