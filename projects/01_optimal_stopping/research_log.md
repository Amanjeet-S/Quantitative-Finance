# Research log

## 14 September 2026: foundations and scope review

**Question at this stage:** can the preliminary implementation value matching contracts consistently, and what observed data are required before an empirical investigation is feasible?

Implemented analytical prices and Greeks, exact GBM sampling, CRR valuation, a finite-difference prototype and backward regression with independent forward evaluation. Initial checks cover parity, analytical references, finite-difference Greeks, deterministic limits, exercise-calendar alignment, martingale moments and evaluation-time information restrictions.

The initial model-validation experiment uses a declared mathematical test fixture. It compares deterministic refinements and independently evaluated policies across pre-specified polynomial degrees and training sizes. The results are saved as preliminary numerical checks. Their interpretation is restricted to the stated model.

**Research design decision:** the complete project will be developed in reviewed stages. Observed data must ground the main empirical study. The existing simulations remain numerical validation, and the current code must not be treated as a completed research contribution.

**Data progress:** S&P Capital IQ access is available. Its actual option quote entitlement remains to be inspected. The official Treasury snapshot contains 175 observations, with the latest dated 11 September 2026. The ingestion module preserves raw observations, dates, units and hashes. Treasury par yields are not yet pricing discount factors.

**Validation:** 44 automated checks pass in the frozen Python 3.13.9 environment. A clean environment outside the cloud-synchronised repository resolves an import-path issue caused by hidden file flags on editable-install files. The documented setup uses that location. Broader numerical validation remains a later stage.

**Open issues before advancing:** inspect actual quote and corporate-action fields; select a feasible empirical sample; review dividend and exercise conventions; isolate numerical error sources; investigate whether the observed policy differences can be measured precisely enough to support a research conclusion.

**Language plan:** retain Python for research and build a C++ numerical component after profiling identifies a useful target. Numerical equivalence and a fair performance protocol precede any efficiency claim.

This entry records the current design. Later entries should retain changes of direction and unsuccessful tests instead of rewriting the history around favourable results.

## 14 September 2026: Capital IQ Pro access inspection

Verified signed-in access to Capital IQ Pro and the US SPY security identity, ARCA:SPY / ISIN US78462F1030. The underlying security page displayed pricing dated 11 September 2026, dividend summary fields and an Excel export action.

Checked the security side menu, Markets navigation, site map and Pricing Data field selector. No option-chain interface was identified in these checks. This does not establish that the account lacks option data. Historical bid and ask coverage remains unresolved, and the provider Help Center returned an error.

An attempted export of the underlying reference page did not yield a confirmed download. No option quotes or underlying observations from this inspection have been ingested. The [data plan](data_plan.md) records the checks and the [pilot request](capital_iq_pilot_request.md) remains outstanding. The empirical stage is not ready to advance until an actual option export and its definitions pass the audit.

## 14 September 2026: correction to the option-data source

The account holder clarified that Capital IQ Pro's options and warrants information supports capital structure and dilution analysis. It is not the source of the exchange-traded option quotes required by this study. The earlier option-chain request is superseded. Capital IQ remains a supporting source for underlying prices, dividend histories and corporate actions. The research design needs consistent historical snapshots, not a live trading terminal.

Reviewed Cboe's Option EOD Summary documentation and downloaded its published demonstration ZIP and version 1.1 file specification. The two alternative CSV variants each contain 32,672 records and 34 columns, including 7,630 SPY records, dated 25 August 2023. Column-width and candidate duplicate-key checks passed. Stored original files and hashes privately. These are format checks only; the demonstration is not recent empirical evidence and the variants must not be pooled.

The specification identifies start-of-day open interest, an early-close time exception, several zero placeholders and deprecated fields. It also contains inconsistent wording for `bid_eod`. These issues are recorded in the [sample audit](cboe_sample_audit.md), with a revised [option-quote pilot request](option_quote_pilot_request.md). No recent option panel or paid access has been acquired, and no calibration or empirical conclusion has been produced. The production loader and full quote-quality audit remain future work.
