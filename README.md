# Regen Network Plugin

Access the Regen Network ecosystem through a host-agnostic MCP + skills package: knowledge commons, blockchain ledger, ecological credit markets, and AI compute footprinting.

## What is Regen Network?

Regen Network is a public blockchain purpose-built for ecological assets. Built on the Cosmos SDK, it provides an on-chain registry for verified ecological outcomes — primarily **ecocredits**, which represent quantified, verified improvements to land, biodiversity, or climate.

Key parts of the ecosystem this plugin connects to:

- **Regen Ledger** — the blockchain itself. Stores ecocredit classes, project registrations, credit batch issuances, retirements, marketplace sell orders, staking, and on-chain governance.
- **KOI (Knowledge of Impact)** — Regen's knowledge commons. A semantic index of 86,000+ documents spanning forum posts, Notion workspace, GitHub repos, podcast transcripts, YouTube videos, and ecosystem publications. Supports hybrid vector + keyword + knowledge graph search.
- **Regen Compute** — connects AI compute usage to verified ecological credit retirement. Estimates the energy and CO2 cost of an AI session and provides a direct path to retire ecocredits as compensation.
- **Registry Review** — an LLM-assisted workflow for reviewing carbon credit project documents against methodology requirements, producing structured compliance reports.

### Ecocredit types on Regen Network

| Code | Type | What it represents |
|------|------|-------------------|
| C | Carbon | Verified carbon sequestration or emissions reduction |
| BT | Biodiversity / Terrasos | Habitat conservation and biodiversity impact |
| KSH | Kilo-Sheep-Hour | Pasture management and grazing outcomes |
| MBS | Marine Biodiversity Stewardship | Ocean ecosystem health |
| USS | Umbrella Species Stewardship | Habitat protection for keystone species |

## MCP Servers

Four MCP servers ship with this package, each connecting to a different layer of the Regen ecosystem:

### `regen-koi` — Knowledge Commons

**Package:** `regen-koi-mcp` (npm)

Connects to the KOI semantic index — Regen's living knowledge base with 86,000+ indexed documents. Supports three search modes (lexical BM25, semantic vector, and hypothetical document embedding), entity resolution (people, projects, organisations), knowledge graph traversal, SPARQL queries over the linked data graph, and code graph queries for the Regen Ledger codebase itself.

Used by: `/regen-search`, `/regen-digest`, `/regen-code`

### `regen-network` — Regen Ledger Blockchain

**Package:** `regen-python-mcp` (PyPI)

Connects to Regen Ledger via gRPC. Exposes read-only queries across: ecocredit classes, projects, and batches; marketplace sell orders; on-chain governance proposals, votes, tallies, and parameters; staking, delegation, and validator data; bank balances and supply; and community pool.

Used by: `/regen-credits`, `/regen-market`, `/regen-governance`, `/regen-portfolio`

### `regen-compute` — AI Footprint & Credit Retirement

**Package:** `regen-compute` (npm)

Connects to the Regen Compute service. Estimates the energy and CO2 footprint of an AI session, browses available credits on the marketplace, and initiates credit retirement — either via a Regen Marketplace credit card link (no config required) or direct on-chain via mnemonic. Also supports cross-chain token payments via ecoBridge (50+ tokens across 10+ blockchains).

Used by: `/regen-footprint`, `/regen-credits`, `/regen-market`

### `registry-review` — Project Document Review

**Package:** `registry-review-mcp` (PyPI)

Runs an 8-stage structured review of carbon credit project documents against registry methodology requirements: document discovery, requirement mapping, evidence extraction, cross-document validation, and report generation. Requires an Anthropic API key for LLM-assisted extraction.

Used by: `/regen-review` *(skill planned — MCP included)*

## Skills

Nine skills are included, each routing through one or more of the MCP servers above.

### `/regen-search` — Search the knowledge commons

Searches KOI using hybrid retrieval (vector + keyword + knowledge graph). Supports source filtering (forum, GitHub, Notion, podcasts, YouTube), date-bounded queries, and entity-centric lookups. For entity queries (people, projects, organisations), resolves structured entity data and retrieves all related documents.

**Trigger phrases:** "search regen", "search KOI", "what does regen know about", "find in regen commons", "regen docs about"
**MCPs used:** `regen-koi`

---

### `/regen-digest` — Weekly activity digest

Generates a structured summary of recent Regen Network activity using the KOI knowledge base. Covers governance, development, community, and media activity. Can be scoped to a custom date range or filtered by domain.

