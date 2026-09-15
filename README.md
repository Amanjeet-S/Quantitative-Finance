# Quantitative Finance: Theory, Computation and Model Validation

A research portfolio exploring how mathematical models and computational methods inform financial and economic decisions, and how uncertainty in their assumptions, inputs and numerical solutions affects the answers.

The work spans option exercise, investment allocation and the transmission of monetary policy to production. Each investigation will combine a focused research question, mathematical or econometric foundations, reusable software and evidence that assesses the reliability and economic significance of the results.

**Status: research in progress.** Project 1 is in Stage 1, foundations and data feasibility. A Python prototype, mathematical notes and preliminary numerical checks are available. An official Treasury data snapshot has been acquired; the option-market dataset and empirical study remain under development. Project 2 is planned. Project 3 now focuses on ECB monetary-policy shocks and German consumer-goods production. An initial data-access and coverage check is complete, but its empirical pipeline and results are not yet available. No project is presented as a completed research contribution.

Start with the [Project 1 overview](projects/01_optimal_stopping/README.md), its [staged research design](projects/01_optimal_stopping/research_design.md) and the [observed-data plan](projects/01_optimal_stopping/data_plan.md). Setup and reproduction instructions are in the project overview.

The [Germany research overview](#3-monetary-policy-and-consumer-goods-production-in-germany) below describes the economics question, accessible datasets and planned empirical work.

## What this repository investigates

Financial and economic research connects assumptions and observations to decisions and outcomes: when to exercise an option, how to allocate a portfolio, and how an unexpected monetary-policy change affects production. The conclusions depend on several choices:

- **Economic assumptions:** preferences, incentives, constraints and the information available to decision-makers.
- **Financial modelling:** contract terms, risk exposures, market frictions and the distinction between valuation and forecasting.
- **Mathematical methods:** probability, analysis, linear algebra, differential equations, statistics and optimisation.
- **Econometric identification:** the variation used to estimate an effect, the assumptions supporting a causal interpretation and the treatment of competing explanations.
- **Computational methods:** algorithms, data representations, numerical approximations and the allocation of computational resources.

The common research question is how these choices affect the conclusions. A method will be assessed for the accuracy of its implementation and the credibility of the financial or economic interpretation it supports.

Python is the primary implementation language. Project 1 includes a planned C++ computational study, with a readable Python reference and accuracy checks before performance comparisons. Project 3 will use Python for data processing and estimation, with selected estimates independently checked in R.

## Planned project portfolio

| Research direction | Central question | Main foundations |
| --- | --- | --- |
| **Optimal stopping and option pricing** | How do numerical approximations affect option values and exercise decisions? | Conditional expectation, martingales, stochastic calculus, PDEs and numerical analysis |
| **Robust portfolio optimisation** | When does protection against estimation uncertainty improve investment decisions? | Matrix analysis, convex optimisation, duality, statistical estimation and decision theory |
| **Monetary policy and German production** | How does an unexpected ECB monetary tightening affect durable consumer-goods production relative to nondurable consumer-goods production in Germany? | Monetary economics, time-series econometrics, identification using financial-market surprises, local projections and statistical inference |

Two possible later extensions will investigate **stochastic-volatility inference** and **option-model calibration and identifiability**. Their scope is outlined below.

## 1. Optimal stopping and option pricing

**Research question:** How much option value is lost through approximate exercise decisions, and which computational improvements reduce that loss most efficiently?

The intended empirical study will fit models to observed market conditions and assess whether improvements matter relative to option value and bid–ask spreads. The existing simulated checks serve numerical validation only. [Stage completion criteria](projects/01_optimal_stopping/research_design.md) separate this preliminary work from the eventual research conclusions.

An early-exercise option requires a decision between exercising now and preserving the opportunity to exercise later. Computing its value therefore involves estimating future outcomes and solving an optimal-stopping problem using only the information available at each decision date.

The project will begin with European-option benchmarks and then investigate early exercise through trees, finite differences and least-squares Monte Carlo. It will distinguish errors caused by sampling, discretisation, regression and differences in the exercise opportunities being modelled.

### Mathematical and computational work

- Derive European pricing through replication and risk-neutral expectation.
- Define the information structure, admissible stopping times and discrete-time Snell-envelope recursion.
- Prove the conditional-expectation projection property for square-integrable payoffs and connect it to continuation-value regression.
- Implement exact geometric-Brownian-motion simulation, analytical prices and Greeks, trees and finite-difference solvers.
- Implement regression-based exercise policies with separate training and evaluation paths.
- Examine numerical consistency, stability, regression conditioning and convergence under refinement.

### Experiments and intended outputs

Comparisons will use matching contract terms and exercise schedules. Experiments will vary grids, sample sizes, regression bases and exercise frequency, then measure both price error and the value of the resulting exercise policies.

Validation will include analytical benchmarks, applicable price bounds, European put–call parity, independent policy evaluation and repeated simulation runs. Sampling intervals will be distinguished from uncertainty caused by numerical approximation. Payoff regularity will be considered when interpreting convergence rates.

The intended outputs are a mathematical report, exercise-boundary figures, policy-value comparisons and tables of accuracy against computational cost. Martingale dual bounds are a possible later extension.

**Starting reference:** [Longstaff and Schwartz, *Valuing American Options by Simulation: A Simple Least-Squares Approach*](https://www.anderson.ucla.edu/documents/areas/fac/finance/file11.pdf).

## 2. Robust portfolio optimisation under estimation uncertainty

**Research question:** When is protection against uncertain expected returns and imperfect covariance estimates worth its opportunity cost?

Portfolio optimisation can translate small changes in estimated inputs into large changes in investment weights. This project will investigate the mechanisms behind that sensitivity and assess when constraints, regularisation and explicit uncertainty sets improve the resulting decisions.

### Mathematical and computational work

- Derive minimum-variance and mean–variance portfolio problems with explicit feasible sets.
- Analyse covariance eigenvalues, conditioning and the effects of regularisation.
- Establish existence and optimality conditions, distinguishing convexity from strict convexity and identifying conditions for uniqueness.
- Derive a robust portfolio objective from an ellipsoidal uncertainty set for expected returns.
- Explain the construction of the uncertainty set and how its size is selected.
- Implement an algorithm for a manageable convex subproblem and compare it with an established optimisation solver.

Uncertainty about expected returns, covariance regularisation and turnover preferences will be treated as distinct modelling choices. A turnover penalty in the objective will also be distinguished from the transaction costs charged during investment evaluation.

### Experiments and intended outputs

Synthetic experiments will vary asset dimension, sample length, correlation structure and input perturbations. Known generating parameters will provide an oracle benchmark for assessing estimation effects.

Historical-data experiments will use chronological estimation, validation and evaluation periods. Results will include feasibility and optimality diagnostics, weight sensitivity, concentration, turnover, realised risk and performance after stated transaction costs. Solver accuracy and investment performance will be evaluated separately.

The intended outputs are derivations, spectral diagnostics, solver comparisons and a research report identifying conditions under which robust decisions improve or deteriorate. Expected-shortfall optimisation is a possible extension if it introduces a distinct research question.

## 3. Monetary policy and consumer-goods production in Germany

**Research question:** How does an unexpected ECB monetary tightening affect durable consumer-goods production relative to nondurable consumer-goods production in Germany?

The project will investigate whether the ability to postpone durable purchases is reflected in different production responses to monetary-policy shocks. This is a hypothesis to assess. German production also serves foreign demand and responds to supply conditions, so the estimates will not, by themselves, isolate household spending or a single transmission mechanism.

The central outcome is the difference between the two production responses at a pre-specified horizon, supported by estimated response paths. The study will estimate and test this difference directly, accounting for dependence between the two series. Statistical significance in one series and its absence in the other will not be treated as evidence that their responses differ.

### Data and sample

An initial check on **15 September 2026** verified public downloads of the following series without a subscription or institutional login:

| Input | Source | Verified coverage |
| --- | --- | --- |
| German durable and nondurable consumer-goods production | [Eurostat industrial production, `sts_inpr_m`](https://ec.europa.eu/eurostat/databrowser/view/sts_inpr_m/default/table?lang=en) | January 1991 to July 2026; 427 monthly observations in each series, with no gaps within this interval |
| ECB monetary-policy and central-bank information shocks | [Jarociński's author-maintained update](https://github.com/marekjarocinski/jkshocks_update_ecb) | January 1999 to October 2025; 312 events, supplied as 322 monthly observations |
| German all-items Harmonised Index of Consumer Prices | [Eurostat historical HICP series, `prc_hicp_midx`](https://ec.europa.eu/eurostat/en/web/products-datasets/-/PRC_HICP_MIDX) | January 1996 to December 2025; 360 monthly observations |

Production uses the seasonally and calendar adjusted volume indices, with 2021=100, for `MIG_DCOG` and `MIG_NDCOG`. These are consumer-goods production groups. Their classification differs from the broader US durable and nondurable manufacturing split.

The production and shock series overlap for **322 months, January 1999 to October 2025**, before adjustments for lags, response horizons, controls and exclusions. This establishes data availability, not statistical power. The final estimation sample will be documented for each specification. More recent production observations do not extend the shock series beyond its verified coverage.

The shock data are an update of a published research method, rather than the original journal replication vintage. Raw announcement-window observations are also available in the [ECB's Euro Area Monetary Policy Event-Study Database](https://www.ecb.europa.eu/pub/pdf/annex/Dataset_EA-MPD.xlsx). Reconciling event coverage and documenting the construction of the derived shocks remain part of the data audit.

### Econometric and computational work

- Develop the economic argument and state the assumptions required to interpret announcement-window surprises as policy shocks.
- Explain the distinction between monetary-policy shocks and information about the economic outlook conveyed by the central bank.
- Build a reproducible Python pipeline for acquisition, date alignment, missing-value checks, transformations and sample construction.
- Specify local projections for the production responses and their difference, with a primary horizon and a compact lag structure chosen before examining the main results.
- Use uncertainty estimates appropriate to serial dependence and overlapping response horizons, and distinguish pointwise intervals from inference across a response path.
- Examine sensitivity to lag choices, shock decomposition, crisis periods and influential announcements, reporting material changes in the conclusions.
- Check selected estimates independently in R using the same observations, transformations and inference conventions.

An extension using a fixed-composition euro-area aggregate may assess whether the German pattern is also visible at the monetary-union level. It will be treated as a separate geographical comparison, not as independent replication of the same policy shocks.

### Intended outputs

The project will produce a documented analysis dataset, reusable estimation code, an executed Jupyter notebook, response figures, robustness tables and an economics research paper. A concise summary will retain the same research question, principal estimates and limitations as the full paper.

The report will connect the estimates to monetary transmission and German production, discuss competing explanations, and distinguish causal assumptions from observed patterns. Imprecise estimates and results that do not support the initial hypothesis will be reported. No empirical findings are claimed at this stage.

**Published foundations:**

- Altavilla, C., Brugnolini, L., Gürkaynak, R. S., Motto, R. and Ragusa, G. (2019). [*Measuring euro area monetary policy*](https://doi.org/10.1016/j.jmoneco.2019.08.016). *Journal of Monetary Economics*, 108, 162–179.
- Jarociński, M. and Karadi, P. (2020). [*Deconstructing Monetary Policy Surprises: The Role of Information Shocks*](https://doi.org/10.1257/mac.20180090). *American Economic Journal: Macroeconomics*, 12(2), 1–43.

## Possible later extensions

### Stochastic-volatility inference and predictive uncertainty

This project would investigate when latent-volatility models improve financial predictive distributions and risk decisions. It would develop filtering from linear Gaussian benchmarks to a stochastic-volatility model, with a bootstrap particle filter and a documented parameter-estimation method.

The investigation would cover filtering versus smoothing, stable importance weights, particle degeneracy, resampling and the distinction between latent-state and parameter uncertainty. Validation would begin with synthetic recovery and comparisons against a Kalman solution in a separate linear Gaussian benchmark, followed by chronological predictive evaluation.

Predictive accuracy and the quality of any risk-management or allocation decision would be reported separately. Particle MCMC would be an advanced extension after the filtering implementation is validated.

**Starting reference:** [Andrieu, Doucet and Holenstein, *Particle Markov Chain Monte Carlo Methods*](https://www.stats.ox.ac.uk/~doucet/andrieu_doucet_holenstein_PMCMC.pdf).

### Volatility calibration and identifiability

This project would ask whether parameter sets that fit option prices almost equally well imply materially different Greeks or values for other contracts.

It would combine Heston characteristic-function pricing, Fourier inversion, numerical quadrature and constrained nonlinear calibration. Model-generated surfaces would serve parameter-recovery tests only. The principal investigation would use observed option quotes, multiple-start optimisation, Jacobian singular-value diagnostics and sensitivity to documented quote uncertainty.

The analysis would separate integration error, optimisation error, weak identification and model misspecification. Historical-return inference and risk-neutral option calibration would retain their distinct probability measures and model specifications.

## What a completed project will include

| Deliverable | Purpose |
| --- | --- |
| **Project overview** | Explain the question, contribution, principal findings and limitations |
| **Research report** | Present assumptions, derivations, algorithms, experiments and interpretation |
| **Reusable source code** | Implement the central models and numerical methods |
| **Jupyter notebooks** | Explain the analysis and reproduce selected figures and tables using the source package |
| **Experiment configurations** | Record parameters, seeds, tolerances and evaluation choices |
| **Validation and benchmarks** | Establish correctness and assess accuracy, stability and computational cost |
| **Generated figures and tables** | Make the findings inspectable and reproducible |
| **Data and source documentation** | Record provenance, transformations, permissions and attribution |

Reusable algorithms live in the source package. The first implementation includes documented command-line experiments and generated validation records. Planned explanatory notebooks will import the reusable implementation and be executed from a fresh kernel before release. Notebooks are not yet included in the current repository.

## Reproducibility and research standards

Every released experiment will record its assumptions, configuration, random-number handling, numerical tolerances and dependency versions. Performance comparisons will identify the hardware and the operations being timed, and compare methods at stated levels of accuracy.

Analytical solutions, independent implementations and known generating parameters will provide checks wherever possible. Repeated runs will assess simulation variability. Empirical work will document information timing, data transformations and chronological evaluation, with uncertainty estimates appropriate to the dependence in the observations.

Reports will distinguish established theory, numerical evidence and empirical interpretation. Mathematical arguments will state their assumptions. Sources, adapted arguments, reused code and original contributions will be identified explicitly. Public datasets will be accompanied by provenance and redistribution information.

Literature reviews will prioritise published, peer-reviewed papers. Working papers will be identified as such and used where they address a relevant gap. Source records will identify the version actually consulted and distinguish published methods from subsequent data updates.

Empirical claims must use observed, traceable data. Simulation and analytically specified fixtures are labelled as mathematical validation and never passed off as market observations. Current retrieval dates and historical observation dates are recorded separately. See the [provenance record](PROVENANCE.md).

Each project will be released when its central question has been answered with reproducible evidence and its limitations have been documented. Optional extensions will have separate milestones so that a sound core investigation can be completed and presented independently.
