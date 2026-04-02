# Regen Network Plugin

Access the Regen Network ecosystem from Claude — knowledge commons, blockchain ledger, ecological credit markets, and AI compute footprinting.

## Recommended setup: Claude Cowork

This plugin is designed for **[Claude Cowork](https://claude.ai/download)** (Claude Desktop app). Cowork gives you the full experience:

- **One-click install** — accept the `.plugin` file and everything configures automatically
- **Skill slash commands** — type `/` to see all 9 regen skills in the command menu and trigger them directly
- **MCPs load automatically** — all four MCP servers start up when you open a session
- **No terminal required** — no CLI commands, no config file editing

### Installing in Cowork

Download [`regen-network.plugin`](https://github.com/MontyBryant/regen-network-cowork-plugin/releases) and open it in Claude Desktop, or install directly from a session:

```
/plugin install https://github.com/MontyBryant/regen-network-cowork-plugin
```

Once installed, type `/regen` to see all available skills.

---

## Other setups

Skills (slash commands) are a Cowork-specific feature. In other environments you get the full MCP tool access — Claude can use all the same underlying capabilities, but you invoke them through natural language rather than `/` commands.

### Claude Code CLI

Best for developers working in the terminal. MCPs load per-project from `.mcp.json` (already configured if you're working in the bridging-worlds repo), or install globally:

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

Or install via the upstream plugin marketplace (KOI and Ledger only):
```bash
/plugin marketplace add https://github.com/regen-network/regen-ai-claude
/plugin install koi@regen-ai
/plugin install ledger@regen-ai
```

**What you get:** Full MCP tool access. No skill slash commands — instead just ask Claude naturally ("search KOI for bioregional finance", "show me active governance proposals", "estimate my session footprint").

**Note:** The `.mcp.json` at the repo root already includes all four MCPs, so if you're working inside a repo that includes this config, no additional setup is needed.

### Cursor

Add to `.cursor/mcp.json` in your project (or `~/.cursor/mcp.json` for global config):

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

**What you get:** Full MCP tool access in Cursor's AI panel. No slash command skills — use natural language prompts instead. The `.cursor/mcp.json` in this repo already includes this config if you clone it.

### VS Code (GitHub Copilot / Continue.dev)

For GitHub Copilot Chat with MCP support, add to your VS Code `settings.json`:

```json
{
  "github.copilot.chat.mcp.servers": {
    "regen-koi": {
      "command": "npx",
      "args": ["-y", "regen-koi-mcp@latest"],
      "env": {
        "KOI_API_ENDPOINT": "https://regen.gaiaai.xyz/api/koi",
        "JENA_ENDPOINT": "https://regen.gaiaai.xyz/api/koi/fusuki/koi/sparql"
      }
    },
    "regen-compute": {
      "command": "npx",
      "args": ["regen-compute"]
    }
  }
}
```

For [Continue.dev](https://continue.dev), add servers to `~/.continue/config.json` under `"mcpServers"` using the same format as Cursor above.

**What you get:** MCP tool access within your editor AI assistant. Coverage varies by extension — Copilot Chat and Continue both support MCP, but tool availability depends on the extension version.

### Other MCP-compatible clients

Any client that supports the Model Context Protocol (Windsurf, Sourcegraph Cody, Warp, Amp, JetBrains AI, Gemini CLI) can use these MCPs. The server definitions are standard stdio transport — use the same `command`/`args`/`env` config shown above. See each client's MCP documentation for where to place the config.

---

## What's included

### MCP Servers (4)

| Server | Package | What it connects to |
|--------|---------|-------------------|
| `regen-koi` | `regen-koi-mcp` (npm) | Knowledge commons — 86,000+ docs across forum, Notion, GitHub, podcasts, YouTube |
| `regen-network` | `regen-python-mcp` (PyPI) | Regen Ledger blockchain — ecocredits, governance, marketplace, staking |
| `regen-compute` | `regen-compute` (npm) | AI footprint estimation and ecological credit retirement |
| `registry-review` | `registry-review-mcp` (PyPI) | Carbon credit project document review (planned skill — MCP included) |

### Skills (9) — Cowork only

| Skill | Trigger phrases | MCPs used |
|-------|----------------|-----------|
| `/regen-search` | "search regen", "search KOI", "find in regen commons" | KOI |
| `/regen-digest` | "regen digest", "what's new at regen" | KOI |
| `/regen-credits` | "ecocredits", "credit classes", "what credits are available" | Ledger + Compute |
| `/regen-market` | "credit prices", "sell orders", "market trends" | Ledger + Compute |
| `/regen-governance` | "proposals", "regen votes", "community pool" | Ledger |
| `/regen-portfolio` | "wallet impact", "analyze address", "portfolio analysis" | Ledger |
| `/regen-footprint` | "offset my session", "environmental cost", "retire credits" | Compute |
| `/regen-code` | "regen architecture", "regen codebase", "trace call chain" | KOI |
| `/regen-review` | "registry review", "document review" | Registry Review *(planned v0.2.0)* |

In non-Cowork environments, use these same phrases as natural language prompts — Claude will use the underlying MCP tools to fulfil the request.

## Prerequisites

- **Node.js 18+** — required for KOI and Compute MCPs (`node --version` to check)
- **`uv` Python package manager** — required for Ledger and Registry Review MCPs
  ```bash
  curl -LsSf https://astral.sh/uv/install.sh | sh
  ```

All MCP servers download automatically on first use — no separate build or install step needed.

## Optional environment variables

| Variable | Purpose |
|----------|---------|
| `REGEN_WALLET_MNEMONIC` | Enable direct on-chain credit retirement (Compute MCP) |
| `ECOBRIDGE_EVM_MNEMONIC` | Enable cross-chain token payments via ecoBridge (Compute MCP) |
| `REGISTRY_REVIEW_ANTHROPIC_API_KEY` | Required for LLM-powered document extraction (Registry Review MCP) |

## Links

- [Regen Network](https://regen.network)
- [Regen Marketplace](https://app.regen.network)
- [Regen Compute](https://compute.regen.network)
- [KOI MCP source](https://github.com/gaiaaiagent/regen-koi-mcp)
- [Ledger MCP source](https://github.com/gaiaaiagent/regen-python-mcp)
- [Compute source](https://github.com/regen-network/regen-compute)
- [Registry Review source](https://github.com/gaiaaiagent/regen-registry-review-mcp)
- [Upstream plugin marketplace](https://github.com/regen-network/regen-ai-claude)

## License

Apache-2.0