**Trigger phrases:** "regen digest", "what's new at regen", "regen weekly summary", "regen activity update", "regen network news"
**MCPs used:** `regen-koi`

---

### `/regen-credits` — Explore ecocredits

Provides a unified view of the Regen credit ecosystem: credit types and classes, active projects, issuance batches, live marketplace listings with pricing, and network-wide aggregate stats (total retirements, credits issued vs retired, hectares under stewardship).

**Trigger phrases:** "ecocredits", "credit classes", "what credits are available", "carbon credits on regen", "biodiversity credits"
**MCPs used:** `regen-network`, `regen-compute`

---

### `/regen-market` — Market analysis

Analyses the Regen credit marketplace: current sell orders (by batch or seller), allowed payment denominations, market trends over time, and methodology comparisons across credit classes. Also surfaces a user-friendly browsable view of currently purchasable credits with pricing.

**Trigger phrases:** "regen market", "credit prices", "sell orders", "market trends", "compare methodologies", "marketplace activity"
**MCPs used:** `regen-network`, `regen-compute`

---

### `/regen-governance` — On-chain governance

Queries Regen Ledger governance: proposals (list, filter by status, full detail), vote tallies, individual voter positions, deposit status, governance parameters (voting period, quorum, threshold, veto threshold), and community pool balance.

**Trigger phrases:** "regen governance", "proposals", "regen votes", "voting results", "community pool", "active proposals"
**MCPs used:** `regen-network`

---

### `/regen-portfolio` — Wallet impact analysis

Analyses the ecological impact and holdings of a `regen1...` wallet address: credit holdings and retirements, token balances (REGEN, ecocredits, basket tokens), staking positions and validator delegations, active marketplace listings, and basket token composition.

**Trigger phrases:** "regen portfolio", "wallet impact", "analyze address", "ecological impact of address", "my regen credits"
**MCPs used:** `regen-network`

---

### `/regen-footprint` — AI session footprint & offset

Estimates the energy and CO2 cost of the current AI session (by duration and tool call count), shows matching credits available for retirement, and walks through credit retirement — generating either a Regen Marketplace credit card link or an on-chain transaction. Issues a verifiable on-chain retirement certificate.

**Trigger phrases:** "offset my session", "environmental cost of AI", "retire credits", "session footprint", "carbon offset"
**MCPs used:** `regen-compute`

---

### `/regen-code` — Codebase exploration

Explores the Regen Network codebase using KOI's code graph and GitHub document index. Supports call chain tracing, caller lookups, orphaned code detection, and architectural queries across Regen Ledger modules. Also provides structured repo overviews and tech stack summaries for individual repos (`regen-ledger`, `regen-web`, `regen-data-standards`, etc.).

**Trigger phrases:** "regen architecture", "regen codebase", "regen tech stack", "trace call chain", "find function in regen"
**MCPs used:** `regen-koi`

---

### `/regen-review` — Registry document review *(planned)*

Will guide an 8-stage structured compliance review of carbon credit project documents: document discovery and classification, requirement mapping against methodology checklists, evidence extraction with page-level citations, cross-document consistency validation, and structured Markdown + JSON report generation. Requires the Registry Review MCP and an Anthropic API key.

**Trigger phrases:** "registry review", "review project documents", "document compliance check"
**MCPs used:** `registry-review`

## Host Positioning

This package is host-agnostic: any environment that supports MCP and markdown skills can use it.

- **Cowork** remains the smoothest end-user experience — one-click plugin install, skill slash commands in the `/` menu, and automatic MCP loading.
- **Claude Code, Cursor, VS Code, and other MCP-capable hosts** use the same underlying MCPs and skills, typically through natural-language invocation and host-specific MCP configuration.

## Setup

See [SETUP.md](SETUP.md) for per-host installation and MCP configuration (Cowork, Claude Code CLI, Cursor, VS Code, and generic MCP hosts).

## Optional Environment Variables

These are needed only for specific capabilities. The core read-only functionality works without any configuration.

| Variable | MCP | Purpose |
|----------|-----|---------|
| `REGEN_WALLET_MNEMONIC` | `regen-compute` | Enable direct on-chain credit retirement (alternative to credit card link) |
| `ECOBRIDGE_EVM_MNEMONIC` | `regen-compute` | Enable cross-chain token payments via ecoBridge |
| `REGISTRY_REVIEW_ANTHROPIC_API_KEY` | `registry-review` | Required for LLM-powered document extraction in registry review |

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
