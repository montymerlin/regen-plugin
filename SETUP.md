# Setup Guide

This package works anywhere MCP is supported. The host changes the setup flow and the UX, not the underlying capabilities.

---

## Quick Install (for AI agents)

If you're an AI agent installing this plugin, **detect the host first**, then pick the matching path. Only ask the user the questions explicitly listed below.

```
1. Detect host:
   ├── Cowork (Claude Desktop, sandboxed)
   │     → Path A: Cowork upload (no questions needed)
   │     → MCPs auto-load. Skills appear under /regen-*.
   │
   ├── Claude Code CLI (terminal, has `claude` command)
   │     → Path B1: claude plugins install (gets MCPs + skills + slash commands)
   │     → ASK USER: "Install globally (every project) or locally (this project)?"
   │       • global → `claude plugins install github.com/regen-network/regen-ai-claude`
   │       • local  → symlink the cloned repo into `./.claude/plugins/`
   │     → Path B2 (alternative): `claude mcp add` for MCPs only — use if user only wants MCPs without skills
   │
   ├── Cursor (has Cursor MCP config)
   │     → Path C: Add MCP servers to .cursor/mcp.json (no skills loaded)
   │     → MCPs only; skills don't load in Cursor
   │
   ├── VS Code (Continue.dev or Copilot Chat)
   │     → Path D: Add MCP servers to host's MCP config (no skills loaded)
   │
   └── Other MCP-compatible host
         → Path E: Reuse the .mcp.json server definitions in the host's MCP config location
```

**Host detection signals:**
- `$CLAUDE_COWORK == "1"` or `mount | grep virtiofs` matches → Cowork
- `command -v claude` succeeds and not Cowork → Claude Code CLI
- `~/.cursor/` exists → Cursor
- `~/.continue/` exists → Continue.dev (VS Code)

**Pre-install checks (always):**
- All MCP servers reach external endpoints (KOI, Regen Ledger gRPC, Regen Compute) — verify the host has outbound HTTPS to `regen.gaiaai.xyz` and `regen.network` before troubleshooting tool failures.
- `npx` and `uvx` must be on PATH for the MCPs to start.

---

## Compatibility Matrix

| Host | Status | Notes |
|------|--------|-------|
| **Cowork** | ✓ | One-click plugin upload, skills in `/` menu, auto MCP loading |
| **Claude Code CLI** | ✓ | `claude plugins install` or symlink, natural-language invocation |
| **Cursor / VS Code** | ✓ MCP only | MCPs work; skills don't load (requires Continue/Copilot Chat) |
| **Codex (OpenAI)** | partial | Use global install script (not applicable here) |
| **Agent SDK** | ✓ | Standard plugin loader via `.mcp.json` |
| **Anthropic API direct** | partial | Manual tool wiring from MCP definitions |

---

## Cowork

Cowork provides the smoothest user experience: one-click plugin install, skill slash commands in the `/` menu, and automatic MCP loading.

You need a `.plugin` zip to upload. Get one of these three ways:

**Option 1 — pre-built release (preferred when available):**
Download `regen-network-<version>.plugin` from the GitHub Releases page of `regen-network/regen-ai-claude`.

**Option 2 — built locally from this repo:**

```bash
git clone https://github.com/regen-network/regen-ai-claude.git
cd regen-ai-claude
zip -r /tmp/regen-network-0.2.0.plugin . \
  -x "*.DS_Store" "*/__pycache__/*" "*.pyc" ".git/*" "node_modules/*" "*.log" "_dist/*"
```

The output `/tmp/regen-network-0.2.0.plugin` is your upload artifact.

**Option 3 — built by a packager skill** (e.g. the workspace `cowork-plugin-packager`):
The packaged file lives at `<workspace>/ops/plugins/_dist/regen-network-0.2.0.plugin` after the skill runs.

Then upload:

1. Open Claude Desktop → **Cowork** → **Plugins** in the sidebar.
2. Click **+ Add plugin** → **Upload a file** → select the `.plugin`.
3. Confirm install. Skills appear under `/regen-*` commands; MCPs auto-load.

**Pre-upload verification (recommended):**

