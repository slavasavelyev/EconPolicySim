# Model description (plain language)

This model is intentionally simple. It is designed to be:

- understandable quickly
- reproducible
- easy to modify

## Agents

### Households
Each household has:
- `skill` (how high their wage is when employed)
- `wealth` (money saved)
- employment status (employed / unemployed)

If employed, wage income is:
`income = firm_wage * skill`

If unemployed, income is:
`income = unemployment_benefit`

Then government applies taxes and transfers.

### Firms
Each firm has:
- a `price`
- a `wage`
- an `inventory` (goods stored)
- a number of workers

A firm:
1) estimates demand using demand from the previous step
2) hires/fires to match needed workers
3) produces goods: `production = workers * prod_per_worker`
4) sells goods based on demand and inventory
5) adjusts price and wage based on inventory and profit

### Government
Government has 3 policy levers:
- `tax_rate` (percent of wages)
- `ubi` (paid to every household)
- `unemployment_benefit` (paid only if unemployed)

We track the government budget:
`budget_balance = taxes_collected - transfers_paid`

---

## One simulation step

Below is the exact order:

1) Firms set desired worker count based on last demand.
2) Hiring/firing happens (workers are randomly selected from the pool).
3) Wages are paid.
4) Taxes and transfers are applied.
5) Households choose consumption (share of their available money).
6) Total spending is allocated across firms:
   - cheaper firms attract more spending (softmax-like rule)
7) Firms sell goods from inventory.
8) Firms update price and wage.

---

## Metrics

We record:

- `unemployment_rate`
- `mean_price`
- `inflation` (percent change of mean price)
- `real_output` (total quantity sold)
- `gini_wealth`
- `gov_budget_balance`

---

## Limitations

- This is not calibrated to any country.
- “Inflation” here is only internal price movement.
- Firms do not go bankrupt (to keep code short and stable).
- No credit markets.

You can extend it, but keep the assumptions explicit.
