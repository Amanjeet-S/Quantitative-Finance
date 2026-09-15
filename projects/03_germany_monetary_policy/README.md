# ECB monetary policy and consumer-goods production in Germany

**Status: data feasibility and research design. The source snapshot and coverage audit are available. No causal effects have been estimated.**

**Question:** How does an unexpected ECB monetary tightening affect durable consumer-goods production relative to nondurable consumer-goods production in Germany?

The intended estimand is the difference between the two production responses at a pre-specified horizon. The ability to postpone durable purchases motivates the question. Production also reflects exports, inventories and supply conditions; an estimated difference would not identify a unique transmission channel.

## Start here

1. [Research design and remaining decisions](research_design.md).
2. [Data audit and limitations](data_audit.md).
3. [Sources and publication status](SOURCES.md).
4. [Executed data-coverage notebook](notebooks/01_data_coverage.ipynb).
5. [Public snapshot and source manifest](../../data/public/germany_monetary_policy/README.md).

From the repository root:

```bash
python scripts/audit_germany_data.py
```

This command checks saved bytes and observed coverage without downloading new data. It does not run local projections. See [development instructions](../../docs/development.md) for environment setup and notebook execution.

## Planned analysis

Python will handle data construction, local projections, inference and generated outputs. Selected estimates will be independently checked in R with identical samples and conventions. The primary result will be a directly estimated response difference, with uncertainty appropriate to serial dependence and overlapping horizons.

A full research paper and a concise summary will present the same question, principal estimates and limitations. The main paper is intended to fit within 20 pages including references; the concise version will be designed to fit within 2,000 words, allowing for the destination's treatment of tables and figures. Neither document exists as a completed writing sample yet.
