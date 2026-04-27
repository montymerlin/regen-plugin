---
name: regen-update
description: >
  Check for updates across the regen-network plugin's upstream MCP dependencies
  and skill coverage. Use when the user asks to "check for regen updates",
  "update regen plugin", "what's new in regen MCPs", "are there new regen tools",
  or wants to know if any upstream packages have new versions or capabilities
  not yet reflected in this plugin's skills.
metadata:
  version: "0.1.0"
---

Check for upstream updates across the four MCP packages this plugin bundles,
and surface any new MCP tools not yet covered by an existing skill.

## Step 1: Check published package versions

Fetch the latest published version for each upstream package.

**npm packages** — fetch from the npm registry:

```bash
npm show regen-koi-mcp version 2>/dev/null && \
npm show regen-compute version 2>/dev/null
```

If bash is not available, fetch these URLs instead:
- `https://registry.npmjs.org/regen-koi-mcp/latest` → `.version`
- `https://registry.npmjs.org/regen-compute/latest` → `.version`

**PyPI packages** — fetch from the PyPI API:

```bash
pip index versions regen-python-mcp 2>/dev/null | head -1 && \
pip index versions registry-review-mcp 2>/dev/null | head -1
```

If bash is not available, fetch these URLs instead:
- `https://pypi.org/pypi/regen-python-mcp/json` → `.info.version`
- `https://pypi.org/pypi/registry-review-mcp/json` → `.info.version`

## Step 2: Check for new MCP tools not covered by skills

The MCP servers may have gained new tools since this plugin's skills were last updated.
Cross-reference what's available against what the skills actually use.

**List tools currently used across all skills:**

```bash
grep -rh "mcp__regen" skills/ | grep -o 'mcp__[a-z_-]*__[a-z_]*' | sort -u
```

**List tools available in the running MCP servers** (call these if the servers are connected):

- `mcp__regen-koi__*` — check which tools are available in the connected regen-koi server
- `mcp__regen-network__*` — check regen-network server tools
- `mcp__regen-compute__*` — check regen-compute server tools

Compare the two lists. Any tool available in a connected server but not referenced in any skill file is a coverage gap — flag it as a candidate for a new skill or for adding to an existing one.

## Step 3: Check upstream release notes

For each package with a newer published version than what was last noted, fetch recent release notes:

- KOI MCP releases: `https://github.com/gaiaaiagent/regen-koi-mcp/releases`
- Ledger MCP releases: `https://github.com/gaiaaiagent/regen-python-mcp/releases`
- Compute releases: `https://github.com/regen-network/regen-compute/releases`
- Registry Review releases: `https://github.com/gaiaaiagent/regen-registry-review-mcp/releases`

Summarise any new tools, changed APIs, or deprecated endpoints that affect the skills here.

## Step 4: Report

Present a concise update report:

```
## Regen Plugin Update Report

### MCP Package Versions
| Package | Latest | Status |
|---------|--------|--------|
| regen-koi-mcp (npm) | x.y.z | ✅ up to date / ⚠️ update available |
| regen-compute (npm) | x.y.z | ... |
| regen-python-mcp (PyPI) | x.y.z | ... |
| registry-review-mcp (PyPI) | x.y.z | ... |

### New Tools Not Yet Covered by Skills
[list any gaps, or "None found"]

### Notable Changes in New Versions
[summarise release notes if updates are available, or "No updates"]

### Recommended Actions
[specific next steps, or "No action needed"]
```

If everything is up to date and no coverage gaps are found, say so clearly — this is a useful outcome too.

## Notes

- The MCP servers in `.mcp.json` are configured with `@latest` (npm) and `--upgrade` (uvx), so they auto-update on each host restart. This skill is for surfacing *skill-level* gaps — new tools that the MCPs expose but no skill in this repo yet uses.
- To update the skill files themselves, pull the latest version of this plugin repo and reinstall.
