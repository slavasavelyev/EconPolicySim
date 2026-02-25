# EconPolicySim — reproducible toy policy simulator (agent‑based)

**EconPolicySim** is a small **agent-based** simulation to explore policy tradeoffs in a **toy economy**:
tax rate, universal basic income (UBI), and unemployment support.

It is not a forecast of the real world. The goal is to show **clear assumptions**, **reproducible experiments**,
and **honest metrics**.

✅ What this repo is good for:
- You can read the full model in 5 minutes.
- You can reproduce results with 1 command.
- You can change a policy lever and see how metrics move.
- The code has tests + CI.
  
![CI](https://github.com/slavasavelyev/OrbitLab/actions/workflows/ci.yml/badge.svg)
![Baseline vs reform](docs/comparison.png)
---

## Quick start (5 minutes)

### 1) Create a virtual environment
Windows (PowerShell):
```bash
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

macOS / Linux:
```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 2) Install dependencies
```bash
pip install -r requirements.txt
```

(Optional, for tests and linting)
```bash
pip install -r requirements-dev.txt
```

### 3) Run the demo
```bash
python -m econpolicysim demo
```

You will get an `outputs/` folder with:
- `baseline_metrics.csv`
- `reform_metrics.csv`
- `comparison.png`

---

## How the model works (simple words)

This is a **toy** economy with:
- **Households** (people): they can be employed or unemployed, they earn income, consume, and save wealth.
- **Firms**: they hire/fire workers, set prices, produce goods, and sell goods.
- **Government**: it collects income taxes and pays transfers (UBI + unemployment support).

Each simulation step does this:

1) **Firms decide how many workers they need**
   - If demand was high, they hire.
   - If demand was low, they fire.

2) **Households get income**
   - If employed: wage × skill
   - If unemployed: unemployment benefit
   - Everyone also receives UBI (if UBI > 0)

3) **Government collects taxes**
   - Tax is a simple percent of wages.

4) **Households decide consumption**
   - A fixed share (MPC) of their available money is spent.

5) **Demand is allocated across firms**
   - Cheaper firms attract more demand (simple price-sensitive rule).

6) **Firms produce and sell**
   - Sales are limited by inventory.
   - Profit is revenue − wage bill.

7) **Firms update prices and wages**
   - If they run out of inventory: price goes up.
   - If they have too much inventory: price goes down.
   - If profit is strong: wages may rise.
   - If profit is negative: wages may fall.

We then record metrics:
- unemployment rate
- mean price and inflation (price change)
- total output (sales)
- inequality (Gini of wealth)
- government budget balance

See full details in [`docs/model.md`](docs/model.md).

---

## Run your own experiment

### Single run
```bash
python -m econpolicysim run --steps 300 --tax 0.20 --ubi 2.0 --seed 1
```

Outputs go into a timestamped folder inside `outputs/`.

### Policy sweep (optional, more impressive)
```bash
python experiments/policy_sweep.py --steps 200 --seed 0
```

This creates heatmaps (example: inequality vs unemployment) for a grid of policies.

---

## Why this is useful (and honest limitations)

**Useful**:
- Clear assumptions, fast runs, easy to modify.
- Good for learning about feedback loops and tradeoffs.

**Not a real-world forecast**:
- No real data calibration here.
- No real financial sector.
- No realistic labor market frictions.
- The “inflation” is only price movement inside this toy model.

---

## Project structure

- `econpolicysim/` — simulation package
- `experiments/` — repeatable experiments (policy sweeps)
- `tests/` — tests (pytest)
- `docs/` — model explanation

---

## Development

Run tests:
```bash
pytest
```

Format + lint:
```bash
ruff format .
ruff check .
```

---


## License
MIT License (see `LICENSE`).

## Citation
If you reference this work, please use `CITATION.cff`.
