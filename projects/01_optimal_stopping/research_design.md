# Research design and staged completion criteria

**Status: Stage 1 in progress.** The existing Python solvers and numerical checks are a prototype. There is no completed empirical study, established novel result or claim of readiness for research publication.

## The investigation

**Working question:** Under models fitted to observed option-market conditions, how much value is lost through approximate exercise policies, how reliably can that loss be measured, and when is greater numerical accuracy economically material?

The central object is the gap between a feasible policy's expected discounted payoff and the optimal value under the same model and exercise rights. Market quotes provide calibration inputs and a scale for economic relevance. They do not reveal the true optimal model value directly.

The study will connect four forms of reasoning:

- **Mathematics:** conditional expectation, stopping times, Snell envelopes, approximation error, integrability and valid uncertainty bounds.
- **Computation:** stable regression, PDE discretisation, algorithmic complexity, reproducible experiments and a verified C++ implementation of a selected numerical kernel.
- **Finance:** contract conventions, discounting, dividends, calibration, exercise rights and bid–ask spreads.
- **Economics:** the opportunity cost of waiting, the value of additional feasible decisions, and the cost of computation relative to the decision improvement it produces.

A collection of implementations alone does not answer the research question. The final contribution must be a documented finding about policy loss or the allocation of computational effort that survives the checks below.

## Evidence and data policy

The empirical study must use observed, traceable information. Option quotes, underlying prices, dividends, corporate actions and funding inputs must carry provider identifiers, observation timestamps, retrieval dates, units and transformation records. Missing observations remain missing or are excluded under a documented rule. Invented quotes, dates, dividends or market histories are unacceptable.

Mathematical test fixtures and stochastic simulation have a narrower purpose. Analytical identities need specified test inputs, and Monte Carlo necessarily generates random draws from a stated model. Such outputs are labelled as numerical validation and never presented as market observations, calibration evidence or empirical findings. The current baseline uses this test-fixture category only.

The option-quote candidate is Cboe DataShop's Option EOD Summary. OptionMetrics through an institutional subscription is an alternative if access becomes available. Capital IQ Pro is retained for underlying prices, dividends and corporate actions; the account holder has clarified that it does not provide the required exchange-traded option chain. A downloaded Cboe demonstration archive supports a format audit only. No recent empirical option panel has been acquired. [data_plan.md](data_plan.md) specifies the source checks and required fields.

“Latest” means the latest verified observation available from the source at the recorded retrieval time. The most recent incomplete trading session will not silently replace a completed-session snapshot. A current data download does not reconstruct what investors knew on each historical date.

## Stage 1: foundations and a data feasibility audit

**Current work**

- [x] Implement European prices and Greeks, exact GBM simulation and explicit exercise calendars.
- [x] Add prototype tree, PDE and regression-policy solvers.
- [x] Test financial identities and verify evaluation-time information restrictions.
- [x] Draft the model, projection argument and finite-calendar stopping proof.
- [x] Identify the official Treasury feed and the required option-quote fields.
- [ ] Review the derivations and numerical conventions against their stated assumptions.
- [x] Inspect the Cboe demonstration archive and identify documented field conventions and gaps.
- [ ] Inspect a recent option-quote extract and the required underlying and corporate-action exports.
- [ ] Confirm option bid–ask history, corporate-action treatment and publication rights for derived outputs.
- [ ] Freeze an empirical question and feasible sample after the data audit.

**Completion criterion:** a reviewed specification and a real, documented sample that contains the fields needed by the intended empirical test. The Treasury series alone does not satisfy this criterion.

## Stage 2: verified numerical foundations

Isolate spatial, temporal and domain errors instead of changing all grids together. Test off-grid spots and strikes, negative rates, dividends, low volatility, short maturities and deep exercise regions. Inspect price and Greek oscillations, conditioning, rank reduction and extrapolation. Demonstrate that adding nested exercise rights preserves exact-model ordering up to quantified numerical error.

