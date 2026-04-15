---
name: regen:review
description: >
  Review carbon credit project documents for registry compliance. Use when
  the user asks for a "registry review", "review project documents",
  "document review", "credit registration review", "compliance check",
  or wants to verify project documentation against methodology requirements.
  NOTE: This skill requires the Registry Review MCP which needs an Anthropic
  API key configured. See README for setup.
metadata:
  version: "0.1.0"
  status: "planned"
---

> **Status: Planned — not yet implemented in v0.1.0**
>
> This skill will wrap the Registry Review MCP's 8-stage document review workflow
> for carbon credit project registration. It is documented here for completeness
> and will be implemented in a future version.

## What it will do

Guide the full registry review process:

1. **Initialize** — create a review session, load the appropriate methodology checklist
2. **Document discovery** — scan and classify project documents (PDFs, shapefiles, spreadsheets)
3. **Requirement mapping** — match documents to checklist requirements using semantic matching
4. **Evidence extraction** — extract key data points with page-level citations
5. **Cross-validation** — check consistency across documents (dates, land tenure, project IDs)
6. **Report generation** — produce structured review report in Markdown and JSON
7. **Human review** — present flagged items for expert assessment
8. **Completion** — finalise and archive

## Prerequisites (for future implementation)

- **Registry Review MCP**: `uvx registry-review-mcp` (already included in this plugin's `.mcp.json`)
- **Anthropic API key**: set `REGISTRY_REVIEW_ANTHROPIC_API_KEY` in your environment
- **LLM extraction**: enabled via `REGISTRY_REVIEW_LLM_EXTRACTION_ENABLED=true`
- Note: LLM extraction incurs Anthropic API costs during evidence extraction stages

## Source

- Package: [registry-review-mcp](https://pypi.org/project/registry-review-mcp/) (PyPI)
- GitHub: [gaiaaiagent/regen-registry-review-mcp](https://github.com/gaiaaiagent/regen-registry-review-mcp)
- Architecture: [Regen AI forum post](https://forum.regen.network/t/announcing-regen-ai/553)
