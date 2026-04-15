---
name: regen:governance
description: >
  Query Regen Network governance activity. Use when the user asks about
  "regen governance", "proposals", "regen votes", "voting results",
  "community pool", "governance params", "active proposals", or wants
  to understand on-chain governance decisions.
metadata:
  version: "0.1.0"
---

Query and present Regen Network blockchain governance data.

## Workflow

1. **List proposals** — call `mcp__regen-network__list_governance_proposals` to see all proposals. Filter by status if the user asks for active, passed, or rejected proposals.

2. **Proposal detail** — for a specific proposal, call `mcp__regen-network__get_governance_proposal` with the proposal ID to get full description, timing, and status.

3. **Vote tallies** — call `mcp__regen-network__get_governance_tally_result` for a proposal's current or final vote breakdown (Yes, No, Abstain, No With Veto).

4. **Individual votes** — call `mcp__regen-network__list_governance_votes` to see who voted and how, or `mcp__regen-network__get_governance_vote` for a specific voter's position.

5. **Deposits** — call `mcp__regen-network__list_governance_deposits` or `mcp__regen-network__get_governance_deposit` to check deposit status for proposals in deposit period.

6. **Governance parameters** — call `mcp__regen-network__get_governance_params` three times with `params_type` set to `voting`, `tallying`, and `deposit` respectively, to get the full picture: voting period, quorum, threshold, veto threshold, and deposit requirements.

7. **Community pool** — call `mcp__regen-network__get_community_pool` for the current community pool balance.

## Presenting results

- For active proposals, lead with the proposal title, what it does, and when voting ends
- Present vote tallies as percentages alongside raw numbers
- For passed proposals, summarise the outcome and margin
- Note quorum requirements when relevant (from governance params)
