# Research design

**Design stage. The specification is not yet locked and no main estimates have been inspected.**

## Question and measurement

Estimate how an unexpected ECB monetary tightening changes German durable consumer-goods production relative to nondurable consumer-goods production. Eurostat's `MIG_DCOG` and `MIG_NDCOG` identify consumer-goods groups, not the broader US manufacturing categories or household consumption.

The primary estimand will be the difference in the two output responses at one declared horizon. A 12-month horizon is a candidate, subject to the design review. Response paths over a limited range will provide context. The final choice, lag specification and control set must be recorded before the main estimation.

## Identification

Use the author-maintained update of the Jarociński–Karadi decomposition. Distinguish monetary-policy shocks from central-bank information shocks. Review the sign restrictions, normalisation, event windows and monthly aggregation before choosing the primary measure. Published identification methods require assumptions; they do not automatically establish exogeneity in this application.

Reconcile the 315 dated observations in the inspected raw EA-MPD workbook with the 312 rows in the derived event file. Retain documented exclusions. Missing events must not silently become zero shocks. No-event months and genuinely zero measured surprises have different meanings from missing observations.

## Estimation and inference

Derive a parsimonious local-projection specification with output transformations, lagged controls and a clearly stated shock scale. Estimate the response difference directly or through a joint procedure preserving cross-series covariance. One significant response and one insignificant response do not establish a significant difference.

Check time-series properties and justify the outcome transformation. Use an inference method suited to persistent outcomes, serial dependence and overlapping horizons. Distinguish a confidence interval at the primary horizon from simultaneous inference across many horizons. If shocks are reconstructed, discuss uncertainty in their estimation and identification.

## Robustness and interpretation

Pre-specify a small set of checks for lag length, shock decomposition, influential events and crisis-period sensitivity. Record sample losses separately for each horizon. Interpret possible export, inventory and supply mechanisms without claiming to have separately identified them.

A fixed-composition euro-area comparison is optional and requires its own metadata review. The shared policy shocks do not create independent replications. Report imprecise and contrary results as part of the evidence.

## Completion criteria

An auditable dataset, a locked primary specification, independently checked estimates, reproducible figures and tables, a published-literature review and a paper with explicit limitations are required. The data-coverage notebook currently establishes only access and basic coverage.
