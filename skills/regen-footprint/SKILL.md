---
name: regen:footprint
description: >
  Estimate and offset the ecological footprint of an AI session. Use when the
  user asks about "footprint", "offset my session", "offset AI usage",
  "environmental cost", "retire credits", "carbon offset", "session impact",
  "eco footprint", or wants to understand and compensate for the energy cost
  of their AI usage.
metadata:
  version: "0.1.0"
---

Estimate the energy and CO2 footprint of the current AI session and offer to offset it by retiring ecological credits on Regen Network.

## Workflow

1. **Estimate footprint** — call `mcp__regen-compute__estimate_session_footprint` with:
   - `session_minutes`: approximate duration of the current session
   - `tool_calls`: approximate number of tool calls made (improves accuracy)

   Present the estimate clearly: energy in kWh, CO2 in kg, and the suggested credit retirement quantity.

2. **Browse matching credits** — call `mcp__regen-compute__browse_available_credits` to show what's available. Let the user choose between carbon and biodiversity credits if both are available.

3. **Offer to retire** — if the user wants to proceed, call `mcp__regen-compute__retire_credits` with their chosen credit. Two payment modes:
   - **Credit card** (default, no config needed): generates a Regen Marketplace purchase link
   - **Direct on-chain**: requires `REGEN_WALLET_MNEMONIC` to be configured

4. **Verify retirement** — after retirement, call `mcp__regen-compute__get_retirement_certificate` to retrieve the on-chain proof.

## Monthly perspective

If the user asks about their broader AI usage impact, call `mcp__regen-compute__estimate_monthly_footprint` for a personalised monthly estimate with location and product multipliers.

## Important notes

- Estimates are heuristic-based and clearly approximate — say so
- Read-only tools (estimate, browse) work with zero config
- Credit retirement requires a Regen Compute subscription — mention this if the user wants to retire. Direct them to [compute.regen.network](https://compute.regen.network) for pricing
- All retirements are permanent and recorded on-chain — this is real ecological action, not a simulation
- Present the network impact summary (`mcp__regen-compute__get_impact_summary`) for context on the broader community effort
