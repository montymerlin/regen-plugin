# Changelog — regen-network

A narrative record of how this plugin evolves. Updated after significant work sessions, not per-commit.

---

## 2026-04-22 — v0.2.0: Host-agnostic package positioning

Repositioned the repo as a host-agnostic MCP + skills package instead of a Claude-first plugin, while keeping Cowork called out as the smoothest UX where that remains true. `AGENTS.md` is now the canonical repo instruction file, `CLAUDE.md` is a thin compatibility wrapper, and README/SETUP were rewritten to describe the host differences as setup and experience differences rather than product identity.

This keeps the plugin aligned with the newer compatibility-layer pattern across the workspace without changing the underlying MCP or skill behavior. See Decision 003.

## 2026-04-21 — v0.1.1: Dual-distribution packaging

Added `marketplace.json` for Claude Code CLI installation via `claude plugins install`. Added Distribution section to CLAUDE.md covering Claude Code, Cowork (recommended), and other hosts. Cowork remains the recommended host for its slash command UX, but Claude Code CLI is now a first-class install path. See Decision 002.

---

## 2026-04-21 — v0.1.0: adopted agentic scaffold

Added CLAUDE.md, CHANGELOG.md, DECISIONS.md, and ROADMAP.md to bring the plugin up to agentic scaffold conventions. The plugin already had a solid README and SETUP.md; the scaffold adds agent instructions, version tracking, decision logging, and a place for future directions. See Decision 001.

---

## Pre-scaffold history

The plugin was developed incrementally across several sessions. Key milestones reconstructed from git history:

- **Namespace prefix pass** — all skills renamed with `regen:` prefix for Cowork disambiguation
- **README and SETUP rewrite** — positioned Cowork as the recommended install path, added per-environment guidance in SETUP.md
- **Initial release (v0.1.0)** — nine skills bundling four MCP servers (KOI, Ledger, Compute, Registry Review) into a single plugin for the Regen Network ecosystem
