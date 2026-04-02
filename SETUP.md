# Setup Guide

## Claude Cowork (recommended)

Cowork gives the full experience: one-click plugin install, skill slash commands in the `/` menu, and automatic MCP loading — no terminal required.

Download the latest `regen-network.plugin` from [Releases](https://github.com/MontyBryant/regen-network-cowork-plugin/releases) and open it in Claude Desktop, or install directly from a session:

```
/plugin install https://github.com/MontyBryant/regen-network-cowork-plugin
```

Once installed, type `/regen` to see all available skills.

---

## Claude Code CLI

Best for developers working in the terminal. If you're working inside a repo that already has `.mcp.json` configured (e.g. [bridging-worlds](https://github.com/MontyBryant/bridging-worlds)), the MCPs load automatically — no further setup needed.

To install globally across all projects:

```bash
# KOI — knowledge commons
claude mcp add regen-koi -- npx -y regen-koi-mcp@latest \
  --env KOI_API_ENDPOINT=https://regen.gaiaai.xyz/api/koi \
  --env JENA_ENDPOINT=https://regen.gaiaai.xyz/api/koi/fuseki/koi/sparql

# Ledger — blockchain data
claude mcp add regen-network -- uvx regen-python-mcp

# Compute — footprint and credit retirement (recommended: install at user level)
claude mcp add -s user regen-compute -- npx regen-compute
```

Or install KOI and Ledger via the upstream plugin marketplace:
```bash
/plugin marketplace add https://github.com/regen-network/regen-ai-claude
/plugin install koi@regen-ai
/plugin install ledger@regen-ai
```

Skills are not available as slash commands in Claude Code — use natural language instead ("search KOI for bioregional finance", "show me active governance proposals").

---

## Cursor

Add to `.cursor/mcp.json` in your project root, or `~/.cursor/mcp.json` for global config:

```json
{
  "mcpServers": {
    "regen-koi": {
      "command": "npx",
      "args": ["-y", "regen-koi-mcp@latest"],
      "env": {
        "KOI_API_ENDPOINT": "https://regen.gaiaai.xyz/api/koi",
        "JENA_ENDPOINT": "https://regen.gaiaai.xyz/api/koi/fuseki/koi/sparql"
      }
    },
    "regen-network": {
      "command": "uvx",
      "args": ["regen-python-mcp"],
      "env": {
        "REGEN_MCP_LOG_LEVEL": "INFO"
      }
    },
    "regen-compute": {
      "command": "npx",
      "args": ["regen-compute"]
    },
    "registry-review": {
      "command": "uvx",
      "args": ["registry-review-mcp"],
      "env": {
        "REGISTRY_REVIEW_LLM_EXTRACTION_ENABLED": "true"
      }
    }
  }
}
```

If you're cloning a repo that already includes `.cursor/mcp.json` with this config, no further action is needed.

---

## VS Code

**GitHub Copilot Chat** — add to your VS Code `settings.json`:

```json
{
  "github.copilot.chat.mcp.servers": {
    "regen-koi": {
      "command": "npx",
      "args": ["-y", "regen-koi-mcp@latest"],
      "env": {
        "KOI_API_ENDPOINT": "https://regen.gaiaai.xyz/api/koi",
        "JENA_ENDPOINT": "https://regen.gaiaai.xyz/api/koi/fuseki/koi/sparql"
      }
    },
    "regen-compute": {
      "command": "npx",
      "args": ["regen-compute"]
    }
  }
}
```

**[Continue.dev](https://continue.dev)** — add servers to `~/.continue/config.json` under `"mcpServers"` using the same format as Cursor above.

---

## Other MCP-compatible clients

Any client supporting the Model Context Protocol (Windsurf, Warp, JetBrains AI, Gemini CLI, Sourcegraph Cody, Amp) can use these MCPs. All servers use standard stdio transport. Use the same `command`/`args`/`env` configuration shown in the Cursor section above, and place it wherever that client stores its MCP config.

See each client's MCP documentation for the exact config file location.
