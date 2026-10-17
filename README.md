# AETHER 1: Live Institutional Carbon Pricer (Beta)

**Price a carbon credit from its fundamentals, not its headline.** AETHER 1 builds a fair value per tonne from production cost, project integrity, host-country risk, policy status and live market conditions. Every assumption is visible and adjustable, and the price updates the moment you change one.

**[Open the live demo](https://hitarthd10.github.io/Aether1-CarbonPrice/)** (runs in the browser, nothing to install)

---

## Why it exists

Voluntary carbon credits are priced opaquely. Two credits with the same headline price can differ enormously in quality: one may represent a real reduction in a stable jurisdiction, another may not. Independent research suggests only a minority of issued credits are real reductions, so a price that ignores quality, policy status and country risk is a poor basis for an institutional decision.

AETHER 1 prices the credit instead of quoting the market. It answers three questions:

1. What does one credible tonne of this project type cost to produce?
2. How much should integrity, risk and policy status move that price?
3. How sure are we, and what is driving the answer?

## At a glance

| | |
|---|---|
| **Project types** | 13, from REDD+ and ARR to biochar, CCUS and direct air capture |
| **Adjustable inputs** | 36: 22 sliders, 9 dropdowns, 5 policy gates |
| **Live data sources** | 14 free public feeds (FX, oil, governance, climate, geocoding, registry, rates, news, hazards) |
| **Stress tests** | 5 one-click scenarios |
| **Estimator** | Researches a project from its type and location and fills the whole pricer |
| **Outputs** | Fair value, 90% interval, downside tail, 10-year path, ranked factor attribution |
| **Transparency** | Every number is tagged LIVE, STALE or STATIC with its source and age |

---

## How a price is built

```
Cost-based anchor  ->  vintage  ->  integrity  ->  risk  ->  market & structure  ->  policy gates  ->  fair value
```

**1. Anchor: what a credible tonne costs.** The starting price is the levelised production cost of the project type plus a 15% developer margin, taken from published cost ranges. Published market averages are shown beside the result for comparison and never enter the price. Each anchor carries an evidence grade, and weaker grades widen the uncertainty band.

**2. Vintage.** Older credits trade at a discount. The discount is calculated from the credit's age on the valuation date, rising to about 23% inside the range observed in transaction data.

**3. Integrity score (six weighted factors).** Quality becomes a price multiplier through a bounded, centred curve, so a poor credit is discounted and an excellent one earns a premium.

| Factor | Weight |
|---|---|
| Additionality | 28% |
| Permanence | 20% |
| MRV (measurement, reporting, verification) | 18% |
| Baseline credibility | 16% |
| Leakage | 10% |
| Double-counting risk | 8% |

Methodology, verification frequency and auditor quality can only help or hurt in the right direction: a better auditor never lowers the price.

**4. Risk score (four weighted factors).** Risk becomes an exponential discount, because risk compounds: each added unit of risk removes buyers faster than a straight line would suggest.

| Factor | Weight |
|---|---|
| Sovereign | 32% |
| Policy stability | 22% |
| FX volatility | 20% |
| Delivery | 18% |

Energy prices act through one channel only, a direct effect on compliance-linked carbon, so they are never counted twice.

**5. Market and structure.** Liquidity, compliance-market linkage (CORSIA, EU ETS), buyer type, contract structure and banking optionality each adjust the price within bounded ranges. Co-benefits and registry carry only small capped effects, because transaction data shows little pricing power for them.

**6. Policy gates.** Article 6 status, corresponding adjustment, host-country authorization, export eligibility and CBAM recognition work as on/off gates. A missing authorization does not shave a few percent off a credit. It removes whole buyer pools, so each gate applies as a multiplier.

---

## The Parameter Estimator: from a project to a price in one pass

Most people don't know a project's leakage risk or baseline credibility. The estimator researches the project from what you do know, shows its reasoning, and loads the result into the pricer.

### Step by step

1. **Open the Parameter Estimator tab.**
2. **Choose a project type and a host country.** These are the only required inputs. Type the project's GPS coordinates instead and the country fills in automatically.
3. **Add anything else you know**, such as hectares, vintage, developer, methodology, verification frequency, auditor, registry, and the legal status of Article 6, corresponding adjustment, authorization and CBAM. Anything left blank is estimated from the type and the country.
4. **Optional: enter a Verra project ID and click FETCH.** The tool looks the project up in Verra's public registry and fills in the name, country and status. It sets the project type from the registry's category, and uses the record's annual emission reductions as the crediting volume when the record has one. Verra IDs only.
5. **Click Research Project & Estimate All Parameters.** The tool resolves the location, pulls climate data for the coordinates, infers the biome, estimates the crediting volume and derives every parameter. Each estimate appears with a plain-language rationale.
6. **Click Apply to Pricing Engine.** The tool switches to the pricer, sets every slider, dropdown and gate to the estimated values, and reprices instantly.
7. **Adjust anything you disagree with.** Your judgement overrides the estimate and the price moves immediately.

### How the estimates are produced

Each parameter starts from an evidence-based score for the project type, then adjusts for place.

- **Additionality** starts from the share of credits that independent research found to be real reductions for that type (for example, about 11% for cookstoves and 25% for avoided deforestation). Types without rigorous evidence use a clearly marked analyst judgement. A high-pressure deforestation setting does not raise the score, because inflated baselines are a known cause of over-crediting. An optional ICVCM Core Carbon Principles label adds a small, capped uplift.
- **Permanence** responds to the biome and to a climate-reversal score built from the site's temperature and rainfall:

```
climateRisk = 50 + 25 * tanh((T - 30) / 8) - 20 * tanh((P - 2.5) / 1.5)
```

T is the average daily maximum temperature (°C) and P the average daily precipitation (mm), from climate-model data for the coordinates. The curve is smooth, so a small change in either input never causes a jump in risk.
- **Leakage** blends geographic pressure and market-specific displacement risk.
- **Sovereign risk, policy stability and governance** come from World Bank governance indicators for the host country.
- **FX volatility** is the 30-day realised volatility of the host currency.
- **Liquidity and scale** follow from the crediting volume.

---

## Live data and a world-events layer

The tool reprices on its own as conditions change. Prices refresh every minute and the news layer every ten minutes.

| Layer | Sources |
|---|---|
| FX, oil, rates | Public market-data feeds with fallback chains; US Treasury rate for the forward curve |
| Country risk and macro | World Bank governance, inflation and growth indicators |
| Project environment | Open-Meteo climate data; OpenStreetMap geocoding; Verra public registry |
| News | GDELT and Google News, searched for carbon policy, energy chokepoints, macro stress and politics |
| Hazards | NASA EONET and USGS earthquake feeds, matched to the project's coordinates |

**News overlay.** Headlines are classified by transparent rules (energy chokepoints, carbon policy, integrity scandals, macro stress, political instability, demand deals, and wildfire, flood and earthquake hazards). Each event moves only the factors it affects, with its effect limited in four ways:

- It **saturates** by the number of independent sources, so one story repeated a hundred times does not count a hundred times.
- It **decays** with a half-life.
- Every factor has a **cap**.
- Hazards act only within a set radius of the project.

An **audit table** shows each event, the factor it moved and the effect on price. Individual events can be switched off, and the whole overlay can be turned off to see the baseline price.

**Freshness is never hidden.** Every number carries a LIVE, STALE or STATIC tag with its source and age. Values with no free live source sit in one dated STATIC block, and old ones are flagged for review. A **valuation date** controls all age-based calculations, so you can price as of today or backdate to a past day.

---

## Uncertainty and scenarios

- **Interval.** Prices are modelled as lognormal, so the 90% interval is always positive and asymmetric. The width grows with risk, illiquidity, weak verification, weak cost evidence and the age of static inputs.
- **Downside tail.** The tool reports a 2.5% downside value for stress planning.
- **10-year path.** An illustrative drift-and-jump scenario path, not a forecast.
- **Factor attribution.** A ranked, one-at-a-time breakdown shows which input moves this price the most. The effects are not additive.
- **Scenarios.** One click applies Hormuz escalation, CBAM full rollout, Article 6 collapse, an INR crash or an India carbon mandate on top of live conditions.

---

## Quick start

1. Open the **Parameter Estimator**, pick a project type and country, and run the estimate.
2. Click **Apply to Pricing Engine** and read the fair value, interval and attribution.
3. Move any slider to test an assumption, for example sovereign risk or additionality.
4. Toggle the **policy gates** to see what Article 6 or CBAM status is worth.
5. Click a **scenario** to stress the price, then compare with the baseline.
6. Open the **world-events panel** to see which news is moving the price, and switch events off to test your own view.

---

## Where each choice comes from

| Design choice | Evidence |
|---|---|
| Additionality has the largest weight; scores by project type | Meta-study of 2,346 crediting projects (Probst et al., Nature Communications, 2024) |
| Host-country governance drives the emerging-market gap | Transaction-level study of about 260,000 retirements (WFE Working Paper No. 11, 2025) |
| Vintage discount range; weak pricing power for co-benefits and registry | Same study |
| Integrity premium sized against published rating spreads | Public rating-agency price studies |
| Cost-based anchors | Published levelised-cost literature, graded by evidence quality |

---

## Limitations (please read)

- **The weights are my judgement calls.** They reflect how buyers and rating agencies discuss quality, and they have not been calibrated against transaction data. Change them and the price changes.
- **Some inputs are static.** Cost anchors, benchmark prices, governance priors for missing data and the emerging-market gap carry a source and a date, and are not live.
- **Scores for types without strong studies are analyst judgement** and are marked as such.
- **The news overlay is rule-based.** It reflects headlines and keyword rules, and it is capped on purpose. It does not predict prices.
- **The forward path is illustrative**, not a forecast.
- **Estimates are model-based suggestions.** They are not an audit or a verification of any real project, and the tool is not investment advice or a price quote.
- Free data feeds can fail or be rate-limited. When one does, the tool keeps the last value and tags it STALE.

## Tech

A single self-contained HTML file with plain JavaScript. No build step, no dependencies to install.

## Author

**Hitarth Dand**, finance graduate moving from equity research into carbon markets.
hitarthdand@gmail.com
