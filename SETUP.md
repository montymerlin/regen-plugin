# Setup Guide

This package works anywhere MCP is supported. The host changes the setup flow and the UX, not the underlying capabilities.

## Cowork

Cowork provides the smoothest user experience: one-click plugin install, skill slash commands in the `/` menu, and automatic MCP loading.

Download the latest `regen-network.plugin` from the release flow you use internally, or install via the plugin marketplace when available.

Once installed, type `/regen` to see all available skills.

---

## Claude Code CLI

Best for developers working in the terminal. If you're working inside a repo that already has `.mcp.json` configured, the MCPs load automatically.

To install globally across all projects:

```bash
# KOI — knowledge commons
claude mcp add regen-koi -- npx -y regen-koi-mcp@latest \
  --env KOI_API_ENDPOINT=https://regen.gaiaai.xyz/api/koi \
  --env JENA_ENDPOINT=https://regen.gaiaai.xyz/api/koi/fuseki/koi/sparql

# Ledger — blockchain data
claude mcp add regen-network -- uvx regen-python-mcp

# Compute — footprint and credit retirement
claude mcp add -s user regen-compute -- npx regen-compute
```

Or install supported components through the upstream plugin marketplace when appropriate.

Skills are typically invoked through natural language rather than slash commands in Claude Code.

---

## Cursor

Add the MCP servers to `.cursor/mcp.json` in your project root, or `~/.cursor/mcp.json` for global config:

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

If your repo already includes this config, no further action is needed.

---

## VS Code

**GitHub Copilot Chat** — add MCP servers to your `settings.json`:

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

**Continue.dev** — add servers to `~/.continue/config.json` under `"mcpServers"` using the same format as Cursor.

---

## Other MCP-Compatible Hosts

Any host supporting MCP stdio servers can use this package. Reuse the same `command` / `args` / `env` definitions shown above and place them wherever that host stores MCP config.
