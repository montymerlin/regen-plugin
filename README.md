# Regen Network Plugin

Access the Regen Network ecosystem through a host-agnostic MCP + skills package: knowledge commons, blockchain ledger, ecological credit markets, and AI compute footprinting.

## What's Included

### MCP servers

| Server | Package | What it connects to |
|--------|---------|-------------------|
| `regen-koi` | `regen-koi-mcp` | Knowledge commons |
| `regen-network` | `regen-python-mcp` | Regen Ledger blockchain |
| `regen-compute` | `regen-compute` | AI footprint estimation and ecological credit retirement |
| `registry-review` | `registry-review-mcp` | Carbon credit project document review |

### Skills

| Skill | Purpose |
|-------|---------|
| `/regen-search` | Search the knowledge commons |
| `/regen-digest` | Summarize what’s new |
| `/regen-credits` | Explore ecocredit classes and batches |
| `/regen-market` | Inspect sell orders and market activity |
| `/regen-governance` | Track proposals and governance actions |
| `/regen-portfolio` | Analyze wallet impact and holdings |
| `/regen-footprint` | Estimate and potentially offset AI footprint |
| `/regen-code` | Explore architecture and code relationships |
| `/regen-review` | Registry review workflow |

## Host Positioning

This package is host-agnostic: any environment that supports MCP and markdown skills can use it.

- **Cowork** remains the smoothest end-user experience because slash commands and automatic MCP loading are especially friendly there.
- **Claude Code, Cursor, VS Code, and other MCP-capable hosts** use the same underlying MCPs and skills, typically through natural-language invocation and host-specific MCP configuration.

## Setup

See [SETUP.md](SETUP.md) for per-host installation and MCP configuration.

## Optional Environment Variables

| Variable | Purpose |
|----------|---------|
| `REGEN_WALLET_MNEMONIC` | Enable direct on-chain credit retirement |
| `ECOBRIDGE_EVM_MNEMONIC` | Enable cross-chain token payments via ecoBridge |
| `REGISTRY_REVIEW_ANTHROPIC_API_KEY` | Enable LLM-powered document extraction |

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
