# AETHER 1: Live Institutional Carbon Pricer (Beta)

A browser-based model that prices a carbon credit from first principles and reprices it instantly when you change any assumption. Built to show how project quality, policy status, market conditions and country risk combine into one defensible price. It is designed as a transparent, independent estimate to sit alongside registry methodologies such as Verra and Gold Standard. It does not replace or certify against them.

**[Open the live demo](YOUR-GITHUB-PAGES-LINK)** (no install, runs in the browser)

## What it does

You describe a credit: its project type, vintage, methodology, verification, host country and contract terms. AETHER 1 returns a fair value per tonne, and every input stays adjustable, so a buyer can stress any assumption and see the price move in real time.

- **36 adjustable inputs:** 22 sliders, 9 dropdowns and 5 on/off policy gates
- **7 project types:** REDD+, ARR, CCUS, renewables, agricultural soil, cookstoves, DAC
- **5 one-click scenarios:** Hormuz escalation, CBAM rollout, Article 6 collapse, INR crash, India carbon mandate
- **Live market inputs:** Brent/WTI and USD/INR feed the energy and FX-volatility inputs when the data sources are reachable

## Parameter Estimator tab

Quality inputs such as additionality and leakage risk are usually guesses. The **Parameter Estimator** tab estimates them from the characteristics of a project, so the price rests on a stated rationale instead of a hand-set slider.

### How to use it

1. Open the **Parameter Estimator** tab.
2. Enter the project's characteristics, starting with project type and location.
3. Read the estimated parameters and the reasoning shown beside each one.
4. Click **Apply** to load the estimates into the pricer.
5. Switch to the pricer tab and adjust any parameter you disagree with. The price updates immediately.

### How it delivers its estimates

The estimator starts from the risk profile that is typical for the project type, then adjusts it for where the project sits. Each estimate comes with a reason, so you can accept it or override it with project evidence.

| Project type | Typical risk profile | Reasoning |
|---|---|---|
| REDD+ (avoided deforestation) | Higher leakage, contested baseline | Pressure can move to neighbouring forest, and the "what would have happened" scenario is hard to prove |
| ARR (afforestation / reforestation) | Moderate leakage, reversal risk | Land use can shift elsewhere, and trees can be lost to fire, drought or pests |
| CCUS and DAC | Low leakage, strong additionality | The activity usually only happens if credit revenue pays for it, and storage is engineered |
| Renewables | Low leakage, weaker additionality | Projects often proceed without credit revenue, which weakens the claim |
| Cookstoves | Moderate leakage, usage uncertainty | Fuel demand can shift, and real usage is hard to measure |
| Agricultural soil | Moderate leakage, higher permanence risk | Carbon stored in soil can be released by later changes in practice |

**Location adjusts the result.** Climate risk is scored from the site's average temperature (T, in °C) and average precipitation (P) using a smooth curve, so there are no cliff edges when a figure crosses a threshold:

```
climateRisk = 50 + 25 * tanh((T - 30) / 8) - 20 * tanh((P - 2.5) / 1.5)
```

Hotter, drier sites score as riskier, which matters most for nature-based projects exposed to fire, drought and reversal.

### Planned inputs

These inputs are not built yet. Each would feed a specific parameter, so the estimates rest on more evidence.

| Planned input | Parameter it would inform | Why it matters |
|---|---|---|
| Project size and duration | Delivery risk, permanence | Larger, longer projects face more delivery and reversal exposure |
| Host country and region | Sovereign and policy risk, leakage | Governance and enforcement differ sharply by country |
| Land-use or baseline history of the site | Baseline credibility, additionality | A documented history makes the "what would have happened" case testable |
| Registry and methodology used | Baseline credibility, MRV | Methodologies differ in how conservatively they set baselines |
| Monitoring frequency and who verifies | MRV | More frequent, independent checks lower measurement risk |
| Community and ownership structure | Permanence, leakage | Secure land tenure and local benefit reduce reversal and displacement |
| Permanence buffer and insurance | Permanence | A larger buffer or insurance cover lowers the cost of reversals |
| Proximity to deforestation or leakage pressure | Leakage | Nearby pressure raises the chance activity just moves next door |

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

## How to use the pricer

1. Pick a project type and set its attributes, or load them from the Parameter Estimator.
2. Move any slider or change any dropdown. The price updates immediately.
3. Toggle the policy gates to see what Article 6 or CBAM status is worth.
4. Click a scenario to apply a shock, then compare against the baseline.

## Limitations (please read)

- **The weights are my judgment calls**, chosen to reflect how buyers and rating agencies discuss credit quality. They are not calibrated against transaction data. Change them and the price will change.
- **Benchmark prices and country-risk inputs are static assumptions**, not live quotes. Only the Brent/WTI and USD/INR inputs are fetched live.
- **The confidence interval is parametric**, derived from the risk and liquidity inputs. It is not a simulation.
- **Estimator outputs are model-based suggestions.** They are not audited and not a verification of any real project. Use them as a starting point and override them with project evidence.
- This is a beta for exploring how assumptions interact. It is not investment advice or a price quote.

## Tech

A single self-contained HTML file with plain JavaScript. No build step, no dependencies to install.

## Author

**Hitarth Dand**, finance graduate moving from equity research into carbon markets.
hitarthdand@gmail.com
