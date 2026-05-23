# Trade Execution Framework

## Pre-Trade

### Record Your Trades

### How Does Technical Analysis Look?

Look at longer time frame support/resistance and trend lines.

Metrics: RSI, 14/50/200d MA. What caused previous volatility?

### How to Size the Position?

Take 1-4% risk on the trade — rate the trade confidence out of 10 and assign risk according to it.

- A 5 should yield 50 to 80% upside.
- 5 by 5: up 5x in 5 years for equities.
- A 4% trade is one where you are happy to buy if it is 20% down. Split up your investments into 1% to DCA.
- Set wide stop losses. Dictated by recent asset volatility. Don't put at obvious stops.
- Options can protect against left-tail risk (using options itself or as a hedge).
- Check for correlations between existing positions; use common sense and historical back-testing: geography, asset classes.
- Look for spreads instead that can hedge against systemic risk or risk-on/off.
- Implement the trade in a way that can protect your downside (strongest markets/least downside markets if you are wrong).

### Relative Value (RV) Trades

RV trades depend on mean reversion and also the volatility of the spread, plus the absence of structural changes. Sometimes if it is just one leg that is dislocated, RV may not be necessary.

### Perpetual Funding Rate — Cost of Carry for Perps

Perpetual futures have no expiry (unlike CME quarterlies — no rolling needed). The funding rate is a periodic payment (typically every 1-8 hours; Hyperliquid pays hourly) exchanged directly between longs and shorts to keep the perp price tethered to spot.

- Perp trades above spot (more demand from longs) → longs pay shorts. Funding is positive.
- Perp trades below spot (more demand from shorts) → shorts pay longs. Funding is negative.
- The exchange doesn't take this fee — it's a peer-to-peer transfer. Mechanically it's the perp market's version of "cost of carry" in traditional futures.

**Why funding eats RV pair trades specifically:**
A single directional trade pays or receives funding on one leg. A pair trade pays or receives on both legs simultaneously, and the signs usually don't cancel cleanly.

Example — Long BTC / Short ETH pair:
- Long BTC leg: you pay funding (crypto perps are structurally biased long — retail prefers the long side, so funding is positive most of the time, historically ~5-15% annualized on BTC).
- Short ETH leg: you receive funding.
- Net: roughly washes if BTC and ETH funding are similar.

Example — Long altcoin / Short altcoin pair:
- If the long leg's funding spikes (e.g. 30-50%+ annualized during catalyst windows like ETF speculation) AND the short leg's funding goes negative (thinly traded perp, more shorts than longs), you pay funding on BOTH legs simultaneously.
- At 30% annualized combined funding cost over a 12-week hold, you lose ~7% before the trade even moves against you. If your stop is -10% on the spread, funding alone could trigger it.

**Time scaling — why it's worse for multi-month holds:**
- 2-day trade on 20% annualized funding: ~0.1% cost. Negligible.
- 6-month trade on 20% annualized funding: ~10% cost. Dominates R/R math.
- A trade targeting +10% over 1 quarter with 5-10% net funding cost becomes net 0-5%. The thesis can be right and you still barely break even.

**How to manage funding risk:**
1. Check 30-day average funding for each leg before entering (Coinglass or venue-native funding history). Don't trust spot funding — it's noisy.
2. Estimate total carry cost: (long_funding - short_funding) x days_held / 365. If that number is more than ~30% of your target return, the trade is structurally compromised.
3. For long-duration RV, consider dated futures (CME, Deribit) instead of perps — fixed cost of carry baked into the basis, no surprise funding spikes.
4. For shorter holds (4-12 weeks), perps are usually fine if you monitor weekly and exit if funding inverts persistently against you.
5. Asymmetric funding can also be alpha — if a token's funding is paying 40% annualized to shorts during a run-up, that's a signal positioning is overcrowded long, and a contrarian short with funding tailwind becomes its own trade.

**TL;DR:** Funding is a real cash cost on your P&L every hour, it scales linearly with time, and it's most dangerous on RV trades where you don't get the directional payoff to mask the bleed.

## Trade Monitoring

### Monitor If the Reason You Wrote Still Holds

- Can consider closing if your idea did not manifest — no need to wait for the stop, or can extend the stop.
- Would you still put on the trade today?
- Has information come out against your thesis?

### How Much Profit Is Left to Run?

Same as the risk-reward profile of the entry — trim as potential upside decreases.

### Has It Broken Through Any Major Support or Resistance Lines?

Don't fall in love with a trade; accept when it goes wrong.