```bash
# Confirm the manifest is at the zip root
unzip -l /tmp/regen-network-0.2.0.plugin | head -20

# Confirm size is under 50 MB (this plugin packages to ~28 KB — pure metadata, MCPs install via npx/uvx at runtime)
du -h /tmp/regen-network-0.2.0.plugin
```

**What gets installed:** Ten skills (`/regen-search`, `/regen-digest`, `/regen-credits`, `/regen-market`, `/regen-governance`, `/regen-portfolio`, `/regen-footprint`, `/regen-code`, `/regen-review`, `/regen-update`) + four MCP servers (regen-koi, regen-network, regen-compute, registry-review).

---

## Claude Code CLI

Best for developers working in the terminal. Two install scopes — **ask the user which one fits**:

- **Global** (every project on this machine): `claude plugins install github.com/regen-network/regen-ai-claude` — installs MCPs **and** skills (slash commands available)
- **Local** (this project only, lives in `./.claude/plugins/`): symlink the cloned repo

**Recommended path — `claude plugins install`** (gets MCPs, skills, and slash commands together):

```bash
# Global install
claude plugins install github.com/regen-network/regen-ai-claude

# Or local (project-scoped)
git clone https://github.com/regen-network/regen-ai-claude.git ~/src/regen-network-plugin
mkdir -p ./.claude/plugins
ln -s ~/src/regen-network-plugin ./.claude/plugins/regen-network
```

If you're working inside a repo that already has `.mcp.json` configured, the MCPs load automatically.

**Alternative — MCP-only install** (use if user wants ledger/KOI access without skills):

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

**What gets installed via `claude plugins install`:** Ten skills (invoked via slash commands or natural language) + four MCP servers (regen-koi, regen-network, regen-compute, registry-review).
**What gets installed via `claude mcp add`:** Four MCP servers only. Skills are not loaded.

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

**What gets installed:** Four MCP servers (regen-koi, regen-network, regen-compute, registry-review). MCPs expose tools available in Composer; skills don't load in Cursor.

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

**What gets installed:** Two MCP servers (regen-koi, regen-compute). Tools available in Copilot Chat; skills don't load.

**Continue.dev** — add servers to `~/.continue/config.json` under `"mcpServers"` using the same format as Cursor.

**What gets installed:** Four MCP servers (regen-koi, regen-network, regen-compute, registry-review). MCPs expose tools available in Continue; skills don't load.

---

## Other MCP-Compatible Hosts

Any host supporting MCP stdio servers can use this package. Reuse the same `command` / `args` / `env` definitions shown above and place them wherever that host stores MCP config.

**What gets installed:** Four MCP servers (regen-koi, regen-network, regen-compute, registry-review). Skills availability depends on host skill loader support.

---

## Known Quirks

- **Cowork 50 MB file size limit:** The packaged `.plugin` file must stay under 50 MB. Current size is ~28 KB — the plugin is pure manifest + skills + MCP definitions; the actual MCP server code installs via `npx`/`uvx` at runtime.
- **Cowork `.plugin` vs `.zip` extension:** Some Claude Desktop builds reject `.plugin` at the upload dialog despite accepting it in the docs. Workaround: rename the file to `.zip` (contents are identical). Tracked in [anthropics/claude-code#28337](https://github.com/anthropics/claude-code/issues/28337) and [#40414](https://github.com/anthropics/claude-code/issues/40414).
- **MCP server reachability:** The four MCP servers require outbound HTTPS to:
  - `https://regen.gaiaai.xyz/api/koi` (KOI semantic index and Jena SPARQL)
  - `https://grpc.regen.network` (Regen Ledger gRPC endpoint)
  - `https://compute.regen.network` (Regen Compute API)
  
  If your network blocks these endpoints, MCP tools will time out or fail gracefully. Verify network access before troubleshooting tool failures.
- **Plugin manifest at top level:** When packaging the plugin, `.claude-plugin/plugin.json` must reside at the top level of the `.plugin` zip, not nested inside an extra parent directory. Mis-nesting breaks plugin detection. The `zip` command in the Cowork section above runs from inside the plugin dir specifically to avoid this.
- **`.mcp.json` uses `--upgrade` and `@latest`:** Each MCP cold start re-checks for newer package versions. This keeps MCPs current with the fast-moving Regen ecosystem but adds a few seconds of latency on first invocation per session.

---