For regression, compare suitably normalised polynomial and orthogonal bases, including failure cases. Distinguish conditional-mean approximation from exercise-decision loss. Use a literature review to select the experiments; increasing polynomial degree alone is not a research contribution.

**Completion criterion:** convergence evidence across a declared stress set, explained failures and tolerances supported by the application. Passing the initial tests is necessary but insufficient.

## Stage 3: observed contracts and calibration

Choose a small liquid universe after verifying the data. US equity or ETF puts are a provisional candidate. Record the actual exercise style, settlement, multiplier, calendar and dividends for every contract used. A monthly Bermudan approximation must not be treated as the listed American contract without an exercise-refinement study.

Construct discount factors under an explicit funding convention. Treasury par yields cannot be inserted directly as continuously compounded zero rates. Model discrete cash dividends where the contract requires them, with announcement information appropriate to the valuation date.

Fit a parsimonious baseline using training quotes, report bid–ask-aware residuals, and evaluate on held-out dates or contracts according to the research question. A volatility estimated from historical returns is a physical-measure quantity and is not automatically a risk-neutral calibration.

**Completion criterion:** a reproducible calibration pipeline with quote exclusions, timing checks, diagnostics and a held-out evaluation sample.

## Stage 4: measuring policy loss

Evaluate frozen policies on independent paths under the calibrated model. Use common evaluation paths for paired policy comparisons, while retaining the resulting dependence in inference. Repeat training sufficiently to distinguish training variability from evaluation noise.

Develop and verify a martingale-dual construction if it yields informative upper bounds. Account for the sampling error of both sides. A numerical PDE reference and a noisy feasible-policy mean are not by themselves a certified optimality gap. Bound width, simulation budgets and computational costs are research outcomes.

Investigate whether computational effort is best allocated to more training paths, richer bases, finer exercise calendars or improved continuation targets. Selection and reporting must use separate data or an explicitly exploratory protocol.

**Completion criterion:** defensible estimates or bounds on policy loss, with uncertainty and sensitivity sufficient to support a precise conclusion. If bounds remain uninformative, that limitation is reported.

## Stage 5: Python and C++ computational study

Python remains responsible for data validation, model specification, calibration, experiment orchestration, statistics and figures. After profiling, implement a substantive bottleneck in C++, such as backward tree induction or a tridiagonal stepping kernel.

Maintain a readable Python reference. Test C++ parity across the same contracts and stress cases before reporting performance. Record compiler, optimisation flags, numerical precision, thread count and hardware. Separate compilation, data transfer and computation; report repeated runtime distributions and peak memory. Check scaling with paths, dates and basis size.

**Completion criterion:** verified cross-language results and an explained accuracy–cost comparison. The C++ component must answer a computational question, not merely add a second language to the repository.

## Stage 6: economic interpretation and research paper

Relate policy loss to option value, early-exercise premium, bid–ask spread and computational budget. Analyse how funding and dividend assumptions affect timing incentives. Describe model-price discrepancies as joint effects of modelling, inputs and measurement unless a design can identify the separate causes.

If a hedging extension is undertaken, distinguish risk-neutral pricing from physical-measure or historical evaluation, use executable-price assumptions and record transaction costs and information timing.

Write a paper with a focused question, literature positioning, assumptions, derivations, reproducible evidence, limitations and an appendix sufficient to reproduce the work. Establish novelty only after reviewing the relevant literature and comparing against credible existing methods.

**Completion criterion:** every principal claim is traceable to a derivation, an attributed source or a reproducible experiment; the conclusion survives documented robustness checks and the final repository reproduces from a clean environment.

## Working practice

Complete and review one stage before treating its outputs as foundations for the next. Maintain a research log recording hypotheses before runs, revisions to the design, failures, decisions and unresolved questions. Do not infer competence from the number of algorithms or languages used. The quality of the argument, implementation and evidence is what the project must demonstrate.
