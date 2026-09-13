# Quantitative Finance: Theory, Computation and Model Validation

A collection of five planned research projects on pricing financial contracts, allocating investments, forecasting risk and modelling household financial decisions. Each project will combine mathematical explanations, reusable code and experiments that assess whether the results can be trusted.

Financial models depend on assumptions, estimated inputs and numerical approximations. This repository will investigate how those choices affect the answers a model gives: the price of an option, the composition of a portfolio, a forecast of risk or a household's decision to change banks. The common aim is to understand both how a method works and the circumstances in which it becomes unreliable.

The implementations will primarily use Python, with selected C++ extensions. The mathematical work will connect probability, analysis, linear algebra, differential equations and optimisation to the questions being investigated.

**Status: project specification.** This README describes the intended repository. The implementations, experiments, mathematical reports and results below are planned; their inclusion here does not indicate that they have been completed or validated. No empirical findings or performance improvements are claimed at this stage.

## Start here

This overview is for readers encountering the repository for the first time. Each project section explains its question, the mathematics it will use, what will be implemented and how the work will be assessed. The detailed mathematical sections assume familiarity with undergraduate calculus, linear algebra and basic probability; specialist concepts will be developed in the project reports.

- For a quick overview, read the project table below.
- To understand the research scope, read one of the five numbered project descriptions.
- To see the proposed code, reports and supporting files, read [Intended repository contents](#intended-repository-contents).
- To understand how results will be checked and reproduced, read [Reproducibility and validation standards](#reproducibility-and-validation-standards).
- To see what remains to be built, read [Development milestones](#development-milestones).

A few terms used throughout the overview:

| Term | Meaning in this repository |
| --- | --- |
| Option | A contract giving its holder a right to buy or sell an asset on specified terms |
| Optimal stopping | Deciding when to take an action, such as exercising an option, using only information available at that time |
| Robust optimisation | Choosing a decision while explicitly allowing for uncertainty in model inputs |
| Stochastic volatility | A model in which the variability of returns changes randomly over time |
| Filtering | Estimating an unobserved current state, such as volatility, from observations available so far |
| Calibration | Selecting model parameters to fit observed or generated prices |
| Identifiability | Whether the available observations distinguish one parameter choice from another |

All five projects are described within this README. The proposed code directories and project-specific reports have not yet been created.

## Project portfolio

| Project | Central question | Main mathematical foundations | Role |
| --- | --- | --- | --- |
| 01. Optimal stopping and option pricing | Which approximations explain disagreement between pricing methods and exercise policies? | Conditional expectation, martingales, stochastic calculus, PDEs and numerical analysis | First flagship |
| 02. Robust portfolio optimisation | How do errors in estimated inputs change portfolio decisions, and when does robustness help? | Matrix analysis, convexity, constrained optimisation, duality and statistical estimation | Second flagship |
| 03. Stochastic-volatility inference | When do latent-volatility models produce reliable predictive distributions? | Bayesian inference, state-space models, Monte Carlo and time-series statistics | Further statistical depth |
| 04. Household liquidity and bank switching | How do income risk, liquidity constraints and switching costs affect household choices? | Dynamic programming, fixed points, Markov chains and economic modelling | Independent economics research |
| 05. Volatility calibration and identifiability | Can similar option-price fits imply materially different parameters and risks? | Fourier methods, multivariable calculus, inverse problems and nonlinear optimisation | Derivatives extension |

The first two projects establish complementary foundations. Project 03 adds computational statistics. Project 04 develops a separate household-finance question. Project 05 will reuse validated pricing components where appropriate. The numbering sets the presentation order; the later projects are not prerequisites for completing the first two.

## Research approach

Each project will follow the same sequence:

1. Formulate a precise financial or economic question and define the model, information set and assumptions.
2. Derive the relevant mathematical relationships and identify the results that require proof.
3. Implement a transparent baseline with an analytical or independently computable benchmark.
4. Design controlled experiments that isolate approximation, estimation and model error.
5. Introduce one substantive extension and assess it against the baseline.
6. Publish the code, derivations, reproducible experiments, measured findings and limitations together.

The portfolio will distinguish theoretical results, numerical evidence and empirical interpretation. Simulations can investigate the implications of a theorem or reveal a failure mode; they cannot establish a theorem by themselves.

## 01. Optimal stopping and option pricing

**Research question:** how much of the difference between option-pricing estimates comes from sampling uncertainty, numerical discretisation, regression approximation or the exercise opportunities being modelled?

### Mathematical content

The mathematical report will define the probability space, filtration, pricing measure and admissible stopping times. It will derive European pricing through replication and risk-neutral expectation, and develop the discrete-time Snell-envelope recursion for early exercise.

It will include a proof of the conditional-expectation projection property. For a square-integrable payoff variable $Y$, under the stated pricing measure,

$$
\mathbb{E}[Y\mid\mathcal{F}_t]
=\underset{Z\in L^2(\mathcal{F}_t)}{\arg\min}\;\mathbb{E}[(Y-Z)^2].
$$

The report will connect this result to regression estimates of continuation values, distinguish projection onto the full information space from approximation in a finite basis, and explain the assumptions behind change of measure and optional stopping. It will derive the Black–Scholes PDE and examine finite-difference consistency, stability and convergence under appropriate conditions.

### Planned implementation

- Exact geometric-Brownian-motion simulation and analytical European option prices and Greeks.
- A binomial tree with explicitly controlled exercise dates.
- European finite-difference solvers, followed by an American-put solver with the exercise constraint.
- Least-squares Monte Carlo for Bermudan options, with separate training and policy-evaluation paths.
- Diagnostics for regression conditioning, exercise decisions, numerical error and runtime.
- A selected C++ implementation of a computational bottleneck after profiling the Python version.

### Experiments and validation

Experiments will vary spatial grids, time steps, exercise dates, sample sizes and regression bases. Comparisons will match the exercise schedule before attributing differences to numerical methods. Further experiments will refine the exercise schedule to investigate the American limit.

Validation will cover analytical European benchmarks, price bounds, put–call parity for European contracts, independent exercise-policy evaluation and convergence under refinement. Monte Carlo intervals will describe sampling uncertainty; they will not be presented as covering all sources of approximation error. Smoothness assumptions and payoff kinks will be considered when interpreting observed convergence rates.

### Intended outputs

A mathematical report, a benchmark notebook, tables of error against computational cost, exercise-boundary figures, regression-sensitivity experiments and a documented comparison of Python and C++ results. An optional later extension will investigate martingale dual bounds, with the distinction between theoretical bounds and noisy numerical estimates made explicit.

**Methodological starting point:** Longstaff and Schwartz, [Valuing American Options by Simulation: A Simple Least-Squares Approach](https://www.anderson.ucla.edu/documents/areas/fac/finance/file11.pdf).

## 02. Robust portfolio optimisation under estimation uncertainty

**Research question:** when do small errors in estimated expected returns and covariances produce large changes in portfolio weights, and which forms of regularisation or robustness improve the resulting decisions?

### Mathematical content

The report will derive minimum-variance and mean–variance formulations, explain covariance eigenvalues and conditioning, and establish existence and optimality conditions for the chosen constrained problems. It will distinguish convexity from strict convexity and identify when a unique optimiser is guaranteed.

A central extension will model uncertainty in expected returns through

$$
\mathcal{U}_{\mu}=\{\widehat{\mu}+Au:\|u\|_2\leq\rho\},
$$

where $A$ specifies the geometry of uncertainty and $\rho\geq0$ its size. For risk aversion $\gamma>0$, a positive-semidefinite covariance estimate $\widehat{\Sigma}$ and turnover penalty $\lambda\geq0$, the corresponding robust objective will be derived as

$$
\min_{w\in\mathcal{W}}
\left\{
\frac{\gamma}{2}w^\top\widehat{\Sigma}w
-\widehat{\mu}^{\top}w
+\rho\|A^\top w\|_2
+\lambda\|w-w_{\mathrm{prev}}\|_1
\right\}.
$$

The feasible set $\mathcal{W}$ will be explicitly specified and nonempty, initially using fully invested, long-only weights. The derivation will show why the uncertainty set yields the norm penalty and state which inputs remain treated as fixed. The turnover penalty will be distinguished from the transaction-cost model used in performance evaluation.

### Planned implementation

- Sample and regularised covariance estimators with spectral diagnostics.
- Baseline equal-weight, minimum-variance and constrained mean–variance portfolios.
- The robust formulation, with configurable uncertainty and turnover penalties.
- An independently implemented algorithm for a manageable convex subproblem, checked against an established optimisation solver.
- Chronological estimation and evaluation with explicit rebalancing and information timing.
- A later expected-shortfall formulation if it adds a distinct research question.

### Experiments and validation

Synthetic experiments will vary asset dimension, sample length, correlation structure, input perturbations and model misspecification. Known generating parameters will allow comparison with an oracle benchmark that is unavailable to the estimated strategies.

Validation will examine feasibility, stationarity or subgradient conditions, and duality gaps where available. Penalty selection will use training and validation periods that precede evaluation. Results will report weight sensitivity, concentration, turnover, realised risk and transaction costs. Dependence-aware uncertainty estimates will be used where appropriate.

### Intended outputs

Derivations and optimality conditions, covariance-conditioning diagnostics, solver comparisons, sensitivity figures, chronological evaluation tables and a report explaining when robust decisions improve or deteriorate. Solver accuracy and investment performance will be assessed separately.

**Reference for the optional tail-risk extension:** Rockafellar and Uryasev, [Conditional Value-at-Risk for General Loss Distributions](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=267256).

## 03. Stochastic-volatility inference and predictive uncertainty

**Research question:** when does accounting for latent volatility and parameter uncertainty improve financial forecasts, and when does model misspecification undermine those forecasts?

### Mathematical content

The baseline model will be

$$
r_t=e^{h_t/2}\varepsilon_t,\qquad
h_t=\mu+\phi(h_{t-1}-\mu)+\sigma_\eta\eta_t,
$$

with $|\phi|<1$, $\sigma_\eta>0$ and independent standard-normal innovation sequences in the initial specification. Its observation equation assumes zero conditional mean; the treatment of empirical return means will be documented and estimated without future information.

The report will derive the Bayesian prediction and update recursions, the stationary distribution of the latent Gaussian AR(1), importance weights and the predictive distribution. It will distinguish filtering, smoothing and forecasting, and explain particle degeneracy and the purpose of resampling.

### Planned implementation

- A synthetic state-space data generator with known parameters and latent states.
- A Kalman filter for a separate linear Gaussian benchmark.
- A bootstrap particle filter with stable log-weight calculations and resampling diagnostics.
- Known-parameter volatility filtering, followed by a documented parameter-estimation method.
- Predictive evaluation against constant-volatility, exponentially weighted variance and GARCH benchmarks.
- Optional particle marginal Metropolis–Hastings after the filtering implementation has been validated.

### Experiments and validation

The particle filter will first be tested in the linear Gaussian benchmark against the Kalman solution. Volatility experiments will examine state recovery, sensitivity to particle count, parameter recovery and predictive coverage on generated data.

Chronological empirical evaluation will use a documented public return series. Heavy-tailed synthetic experiments will test a deliberate departure from the Gaussian assumptions. Evaluation will include predictive scores, interval coverage and tail-forecast diagnostics, with uncertainty and dependence considered.

If particle MCMC is implemented, the report will explain the role of a non-negative unbiased likelihood estimator and why its logarithm need not be unbiased. Chain diagnostics, Monte Carlo error and likelihood-estimator variability will be reported.

### Intended outputs

A filtering and inference report, synthetic recovery experiments, latent-volatility plots, predictive calibration figures and comparisons of accuracy against computational cost. Historical-return inference will use the physical probability measure and will be kept distinct from risk-neutral option calibration.

**Reference for the advanced extension:** Andrieu, Doucet and Holenstein, [Particle Markov Chain Monte Carlo Methods](https://research-information.bris.ac.uk/en/publications/particle-markov-chain-monte-carlo-methods/).

## 04. Household liquidity and bank switching

**Research question:** how do liquidity constraints, uncertain income and switching costs determine whether households move deposits to a bank offering a better return?

### Mathematical content

The initial model will use a finite state space for wealth, income, bank affiliation and relevant rate conditions. Households will choose consumption, savings and whether to switch banks. The timing of income, interest payments, transfers and switching costs will be specified explicitly.

The report will derive the Bellman equation. For the finite model with bounded rewards, nonempty feasible action sets and discount factor $0<\beta<1$, it will prove contraction and establish a unique value function:

$$
\|Tv-Tw\|_\infty\leq\beta\|v-w\|_\infty.
$$

It will also derive the residual bound

$$
\|v-v^*\|_\infty\leq\frac{\|Tv-v\|_\infty}{1-\beta},
$$

and use it to justify the stopping criterion. Conditions for stationary distributions will be examined separately. Results for a finite approximation will not be treated as proofs for an unbounded continuous model.

### Planned implementation

- Explicit state, action, budget and transition definitions.
- Independently implemented value iteration and policy iteration.
- Policy evaluation through linear systems and diagnostics for induced Markov chains.
- Synthetic household simulation under the solved policy.
- An optional observation model allowing inactivity to differ from actual switching.

### Experiments and validation

Experiments will vary switching costs, rate differentials, income risk and liquid wealth. Validation will compare the two solution methods, check budgets and feasibility, report Bellman residuals, and assess asset-grid resolution and boundary sensitivity.

Where the induced chain supports a unique limiting distribution, long-run simulations will be compared with the computed distribution. Otherwise, dependence on initial conditions or recurrent classes will be reported. The optional observation model will investigate classification errors against known synthetic customer states.

### Intended outputs

A self-contained economic model, proofs, switching-region figures, policy comparisons, simulated distributions, welfare calculations and an analysis of observation-based classification. Any parameter-recovery extension will begin with synthetic data.

This project will use independently generated data. Restricted bank records, identifiers and non-public research outputs will not form part of the public repository.

**Theoretical foundation:** Sargent and Stachurski, [Discrete State Dynamic Programming](https://python-advanced.quantecon.org/discrete_dp.html).

## 05. Volatility calibration and identifiability

**Research question:** can parameter sets that fit option prices almost equally well imply materially different Greeks or prices for contracts outside the calibration set?

### Mathematical content

The project will study Heston pricing through characteristic functions and Fourier inversion, followed by constrained nonlinear calibration. The report will explain the relevant complex-valued functions, numerical integration, parameter constraints, Jacobians and singular-value diagnostics.

It will distinguish numerical integration error, optimisation error, weak identification and model misspecification. Regularisation will be analysed as an additional modelling choice. Its effect on fitted parameters and prices will be documented.

### Planned implementation

- A Heston characteristic-function pricer with stable complex-function conventions.
- Configurable numerical quadrature with independent tolerance checks.
- Synthetic option-surface generation from known parameters.
- Calibration with parameter constraints, multiple initialisations and optional regularisation.
- Diagnostics for residuals, Jacobian conditioning, parameter sensitivity and implied risks.

### Experiments and validation

Experiments will perturb synthetic quotes, vary the available strikes and maturities, and compare fits from multiple starting points. Held-out contracts will assess interpolation or extrapolation; that exercise will be distinguished from forecasting future market prices.

Validation will use independently computed benchmarks and appropriate limiting cases. Price-bound and strike-convexity checks will be included. Feller's condition will be discussed in terms of variance-boundary behaviour rather than treated as a universal requirement for a usable Heston pricing model.

### Intended outputs

A pricing and calibration report, parameter-recovery experiments, residual surfaces, sensitivity diagnostics, and comparisons of Greeks or held-out contract values across near-equivalent fits. A market-data extension will follow only once data provenance and redistribution terms are documented.

**Methodological starting point:** Cui, del Baño Rollin and Germano, [Full and Fast Calibration of the Heston Stochastic Volatility Model](https://arxiv.org/abs/1511.08718).

## Intended repository contents

The paths below describe the proposed layout. Only this README is supplied at the planning stage.

| Path | Intended contents |
| --- | --- |
| `README.md` | Portfolio overview, project scope and implementation status |
| `pyproject.toml` and a dependency lock file | Package metadata, dependencies, development tools and reproducible environment |
| `src/qf_research/optimal_stopping/` | Pricing models, trees, finite-difference solvers and exercise-policy algorithms |
| `src/qf_research/robust_portfolios/` | Estimators, objectives, constraints, optimisation routines and portfolio evaluation |
| `src/qf_research/volatility_inference/` | State-space models, Kalman and particle filters, estimation and predictive diagnostics |
| `src/qf_research/household_finance/` | Household model, dynamic-programming algorithms and synthetic simulations |
| `src/qf_research/volatility_calibration/` | Characteristic functions, numerical integration, calibration and sensitivity analysis |
| `src/qf_research/common/` | Shared random-number handling, numerical utilities and result metadata |
| `projects/01_optimal_stopping/` | Project README, experiment configurations, notebooks and report source |
| `projects/02_robust_portfolios/` | Project README, experiment configurations, notebooks and report source |
| `projects/03_volatility_inference/` | Project README, experiment configurations, notebooks and report source |
| `projects/04_household_finance/` | Project README, experiment configurations, notebooks and report source |
| `projects/05_volatility_calibration/` | Project README, experiment configurations, notebooks and report source |
| `tests/` | Mathematical benchmarks, numerical checks and small reproducibility checks |
| `benchmarks/` | Performance experiments, hardware descriptions and accuracy comparisons |
| `data/manifests/` | Source descriptions, observation periods, licences, transformations and checksums |
| `data/synthetic/` | Reproducible generated examples, with large outputs excluded from version control |
| `results/` | Selected generated figures and tables, grouped by project and experiment |
| `reports/` | Released mathematical and research reports with reproducible source references |
| `scripts/` | Experiment runners and report-generation utilities |
| `cpp/` | Selected optional C++ implementations and their build configuration |
| `.github/workflows/` | Automated checks for released code and small benchmark cases |
| `CITATION.cff` | Citation metadata once a public release is available |
| `LICENSE` | Code reuse terms, to be selected before a code release |

Each project will have its own local overview and reproducible workflow. Shared components will be reused only where the model assumptions and numerical conventions agree.

## Mathematical coverage

| Foundation | Planned evidence |
| --- | --- |
| Analysis and convergence | Proofs with explicit assumptions; numerical consistency and convergence studies; limit arguments where required |
| Compactness and continuity | Existence arguments for continuous objectives on explicitly compact feasible sets |
| Linear algebra | Spectral diagnostics, conditioning, regression, covariance updates and linear systems |
| Measure-theoretic probability | Information sets, conditional expectation, filtrations, stopping times and change of measure |
| Stochastic processes | Geometric Brownian motion, latent-state dynamics, martingales and controlled Markov chains |
| Differential equations | Pricing PDE derivations, boundary conditions and numerical solution |
| Statistics | Estimation uncertainty, synthetic recovery, predictive calibration and chronological evaluation |
| Optimisation | Convexity, optimality conditions, robust objectives, dynamic programming and inverse problems |
| Scientific programming | Reusable implementations, independent checks, reproducible experiments and documented performance |

This is a map of intended evidence, not a claim that every topic has been mastered or that the portfolio exhausts these subjects.

## Reproducibility and validation standards

Every released experiment will record its configuration, random seeds, model assumptions, numerical tolerances, dependency versions and relevant data provenance. Long-running experiments will include their computational budget and hardware details. Performance comparisons will identify what is timed and compare accuracy as well as runtime.

Analytical benchmarks, independent implementations and known generating parameters will be used wherever possible. Tolerances will reflect the underlying numerical or statistical error. Repeated simulation runs will be used when a single seed would conceal variability.

Empirical workflows will document information timing, training and evaluation periods, rebalancing conventions and transaction costs where applicable. Results will retain unsuccessful methods and parameter regimes where conclusions weaken. Code that implements a method will be distinguished from third-party solvers used for numerical primitives or independent verification.

Notebooks will communicate experiments. Reusable algorithms will live in the source package. Each implemented project will provide a short demonstration and a separate command for the full experiment suite. Runnable setup and reproduction instructions will be added with the first implementation; no executable package is claimed by this planning README.

## Data, attribution and reporting

Synthetic data will provide the initial validation environment. Public-data extensions will record the source, acquisition date, units, transformations, missing-data treatment and redistribution conditions. Data requiring credentials or restricting redistribution will be represented by documented acquisition instructions rather than bundled records.

Reports will identify established methods, external code and original extensions separately. Mathematical arguments adapted from a source will cite it. Code reuse will follow the relevant licence, and data rights will remain separate from the repository's eventual code licence.

Every completed project report will contain the question, assumptions, derivations, algorithms, experimental design, measured results and limitations. Figures and tables will be generated from recorded experiments. Claims about accuracy, runtime or predictive performance will be added only after the corresponding experiments have been run and checked.

## Development milestones

- [ ] Define the shared package layout, environment and experiment metadata.
- [ ] Complete Project 01's European benchmarks and mathematical derivations.
- [ ] Add and validate Project 01's early-exercise methods and error analysis.
- [ ] Complete Project 02's baseline formulations, robust derivation and solver checks.
- [ ] Run Project 02's controlled uncertainty experiments and chronological evaluation.
- [ ] Develop Project 03's filtering benchmarks before parameter-inference extensions.
- [ ] Develop Project 04's finite household model, proofs and independent solvers.
- [ ] Develop Project 05 after the required pricing components have been validated.
- [ ] Release each completed project with its report, figures and reproduction instructions.

A project will be marked complete when its main question has been answered with reproducible evidence, its central mathematical claims have been justified, and its limitations have been documented. Optional extensions will have their own status and will not be required to release a sound core project.