## Self-Evaluation and Scoring (P72)

Score yourself on whether the specific research call was right, not on whether the stock moved. "You're not getting paid on the return, you're getting paid on the Grinch."

"You want to be able to track it maniacally and be brutally intellectually honest about whether it was right or wrong because you scoring yourself hard is better than anything else."

"You want to get it right, not be right." — never get too married to a view.

## Size Based on Conviction and Evaluate

PM framework: at-bats x hit rate x sizing. Optimize across all three.

- **Hit rate game (Point72):** many trades, tight execution, catalyst-driven. "Understanding when stories changed one degree instead of 10." Turn the book far more frequently. 120 teams inside the organization investing in different ways.
- **Slugging game (Glen View):** fewer bets, 2-year time horizon, 50% of capital in top 10 positions. "You had to know your names and you had to know them cold." Deep-tissue understanding: PE-trained analysts comfortable in Excel with deep P&L decomposition, unit cost and unit driver analysis, strong management relationships.

## Grade Signals and Update Probabilities

"There are different types of signals — low grade, medium grade, high grade — and that can change over time depending on where you are in the story." It is not binary; it is on a spectrum.

Each data point updates your decision tree.

**Trade Example — Lululemon Short:** base case 60/40. Mall walks, inventory checks, Alo/Vuori competitive intelligence shifted it to 80/20. Doubled position from $100 to $200 short. "You just took the probability from 60/40 to 80/20 and you would size it appropriately."

## Match Against Historical Playbooks

"When was the last time you saw this? 2016. What happened after? You saw cuts, then this happened. Timeline? 3 weeks, 3 months, 3 years. Then I had a playbook and you're just matching against the playbook."

"If I look at history it tells you the future."

**Trade Example — Beijing Olympics 2008:** China shut ports, forcing pre-buying of Caterpillar equipment. Created an air pocket that layered on top of the financial crisis. "Once everything opened back up there was nothing to buy, so it creates an air pocket. Again, it's inventory."

**Trade Example — Lululemon Long Counterargument:** a long investor could see the same data and apply the Abercrombie & Fitch 2017 turnaround analog ($10 to $25 over 2 years). Both valid — difference is time horizon. "You need persistence of capital and I need timing."

## Asset Class Thesis

### Options

C = S + P - X / (1 + r)^t (X is strike)

#### The 5 Greeks

**a. Delta** — Measures change in option premium for a unit change in the underlying. Also seen as the probability the option expires ITM. Range: 0 (deep OTM) to 1 (deep ITM), 0.5 for ATM. As expiry approaches, delta moves to 1 for ITM, 0 for OTM. In rates, delta = duration.

**b. Gamma** — How delta changes with the underlying. Second derivative. Gamma = convexity (bond analogy). Maximizes at ATM and near expiry. Short-dated or low vol = peaked gamma. Long-dated or high vol = flatter gamma. P/L from gamma = 1/2 x gamma x (change in underlying)^2.

**c. Theta** — Time decay of option premium. Negative for long positions (buyers), positive for short positions (sellers, "collecting theta"). Accelerates near expiry, especially for ATM. Opposite payment structure of gamma.

**d. Vega** — Sensitivity of premium to changes in implied volatility. ATM options have highest vega. Longer-dated = higher vega. "Trading gamma" = short-dated options. "Trading vega" = long-dated options. P/L = vega x change in IV.

**e. Rho** — Sensitivity of option price to changes in interest rates.

#### Additional Options Insights

- Low volatility creates cheap options — advantageous to participate in speculative market bubbles.
- Low historical volatility predicts future increases — ironically, low historical vol is the best predictor of future increases, and vice versa.
- Options market mispricing due to incorrect assumptions — the market assigns normal probability distributions to situations with clear bimodal outcomes.
- Longer-duration options often priced with lower IV, which contradicts logic.
- Volatility scales with sqrt of time — reasonable short-term but can underestimate longer periods.
- Selling puts can generate abnormal returns — due to natural demand for hedges.

### Gold

- Gold does well in uncontrolled inflation (when the CB is expected to fail) or periods of low real growth.
- Inverse relationship between gold and bonds.
- Gold and bonds can rise together during early deleveraging cycles.
- CB Impacts — central banks hold gold for foreign currency reserves; affected by rates, dollar strength, geopolitics.

### FX

- FX typically focuses on yield differentials or growth, depending on risk-on vs. risk-off.
- Currencies segmented into reserve vs. high-beta/growth.
- CB Impacts — central banks hold foreign currency reserves; affected by rates, dollar strength, geopolitics.
- Cost of carry, equity performance, fiscal stability.

