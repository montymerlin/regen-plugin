# Roadmap — regen-network

Where this plugin could go. Items here are aspirations, not commitments. When an item is evaluated, the outcome moves to the Decided section with a pointer to DECISIONS.md.

**Statuses:** `active` (in progress) · `idea` (worth evaluating) · `parked` (inspiration, no timeline) · `decided` (evaluated — see DECISIONS.md)

---

## Near-term

- **Registry Review skill (v0.2.0)** — The MCP server is already bundled in `.mcp.json` but the `regen-review` skill is a placeholder. Needs: full SKILL.md with document review workflow, integration with `REGISTRY_REVIEW_ANTHROPIC_API_KEY` env var, and testing against real registry project documents. `status: idea`

## Future explorations

- **Cross-skill workflows** — Compose skills into higher-level workflows, e.g., "analyze this credit class" could invoke regen-credits → regen-market → regen-portfolio in sequence. Currently each skill is independent. `status: idea`
- **Offline/degraded mode** — When MCP servers can't connect (network issues, missing dependencies), provide cached or static fallback data for common queries. `status: idea`
- **Notification skill** — Alert on governance proposals approaching vote deadline, or significant market movements. Would need a scheduling mechanism. `status: idea`

## Parking lot

- **Additional MCP integrations** — IBC-connected chains (Osmosis DEX data, Cosmos Hub governance), off-chain data sources (Toucan, Verra registry APIs). `status: parked`
- **Curriculum/learning skill** — Guided introduction to Regen Network concepts for newcomers, using KOI commons content. `status: parked`

## Decided

- **Adopted agentic scaffold** — → Decision 001. `status: decided`
