# Robust portfolio optimisation under estimation uncertainty

**Status: planned. No investment results or completed solver are available.**

**Question:** When is protection against uncertain expected returns and imperfect covariance estimates worth its opportunity cost?

The study will connect covariance conditioning, estimation uncertainty and convex optimisation to realised investment decisions. It will compare transparent portfolio benchmarks under matching asset universes, information sets, constraints and costs.

## Planned work

- Derive minimum-variance and mean–variance formulations and their optimality conditions.
- Examine eigenvalues, conditioning, regularisation and an explicitly justified uncertainty set.
- Validate a manageable optimisation subproblem against an established solver.
- Use a chronological estimation, validation and evaluation design with documented costs.
- Report concentration, turnover, realised risk and sensitivity, alongside solver diagnostics.

The historical asset universe and data source remain to be selected. No subscription dataset is assumed available. Model-generated inputs may support numerical validation only; empirical conclusions will require observed, traceable returns.

## Directory guide

- `configs/`: future experiment settings.
- `notebooks/`: future executed research notebooks.
- `reports/`: future paper and derivations.
- `results/`: future generated outputs and run manifests.

These directories currently document their intended use. The [repository roadmap](../../docs/roadmap.md) defines the stages required before reporting findings.
