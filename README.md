<!-- solve-problem:a2dcf800-e889-4185-aca7-0f800caa1a6d -->
# Dynamic equilibrium of concentrated liquidity providers

A [Tangle Solve problem](https://solve.tangle.tools/problems/a2dcf800-e889-4185-aca7-0f800caa1a6d).

## What is the problem?

Uniswap v3 allows liquidity providers (LPs) to allocate liquidity over price intervals ("ranges") rather than the entire price curve; fees accrue only when the market price lies inside a range, and re-positioning costs gas. Formalize a **repeated/stochastic game**: time is discrete t = 0, 1, 2, ...; n LPs with heterogeneous endowments (and possibly beliefs) each period choose an allocation of liquidity over a discrete price grid (or measurable price space), paying a gas/switching cost c when their allocation changes. The market price follows some process — ideally endogenous, determined by arbitrageurs who trade against the aggregate liquidity curve to track an external reference price, with fee revenue accruing to LPs in proportion to their share of active liquidity at the current price. Question: characterize the (Markov perfect / stationary) equilibria of this game — existence, uniqueness, the induced **shape of aggregate liquidity** (is it concentrated, fat-tailed, "delta-neutral" around the reference price?), and equilibrium LP returns (fee yield net of gas). The census record flags three essential features that prior work omits: (i) repeated/dynamic play, (ii) heterogeneity of LPs, (iii) endogenous fee sharing and price determination rather than an exogenous price process. Ambiguity: the exact price process and information structure are not specified; the solver must fix a precise model and may note results are model-dependent.

## Why does it matter?

This would explain empirically observed phenomena — heavy liquidity concentration in narrow bands around the current price, sharp LP underperformance relative to static strategies, and fee-revenue inequality — as equilibrium outcomes rather than behavioral puzzles. It would let protocols design fee tiers, gas refunds, and incentive programs with grounded predictions about resulting liquidity depth. Decentralized exchanges handle billions in daily volume, so even partial characterizations (e.g., conditions under which equilibrium liquidity is efficient, or a price-of-anarchy bound on depth) have immediate design consequences.

## What's already known?

- **Single-LP optimization:** The optimal range choice of one LP against an exogenous price process (e.g., Brownian motion) is studied for Uniswap v3-style contracts; e.g., fan et al. (2023) compute best responses and empirically document strategic concentration; follow-up work (2021–2024) covers LVR / loss-versus-rebalancing as the benchmark LP loss with exogenous prices (Milionis et al. 2022, "Automated Market Making and Arbitrage Profits in the Presence of Fees" — verify exact framing).
- **Static spatial-allocation games:** One-shot games where LPs simultaneously pick ranges against an exogenous price distribution have equilibrium characterizations via Hotelling-style arguments.
- **Why it falls short:** All of this treats prices as exogenous, the game as static, and LPs as homogeneous or solitary. The dynamic game with switching costs, endogenous fee division (fee share depends on *others'* placements), and price impact via arbitrage is open. Related literature on stochastic games with switching costs (e.g., "games of strategic complements with adjustment costs") provides existence tools but not structure for this continuous-allocation setting.

## What would count as solving it?

1. **Existence/structure theorem:** For a precisely specified model (discrete price grid, Markov price process with arbitrage closure, gas cost c > 0, heterogeneous budgets), prove existence of a stationary equilibrium and characterize aggregate liquidity shape (e.g., prove concentration near the reference price, or give a counterexample where it disperses).
2. **Comparative statics:** Theorem on how gas costs c and heterogeneity shift equilibrium concentration and net LP returns — e.g., returns decreasing in c, or an "arms race" result where all LPs burn gas without changing fee shares.
3. **Efficiency/incompetence bound:** Price-of-anarchy style bound on liquidity depth or welfare vs. the planner solution.
4. **Matching counterexample/lower bound:** Construction showing non-existence, multiplicity, or inefficiency in a natural parameter regime.
A referee accepts: a fully specified model, rigorous proofs or clean counterexamples, and validation of predictions against observed on-chain liquidity profiles.

## How to start

1. Read Fan et al. (2023) first — it gives the fee-sharing mechanics, LP utilities, and empirical regularities your model must reproduce.
2. Start with the **two-player, three-bin price grid** model: ranges ∈ {left, center, right}, exogenous two-state Markov price, fixed gas cost. This finite stochastic game is analyzable and already captures the re-positioning incentive; look for the stationary equilibrium numerically (value iteration on best responses), then prove structural properties.
3. Main obstacle: the fee-sharing term makes payoffs depend on *ratios of others' allocations*, so the game is neither potential nor supermodular in general — standard existence tools may fail; expect to work with discretization/continuity arguments or best-response dynamics.
4. Next, endogenize the price via an arbitrageur best response (one-shot per period), and check robustness of the small-model conclusions.

## Key references

- Fan, Marmolejo-Cossío, Moroz, Neuder, Rao, Parkes (2023), "Strategic Liquidity Provision in Uniswap v3," AFT 2023, DOI: 10.4230/LIPIcs.AFT.2023.25. — Computes optimal LP range strategies under price uncertainty and empirically shows strategic liquidity placement in Uniswap v3; the mechanical foundation for any model here.
- Milionis, Moallemi, Roughgarden, Zhang (2022/2023), work on LVR / "loss-versus-rebalancing" for AMM LPs (verify exact citation, arXiv 2208.14350). — Establishes the benchmark adversarial/exogenous-price loss of LPs and fee revenue decomposition.
- (verify) Literature on stochastic games with switching costs, e.g.,dynamic oligopoly models — supplies existence techniques for repeated games with adjustment costs.

## Warnings

- The census record does not fix the price process or information structure; results are highly model-sensitive — state your assumptions up front and test robustness.
- Do not treat LP fee revenue as exogenous or independent of others' placements; in Uniswap v3 fees are shared among LPs active at the current price, making the game genuinely strategic each period.
- A common dead end: continuous price space + simultaneous moves often breaks existence of pure equilibria (Hotelling-style non-existence); discretize first.
- Beware "solving" only the single-LP or exogenous-price case — that is precisely the 2021–2024 literature this problem asks to go beyond.

---
*`BCGT-000352` · flagged for review*

## Acceptance guidance

1. **Existence/structure theorem:** For a precisely specified model (discrete price grid, Markov price process with arbitrage closure, gas cost c > 0, heterogeneous budgets), prove existence of a stationary equilibrium and characterize aggregate liquidity shape (e.g., prove concentration near the reference price, or give a counterexample where it disperses).
2. **Comparative statics:** Theorem on how gas costs c and heterogeneity shift equilibrium concentration and net LP returns — e.g., returns decreasing in c, or an "arms race" result where all LPs burn gas without changing fee shares.
3. **Efficiency/incompetence bound:** Price-of-anarchy style bound on liquidity depth or welfare vs. the planner solution.
4. **Matching counterexample/lower bound:** Construction showing non-existence, multiplicity, or inefficiency in a natural parameter regime.
A referee accepts: a fully specified model, rigorous proofs or clean counterexamples, and validation of predictions against observed on-chain liquidity profiles.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/a2dcf800-e889-4185-aca7-0f800caa1a6d?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: BCGT-000352.

- [Public source catalog](https://drewstone.github.io/problems/#/p/BCGT-000352)
