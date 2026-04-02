# Regen Network Plugin

Access the Regen Network ecosystem from Claude — knowledge commons, blockchain ledger, ecological credit markets, and AI compute footprinting.

## What's included

### MCP Servers (4)

| Server | Package | What it connects to |
|--------|---------|-------------------|
| `regen-koi` | `regen-koi-mcp` (npm) | Knowledge commons — 86,000+ docs across forum, Notion, GitHub, podcasts, YouTube |
| `regen-network` | `regen-python-mcp` (PyPI) | Regen Ledger blockchain — ecocredits, governance, marketplace, staking |
| `regen-compute` | `regen-compute` (npm) | AI footprint estimation and ecological credit retirement |
| `registry-review` | `registry-review-mcp` (PyPI) | Carbon credit project document review (planned skill — MCP included) |

### Skills (9)

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

Skill slash commands are available in **Claude Cowork** (recommended). In other environments (Claude Code CLI, Cursor, VS Code), the same capabilities are available via natural language — just describe what you want and Claude will use the underlying MCP tools.

## Prerequisites

- **Node.js 18+** — required for KOI and Compute MCPs (`node --version` to check)
- **`uv` Python package manager** — required for Ledger and Registry Review MCPs
  ```bash
  curl -LsSf https://astral.sh/uv/install.sh | sh
  ```

All MCP servers download automatically on first use — no manual build or install step.

## Setup

**Recommended: Claude Cowork** — install the `.plugin` file for one-click setup with full skill slash command support.

For all other environments (Claude Code CLI, Cursor, VS Code, and other MCP-compatible clients), see **[SETUP.md](SETUP.md)**.

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
