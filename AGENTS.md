# AGENTS.md — regen-network

Canonical repo instructions for `regen-network-plugin`.

## Project Identity

- **Name:** regen-network
- **Type:** Host-agnostic MCP + skills package with Claude plugin packaging compatibility
- **Version:** 0.2.0
- **Stack:** Markdown skills + `.mcp.json` server definitions + setup docs
- **Repository:** https://github.com/regen-network/regen-ai-claude

## Canonical Structure

```
regen-network-plugin/
├── .claude-plugin/
│   ├── plugin.json          # Claude plugin packaging metadata
│   └── marketplace.json     # Claude marketplace listing
├── .mcp.json                # MCP server definitions
├── skills/
│   ├── regen-search/
│   ├── regen-digest/
│   ├── regen-credits/
│   ├── regen-market/
│   ├── regen-governance/
│   ├── regen-portfolio/
│   ├── regen-footprint/
│   ├── regen-code/
│   └── regen-review/
├── AGENTS.md                # Canonical repo instructions
├── CLAUDE.md                # Claude compatibility wrapper
├── CHANGELOG.md
├── DECISIONS.md
├── ROADMAP.md
├── README.md
└── SETUP.md
```

## Canonical Rules

- `AGENTS.md` is the canonical instruction file for this repo.
- `CLAUDE.md` is a thin compatibility layer that points Claude-family hosts back to `AGENTS.md`.
- `skills/` and `.mcp.json` are the product.
- `.claude-plugin/` is Claude-specific packaging metadata, not the source of truth for skills or MCP configuration.
- Keep host recommendations in docs where they reflect real UX differences, but do not position the repo itself as single-host.

## Runtime Conventions

- Probe MCP availability before invoking skills that depend on specific servers.
- Do not assume all four MCP servers are connected in every host.
- Use a portable intra-repo root variable if future skill references need filesystem lookups:
  `REGEN_NETWORK_ROOT="${REGEN_NETWORK_ROOT:-${CLAUDE_PLUGIN_ROOT:-$PWD}}"`

## Documentation Rules

- README.md is human-facing overview.
- AGENTS.md is the canonical agent-facing document.
- CLAUDE.md exists for compatibility only and should stay short.
- SETUP.md is the per-host setup guide.
- DECISIONS.md logs major structural choices before implementation.
- ROADMAP.md holds future ideas until they become decisions.
- CHANGELOG.md records narrative milestones after significant work.

## Design Principles

1. **Host-agnostic package, host-specific guidance** — the package should run anywhere MCP works, while docs can still recommend the best host UX.
2. **Skills route through MCPs** — do not improvise capabilities outside the declared MCP surface without checking availability.
3. **Progressive disclosure** — keep the repo-level instructions concise and push setup detail into `SETUP.md`.
4. **Safety around transactions** — never execute on-chain actions without explicit confirmation.
5. **Ecosystem clarity** — docs should distinguish knowledge commons, ledger, compute, and registry-review roles cleanly.

## Boundaries

### Do
- Keep MCP dependencies explicit in skill docs
- Update `.mcp.json`, README, and SETUP together when install paths change
- Preserve host recommendations only where they are operationally justified
- Log major structural choices in DECISIONS.md before implementation

### Don't
- Reintroduce `CLAUDE.md` as the canonical repo instruction file
- Hardcode Claude-specific runtime paths as if they define the package
- Assume one host is mandatory when the MCP layer is portable
- Expose mnemonics, API keys, or other sensitive credentials
