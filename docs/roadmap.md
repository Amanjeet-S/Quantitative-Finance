# Research roadmap

Repository infrastructure and research completion are separate milestones. This file records the current scope rather than promising findings.

| Project | Current stage | Next evidence required |
| --- | --- | --- |
| 1. Optimal stopping | Python prototype and preliminary numerical validation | Observed option-quote pilot, numerical stress tests and a reviewed empirical design |
| 2. Robust portfolio optimisation | Research plan | Feasible historical asset universe, return definitions, benchmark design and source audit |
| 3. Germany monetary transmission | Public source snapshot and reproducible coverage audit | Review shock construction and event exclusions, lock the primary estimand and inference protocol, then implement estimation |
| 4. Stochastic-volatility inference | Optional extension | Establish a distinct question and suitable data before implementation |
| 5. Volatility calibration | Optional extension | Obtain suitable option quotes and validate a pricing engine before calibration |

## Stages shared by the empirical projects

1. Establish the question, economic mechanism, published foundations and data access.
2. Audit observations, measurement conventions, timing, missingness and permissions.
3. Record the primary specification, exclusions and robustness plan before estimating the main results.
4. Implement and independently validate the central methods.
5. Run the documented analysis and interpret uncertainty and competing explanations.
6. Produce the report, concise summary, executed notebooks and reproducible tables.

Stage 6 requires an answered research question with limitations, including a possible inconclusive answer. More algorithms or favourable statistical significance do not constitute completion.
