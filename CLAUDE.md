# CLAUDE.md — regen-network

Access the Regen Network ecosystem from Claude — knowledge commons, blockchain ledger, ecological credit markets, and AI compute footprinting.

## Project Identity

- **Name:** regen-network
- **Stack:** Claude Agent SDK plugin (skills + 4 MCP servers via npm/PyPI)
- **Purpose:** Bridge Claude to the Regen Network ecosystem: search the knowledge commons (KOI), query the blockchain (Ledger), estimate/retire ecological credits (Compute), and review registry documents (Registry Review)
- **Repository:** https://github.com/regen-network/regen-ai-claude
- **License:** Apache-2.0

## Directory Structure

```
regen-plugin/
├── .claude-plugin/
│   ├── plugin.json            # plugin manifest
│   └── marketplace.json       # self-hosted marketplace listing
├── .mcp.json                  # MCP server definitions (KOI, Ledger, Compute, Registry Review)
├── skills/
│   ├── regen-search/          # search the knowledge commons
│   ├── regen-digest/          # weekly/daily digest from commons
│   ├── regen-credits/         # ecocredit classes and batches
│   ├── regen-market/          # marketplace sell orders and trends
│   ├── regen-governance/      # on-chain governance proposals and votes
│   ├── regen-portfolio/       # wallet impact analysis
│   ├── regen-footprint/       # AI session footprint estimation and retirement
│   ├── regen-code/            # codebase architecture via KOI code graph
│   └── regen-review/          # registry document review (planned)
├── CLAUDE.md                  # this file — agent instruction set
├── CHANGELOG.md               # narrative change history
├── DECISIONS.md               # architectural decision log
├── ROADMAP.md                 # future directions
├── README.md                  # human-facing overview
└── SETUP.md                   # per-environment installation guide
```

## Key Conventions

### Skills
- Each skill lives in `skills/<skill-name>/SKILL.md`
- Skills are namespaced as `regen:<skill>` (e.g., `regen:regen-search`, `regen:regen-credits`)
- Skills declare which MCP servers they depend on — the skill won't work if the required MCP isn't connected
- All skills work in Cowork (recommended) and degrade to natural-language invocation in other hosts

### MCP Servers
- Definitions live in `.mcp.json` at the plugin root
- Four servers: `regen-koi` (npm), `regen-network` (PyPI/uv), `regen-compute` (npm), `registry-review` (PyPI/uv)
- Servers download automatically on first use — no manual install step
- Optional env vars (`REGEN_WALLET_MNEMONIC`, `ECOBRIDGE_EVM_MNEMONIC`, `REGISTRY_REVIEW_ANTHROPIC_API_KEY`) enable advanced features

### Versioning
- **Single source of truth:** `.claude-plugin/plugin.json` `version` field is the canonical version
- **Git tags on release:** tag every version bump (`git tag v0.1.0`)
- **Version-check before committing:** plugin.json version = latest CHANGELOG heading = commit message

### Distribution

This plugin supports two installation paths:

- **Claude Code CLI:** `claude plugins install github.com/regen-network/regen-ai-claude` (uses `marketplace.json`)
- **Claude Cowork (desktop, recommended):** Package as `.plugin` zip and drag into Cowork chat, or install from the plugin marketplace. Slash commands (`/regen-search`, etc.) work natively.
- **Other hosts (Cursor, VS Code):** Clone the repository and see SETUP.md for per-environment MCP server configuration.

Cowork is the recommended host because skill slash commands provide the best UX for non-technical users. In Claude Code CLI and other hosts, the same capabilities are available via natural language.

**Auto-update:** Claude Code pins to a commit SHA at install time; users update manually or via marketplace sync. Cowork requires re-uploading the `.plugin` file or GitHub sync if configured at the organization level.

### Documentation
- **README.md** — human-facing: what's included, setup, prerequisites
- **SETUP.md** — per-environment installation instructions
- **CLAUDE.md** (this file) — agent-facing: conventions, boundaries, structure
- **DECISIONS.md** — architectural decision log
- **ROADMAP.md** — future directions
- **CHANGELOG.md** — narrative change history

## Agent Boundaries

### Do
- Check which MCP servers are connected before invoking skills that depend on them
- Follow the skill's declared MCP dependencies — don't improvise tool calls that aren't listed
- Log decisions in DECISIONS.md before implementing structural changes
- Update CHANGELOG.md after significant work sessions
- Bump version in plugin.json for every release

### Don't
- Assume all four MCP servers are connected — probe first, degrade gracefully
- Execute on-chain transactions (credit retirement, token payments) without explicit user confirmation
- Expose wallet mnemonics or API keys in responses or logs
- Hardcode host-specific paths — use `${CLAUDE_PLUGIN_ROOT}` for intra-plugin references

## References

- [README.md](README.md) — human-facing overview
- [SETUP.md](SETUP.md) — per-environment installation
- [DECISIONS.md](DECISIONS.md) — architectural decision log
- [ROADMAP.md](ROADMAP.md) — future directions
- [CHANGELOG.md](CHANGELOG.md) — narrative change history
