---
name: regen:market
description: >
  Analyse the Regen Network credit marketplace. Use when the user asks about
  "regen market", "credit prices", "sell orders", "market trends", "compare
  methodologies", "marketplace activity", "credit pricing", or wants market
  intelligence on ecological credits.
metadata:
  version: "0.1.0"
---

Provide market analysis for Regen Network's ecological credit marketplace.

## Workflow

1. **Current sell orders** — call `mcp__regen-network__list_sell_orders` for a full listing, or filter with:
   - `mcp__regen-network__list_sell_orders_by_batch` for a specific credit batch
   - `mcp__regen-network__list_sell_orders_by_seller` for a specific seller
   - `mcp__regen-network__get_sell_order` for a single order by ID

2. **Allowed payment denominations** — call `mcp__regen-network__list_allowed_denoms` to show what currencies are accepted.

3. **Market trends** — call `mcp__regen-network__analyze_market_trends` for trend analysis across credit types and time periods.

4. **Methodology comparison** — if the user wants to compare credit approaches, call `mcp__regen-network__compare_credit_methodologies` with relevant class IDs (e.g. C01, C02, C03).

5. **Live marketplace browse** — call `mcp__regen-compute__browse_available_credits` for a user-friendly view of what's currently purchasable, with pricing.

## Presenting results

- Present pricing clearly: amount per credit, denomination, quantity available
- When comparing methodologies, use a table format for easy scanning
- For trends, describe direction and magnitude in plain language
- Note that prices are on-chain and may differ from OTC or secondary markets
- Link to [app.regen.network](https://app.regen.network) for direct marketplace access