### Rates

**Nominal Bond Yield Drivers:** Interaction between growth expectations, inflation expectations, and the term premium. Credit risk for EM or high fiscal deficit countries.

**Technical Factors:**
- Supply/Demand Imbalances: Heavy issuance or QT pushes yields higher; QE or pension fund demand compresses yields. Not just notional but also duration.
- Positioning and Flow Dynamics: Dealer balance sheets, hedge fund duration trades, convexity hedging (MBS-related selling). Local corporations, banks, insurance, SWFs, central banks, pension fund flows.
- CB care about: 1. Growth 2. Inflation 3. Financial market stress 4. STIR market pricing

**Growth:** Structural (demographics, productivity — affects long end) vs. Cyclical (credit cycle, fiscal policy, labor — affects short end).

**Inflation Expectations:** CB actions are short-term signals. Long-term yields reflect beliefs about sustainable inflation. Oil price.

**Term Premium:** Compensation for uncertainty about future inflation/growth, volatility, liquidity preferences, policy shocks.

**EM Risk:** Economic health, currency risk (local currency weakens = bond value drops for foreign investors), political stability, local rates, risk appetite. Debt in foreign currency can lead to default.

### VIX

- Implied Vol is driven by Realized Vol (~85% correlation for short-dated).
- Implied Vol exceeds Realized Vol — VRP is significant; carry strategies capitalize on vol shortfall.
- Down vol exceeds up vol in equity indices. Commodities often exhibit up vol > down vol.
- Vol surface has put skew — markets crash down, not up.
- Vol surface has term structure — IV consistently higher for longer-dated. Backwardation signals RV spikes.
- Vol has memory — today's vol is the best predictor of tomorrow's. Low vol breeds stability.
- Vol mean-reverts — disruption events are temporary.
- Vol regimes change — unique regimes from monetary, fiscal, regulatory interactions.
- Vol is reflexive — persistent low risk premia fuel riskier trades; surges force liquidation.
- Vol events result from the shattering of consensus — recency bias, overconsumption of carry trades.

### Credit

**Types of Risks:** Interest Rate Risk (macro, hedged with Treasuries/swaps), Market-Specific Risk (sector/systemic), Issue-Specific Credit Risk (idiosyncratic).

**Hedging During Stress:** Swaps vs. Treasuries — Treasuries often underperform as hedges in crises (rally sharply as safe havens). Swaps track credit assets better, carry bank counterparty risk.

**Key Drivers:** Bank credit concerns widen swap spreads. Mortgage hedging flows. Too much corporate issuance leads to more Treasuries being issued. Important to look at duration supply, not just supply.

### Oil

Geopolitics (Middle East, Venezuela, USA), EIA figures, OPEC figures.

Oil is best modeled as the sum of multiple interacting blocks:
- FX (USD/DXY): local-currency affordability and portfolio/financial conditions. Bi-directional causality.
- Rates: affect cost of carry and opportunity cost of inventories.
- Supply: OPEC+ decisions, compliance, unplanned disruptions.
- Demand: increasingly non-OECD-driven. More sensitive to FX and fiscal constraints.
- Inventories and refinery runs: set the prompt physical/basis tone.
- Geopolitics and shipping: risk premium at chokepoints. Hormuz ~20 mb/d.

## Market Cycles and Behavioral Discipline

- Cycles repeat, but intensity and duration vary. Driven by secular trends.
- Falling prices receive more negative attention than rising prices.
- Investors cycle between greed and fear. Unpredictability is crucial to accept. Remain unemotional.
- It is not the data that matters, but how it is interpreted based on current sentiment.
- Be cautious when others are taking more risks, and risk-seeking when others are cautious.
- Accumulate below intrinsic value and distribute when they exceed it.

### Behavioral Discipline (P72)

- "You want to get it right, not be right." Be cold about the analysis.
- Confirmation bias is a real threat — separate research from portfolio decisions to protect objectivity.
- Knowledge compounds — "your 12 months in 2011 is not as effective as in 2013 because you're compounding knowledge."
- Score yourself on the specific call, not the P&L.
- "Nothing beats doing the work."
- Separate research from P&L: "You don't want to bias... you want to be really focused on getting it right."

### History Rhymes (P72)

"History rhymes because people are animals and animals do the same thing over and over again expecting a different result."

"The fact that we're debating about a bubble means there's a bubble. If people are trying to explain away a bubble, there's a bubble. It's that simple."
