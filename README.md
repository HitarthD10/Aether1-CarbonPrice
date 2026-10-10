# Aether1-CarbonPrice
# AETHER 1: Live Institutional Carbon Pricer (Beta)

A browser-based model that prices a carbon credit from first principles and reprices it instantly when you change any assumption. Built to show how project quality, policy status, market conditions and country risk combine into one defensible price.

**[Open the live demo](YOUR-GITHUB-PAGES-LINK)** (no install, runs in the browser)

## What it does

You describe a credit: its project type, vintage, methodology, verification, host country and contract terms. AETHER 1 returns a fair value per tonne, and every input stays adjustable, so a buyer can stress any assumption and see the price move in real time.

- **36 adjustable inputs:** 22 sliders, 9 dropdowns and 5 on/off policy gates
- **7 project types:** REDD+, ARR, CCUS, renewables, agricultural soil, cookstoves, DAC
- **5 one-click scenarios:** Hormuz escalation, CBAM rollout, Article 6 collapse, INR crash, India carbon mandate
- **Live market inputs:** Brent/WTI and USD/INR feed the energy and FX-volatility inputs when the data sources are reachable

## How the price is built

The model starts from a benchmark price for the project type, then applies adjustments for vintage, integrity, country risk, market conditions and policy status.

**Integrity score (6 weighted factors)**

| Factor | Weight |
|---|---|
| Additionality | 28% |
| Permanence | 20% |
| MRV (measurement, reporting, verification) | 18% |
| Baseline credibility | 16% |
| Leakage | 10% |
| Double-counting risk | 8% |

**Risk score (5 weighted factors)**

| Factor | Weight |
|---|---|
| Sovereign | 32% |
| Policy | 22% |
| FX | 20% |
| Delivery | 18% |
| Energy | 8% |

The integrity score becomes a price multiplier, and the risk score becomes an exponential discount. Further adjustments cover liquidity, buyer type, co-benefits, contract terms, an emerging-market governance discount, and five policy gates: Article 6 status, corresponding adjustment, host-country authorization, export eligibility and CBAM recognition.

## Outputs

- Fair value per tonne
- 90% confidence interval
- Tail-risk (CVaR) estimate
- 10-year forward curve
- Ranked sensitivity breakdown showing which factor moves the price most

## How to use it

1. Pick a project type and set its attributes.
2. Move any slider or change any dropdown. The price updates immediately.
3. Toggle the policy gates to see what Article 6 or CBAM status is worth.
4. Click a scenario to apply a shock, then compare against the baseline.

## Limitations (please read)

- **The weights are my judgment calls**, chosen to reflect how buyers and rating agencies discuss credit quality. They are not calibrated against transaction data. Change them and the price will change.
- 1. The risk discount takes exponential form: Δ = exp(−λR)

We chose an exponential rather than a linear discount because risk in carbon markets doesn't reduce price at a constant rate  it reduces the pool of willing buyers, and that pool shrinks faster as risk accumulates. A compliance buyer who tolerates moderate sovereign risk will not simply accept slightly less; above a threshold they exit the transaction entirely. The exponential captures this convexity: moving from a risk score of 20 to 40 costs less in price terms than moving from 60 to 80, because the latter crosses into territory where entire buyer categories: CORSIA operators, CBAM-exposed firms are disqualified. λ = 0.011 was set so that a fully maxed-out risk score of 100 produces a 67% discount, which is the approximate peak-to-trough price collapse observed in high-risk project categories during the 2022–23 VCM integrity crisis.
- **Benchmark prices and country-risk inputs are static assumptions**, not live quotes. Only the Brent/WTI and USD/INR inputs are fetched live.
- **The confidence interval is parametric**, derived from the risk and liquidity inputs. It is not a simulation.
- This is a beta for exploring how assumptions interact. It is not investment advice or a price quote.

## Tech

A single self-contained HTML file with plain JavaScript. No build step, no dependencies to install.

## Author

**Hitarth Dand**, finance graduate moving from equity research into carbon markets.
hitarthdand@gmail.com
