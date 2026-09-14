# Quantitative Finance: Theory, Computation and Model Validation

A research portfolio exploring how mathematical models and computational methods inform financial and economic decisions, and how uncertainty in their assumptions, inputs and numerical solutions affects the answers.

The planned work spans option exercise, investment allocation and household finance. Each investigation will combine a focused research question, mathematical derivations, reusable software and experiments that assess the reliability and economic significance of the results.

**Status: planning and project specification.** This repository currently contains this overview. Implementations, reports and results will be added as projects are developed and validated. The descriptions below describe intended work, not completed findings.

## What this repository investigates

Financial models turn assumptions and observations into decisions: when to exercise an option, how to allocate a portfolio, or whether a household should move its savings. Those decisions depend on several choices:

- **Economic assumptions:** preferences, incentives, constraints and the information available to decision-makers.
- **Financial modelling:** contract terms, risk exposures, market frictions and the distinction between valuation and forecasting.
- **Mathematical methods:** probability, analysis, linear algebra, differential equations, statistics and optimisation.
- **Computational methods:** algorithms, data representations, numerical approximations and the allocation of computational resources.

The common research question is how these choices affect a model's conclusions. A method will be assessed both for the accuracy of its implementation and for the quality of the decisions it supports.

Python will be the primary implementation language. Selected C++ components may be introduced where profiling identifies a worthwhile computational bottleneck.

## Planned project portfolio

| Research direction | Central question | Main foundations |
| --- | --- | --- |
| **Optimal stopping and option pricing** | How do numerical approximations affect option values and exercise decisions? | Conditional expectation, martingales, stochastic calculus, PDEs and numerical analysis |
| **Robust portfolio optimisation** | When does protection against estimation uncertainty improve investment decisions? | Matrix analysis, convex optimisation, duality, statistical estimation and decision theory |
| **Household finance under constraints** | How do income risk, liquidity constraints and financial frictions affect household choices and welfare? | Dynamic programming, fixed points, Markov processes, constrained optimisation and economic modelling |

Two possible later extensions will investigate **stochastic-volatility inference** and **option-model calibration and identifiability**. Their scope is outlined below. The economics project's final topic remains under development.

## 1. Optimal stopping and option pricing

**Research question:** How much option value is lost through approximate exercise decisions, and which computational improvements reduce that loss most efficiently?

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

## 3. Household finance and decisions under constraints

**Research direction:** How do income uncertainty, liquidity constraints and financial frictions affect household decisions and welfare?

This project will develop a self-contained economic model supported by numerical solutions and controlled experiments. The final topic will be chosen after the mechanism, model scope and available evidence have been assessed.

Two candidate questions are under consideration:

- **Liquidity and bank switching:** How do liquidity risk and uncertainty about the persistence of a deposit-rate advantage affect switching decisions, and which households benefit most from lower switching costs?
- **Income risk and portfolio choice:** How does correlation between labour-income shocks and equity returns affect optimal equity exposure and the welfare cost of borrowing constraints?

The project will pursue one focused question. Both candidates connect household preferences and constraints to a financial decision and a measurable welfare consequence.

### Mathematical and computational work

- Specify states, actions, information, budget constraints and transition dynamics.
- Derive the Bellman equation and relevant optimality conditions.
- Establish the contraction property and uniqueness of the value function for an appropriate bounded, discounted formulation.
- Implement value iteration and policy iteration, with independent checks on tractable cases.
- Use Bellman residuals to assess solution accuracy and investigate grid and boundary sensitivity.
- Compare policies and welfare across carefully controlled changes in economic assumptions.

Proofs for a finite approximation will be identified as such. Any extension to continuous or unbounded state spaces will require its own assumptions and justification. Stationary-distribution calculations will be used only under the relevant conditions on the induced Markov chain.

### Experiments and intended outputs

Initial experiments will use synthetic data and explicit parameter choices. Comparative statics will isolate the mechanism being studied, while sensitivity analysis will assess whether conclusions depend on calibration or numerical approximations.

For a bank-switching model with exogenous deposit rates, findings will concern household responses to those rates. Claims about banks' equilibrium pricing would require an additional model of bank behaviour.

The intended outputs are an economics research paper, documented solvers, policy-region figures, welfare comparisons and a technical account of numerical accuracy.

**Starting reference:** [Sargent and Stachurski, *Optimal Savings III: Stochastic Returns*](https://python.quantecon.org/os_stochastic.html).

## Possible later extensions

### Stochastic-volatility inference and predictive uncertainty

This project would investigate when latent-volatility models improve financial predictive distributions and risk decisions. It would develop filtering from linear Gaussian benchmarks to a stochastic-volatility model, with a bootstrap particle filter and a documented parameter-estimation method.

The investigation would cover filtering versus smoothing, stable importance weights, particle degeneracy, resampling and the distinction between latent-state and parameter uncertainty. Validation would begin with synthetic recovery and comparisons against a Kalman solution in a separate linear Gaussian benchmark, followed by chronological predictive evaluation.

Predictive accuracy and the quality of any risk-management or allocation decision would be reported separately. Particle MCMC would be an advanced extension after the filtering implementation is validated.

**Starting reference:** [Andrieu, Doucet and Holenstein, *Particle Markov Chain Monte Carlo Methods*](https://www.stats.ox.ac.uk/~doucet/andrieu_doucet_holenstein_PMCMC.pdf).

### Volatility calibration and identifiability

This project would ask whether parameter sets that fit option prices almost equally well imply materially different Greeks or values for other contracts.

It would combine Heston characteristic-function pricing, Fourier inversion, numerical quadrature and constrained nonlinear calibration. Synthetic option surfaces would support parameter-recovery experiments, multiple-start optimisation, Jacobian singular-value diagnostics and sensitivity to quote perturbations.

The analysis would separate integration error, optimisation error, weak identification and model misspecification. Historical-return inference and risk-neutral option calibration would retain their distinct probability measures and model specifications.

## What a completed project will include

| Deliverable | Purpose |
| --- | --- |
| **Project overview** | Explain the question, contribution, principal findings and limitations |
| **Research report** | Present assumptions, derivations, algorithms, experiments and interpretation |
| **Reusable source code** | Implement the central models and numerical methods |
| **Experiment configurations** | Record parameters, seeds, tolerances and evaluation choices |
| **Validation and benchmarks** | Establish correctness and assess accuracy, stability and computational cost |
| **Generated figures and tables** | Make the findings inspectable and reproducible |
| **Data and source documentation** | Record provenance, transformations, permissions and attribution |

Notebooks will explain experiments; reusable algorithms will live in the source package. Set-up instructions and reproduction commands will accompany the first implementation.

## Reproducibility and research standards

Every released experiment will record its assumptions, configuration, random-number handling, numerical tolerances and dependency versions. Performance comparisons will identify the hardware and the operations being timed, and compare methods at stated levels of accuracy.

Analytical solutions, independent implementations and known generating parameters will provide checks wherever possible. Repeated runs will assess simulation variability. Empirical work will document information timing, data transformations and chronological evaluation, with uncertainty estimates appropriate to the dependence in the observations.

Reports will distinguish established theory, numerical evidence and empirical interpretation. Mathematical arguments will state their assumptions. Sources, adapted arguments, reused code and original contributions will be identified explicitly. Public datasets will be accompanied by provenance and redistribution information.

Each project will be released when its central question has been answered with reproducible evidence and its limitations have been documented. Optional extensions will have separate milestones so that a sound core investigation can be completed and presented independently.
