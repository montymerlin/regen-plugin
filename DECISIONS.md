# Decisions — regen-network

Architectural decisions for the regen-network plugin, in lightweight ADR format.

---

## Decision 001 — Adopted agentic scaffold (2026-04-21)

**Context:** The plugin had a README and SETUP.md but no agent instruction set (CLAUDE.md), no decision log, no roadmap, and no changelog. As the plugin grows (registry-review skill planned for v0.2.0, potential new MCP integrations), conventions need to be explicit so future sessions don't re-derive them.

**Decision:** Add the standard agentic scaffold (CLAUDE.md, CHANGELOG.md, DECISIONS.md, ROADMAP.md) alongside the existing README.md and SETUP.md.

**Consequences:**
- Agent sessions have explicit conventions for skill authoring, MCP dependencies, and versioning
- Structural decisions have a home rather than being buried in commit messages
- Future directions (registry-review, additional skills) are tracked in ROADMAP.md
- Adds four small files; each serves a distinct purpose

**Alternatives Considered:**
- *Leave as-is* — sufficient at v0.1.0 scale but increasingly fragile as the plugin grows
- *Embed everything in README* — conflates human-facing and agent-facing documentation

---

## Decision 002 — Dual-distribution packaging with marketplace.json (2026-04-21)

**Context:** The plugin README mentioned Cowork as "recommended" and other hosts as alternatives, but had no marketplace.json for Claude Code CLI installation. As the broader plugin portfolio standardised on dual-distribution, regen-plugin needed the same treatment.

**Decision:** Add `.claude-plugin/marketplace.json`, add a Distribution section to CLAUDE.md, and update the directory structure listing.

**Consequences:**
- Users can install via `claude plugins install github.com/regen-network/regen-ai-claude`
- Cowork remains the recommended host (slash command UX), but Claude Code CLI is now a first-class install path
- marketplace.json version must stay in sync with plugin.json on each release

**Alternatives Considered:**
- *Cowork only* — rejected. Claude Code CLI users working with Regen data should be able to install the plugin normally
- *Add COMPATIBILITY.md* — deferred. SETUP.md already covers per-environment configuration; the Distribution section in CLAUDE.md is sufficient for now

---

## Decision 003 — Reframe regen-network as a host-agnostic package (2026-04-22)

**Context:** The plugin had already been distributed to multiple hosts, but the repo still read as Claude-first and Cowork-led in a way that overstated the host identity of the package itself. The underlying product is really a bundle of MCP server definitions, markdown skills, and setup guidance that can be used anywhere MCP works. Cowork may still be the smoothest UX, but it should be described as a recommended host experience, not as the product’s defining context.

**Decision:** Reframe the repo as a host-agnostic MCP + skills package. Add canonical `AGENTS.md`, turn `CLAUDE.md` into a thin compatibility wrapper, and rewrite README/SETUP so host differences are presented as setup and UX differences rather than product identity. Keep Cowork recommendations where they reflect real UX advantages.

**Consequences:**
- The repo now matches the compatibility-layer pattern used elsewhere in the workspace
- Documentation is clearer for non-Claude MCP hosts without losing the Cowork recommendation
- The underlying MCP and skill behavior remains unchanged
- Future host-specific notes can live in setup docs without redefining the package itself

**Alternatives Considered:**
- *Leave the Claude-first framing in place* — rejected. It misdescribes what the package actually is
- *Remove all host recommendations entirely* — rejected. Cowork still offers a materially better slash-command UX for some users
