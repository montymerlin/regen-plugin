---
name: regen-digest
description: >
  Generate a weekly activity digest for Regen Network. Use when the user asks
  for a "regen digest", "what's new at regen", "regen weekly summary",
  "regen activity update", "regen network news", or wants a summary of
  recent activity across the Regen ecosystem.
metadata:
  version: "0.1.0"
---

Generate a digest of recent Regen Network activity using the KOI knowledge base.

## Workflow

1. Call `mcp__regen-koi__generate_weekly_digest`. Accepts optional parameters:
   - Date range for custom periods (defaults to last 7 days)
   - Format preferences

2. Supplement with a `mcp__regen-koi__search` query sorted by `date_desc` to catch any recent high-activity topics the digest may have summarised too briefly.

3. If the user asks about a specific area (governance, development, community), run a targeted search filtered by source:
   - Governance activity → `discourse` source
   - Development → `github` source
   - Community/strategy → `notion` source
   - Media → `youtube` or `podcast` source

## Presenting the digest

- Lead with the most significant developments
- Group by theme (governance, development, ecosystem, community) rather than by source
- Include links to original sources so the user can read further
- Note the date range covered
- If activity was low in a period, say so rather than padding
