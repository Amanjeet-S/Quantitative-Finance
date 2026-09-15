# Cboe Option EOD Summary: demonstration format audit

Inspection date: **14 September 2026**. This is a structural audit of provider demonstration material, not an empirical result or evidence of current quote coverage.

## Source and preservation

The archive was downloaded from Cboe's [published sample link](https://datashop.cboe.com/download/sample/217) on the [Option EOD Summary product page](https://datashop.cboe.com/option-eod-summary). The [file specification](https://datashop.cboe.com/documents/Option_EOD_Summary_Layout.pdf) identifies itself as version 1.1. The original ZIP and PDF, with acquisition metadata, are retained under **data/private/cboe/2026-09-14-format-audit/**. No source file was edited.

The provider's accompanying readme describes the files as demonstrations that may contain a subset of actual data. They must not be treated as a complete market panel. Two alternative versions illustrate different reporting of underlying index values; they must not be concatenated as separate observations.

| Check | Result for each CSV variant |
| --- | --- |
| Quote date present in the file | 25 August 2023 |
| Data rows | 32,672 |
| Columns | 34 |
| SPY rows | 7,630 |
| Other underlying symbols | TSLA, ^SPX, ^VIX |
| Rows with an inconsistent number of columns | 0 |
| Additional duplicate records by the identity tuple below | 0 |

The duplicate check used `underlying_symbol`, `quote_date`, `root`, `expiration`, `strike` and `option_type`. It establishes uniqueness under this candidate key only. These counts are format diagnostics, not a validation of prices, liquidity, completeness or economic relevance. The SPY count includes all expiries in the sample and is not the 30-to-180-day pilot subset.

Archive SHA-256: `981be1aafe6970e798d5be42f049e6c5b6e507f01cb1509ab513e9bc2e08da24`.

Specification SHA-256: `8d39ee5dfcfdee0eb235e667543f92e2d6293dd4e1f6f055d907585f23f0f1be`.

## Consequential field conventions

The following interpretations come from the linked specification, rather than assumptions based on column names.

| Field or feature | Consequence for the future loader |
| --- | --- |
| `bid_1545`, `ask_1545`, `underlying_bid_1545`, `underlying_ask_1545` | Use matched option and underlying snapshots. The labelled time is US Eastern, with the documented early-close exception |
| Early-close days | The `1545` fields represent 12:45 US Eastern; retain this exception explicitly |
| `open_interest` | Start-of-day information from OCC, not an end-of-day count |
| Underlying bid and ask equal to zero | May denote unavailable quotes. Preserve the raw value and flag it; do not interpret it as a traded zero underlying price |
| OHLC and VWAP equal to zero | May mean no eligible trade occurred. Do not replace missing bid and ask observations with these values |
| `implied_volatility_1545` equal to zero | May indicate missing inputs or a failed calculation; it is not necessarily zero economic volatility |
| `implied_underlying_price_1545` | Deprecated and documented to default to zero |
| `delivery_code` | Deprecated and empty; does not identify contract deliverables |
| Calculations add-on | The sample includes model outputs. An order without calculations omits those columns; a loader must not require them |

The specification describes `bid_eod` using ask-price wording, while the field label and product description identify a bid field. This is a documentation inconsistency that needs clarification before relying on that field's exact semantics. The proposed pilot uses the explicitly described 15:45 bid and ask fields. EOD observations remain preserved if delivered, without silently resolving the inconsistency.

The dated sample readme and current product page also differ in their description of access to SPX underlying index values. This reinforces the decision to use the SPY ETF for the pilot and to verify the actual ordered product's terms and definitions. No inference about current index entitlements is drawn from the 2023 sample.

## Outcome and remaining work

The archive supplies a useful candidate schema and exposes important timing and missing-value conventions. It does **not** satisfy the recent-data requirement. No calibration, option-price comparison or policy-loss conclusion has been produced from it.

Next, acquire the [recent SPY pilot](option_quote_pilot_request.md), verify the actual delivered schema, and obtain the required exercise, settlement and deliverable reference data. A production loader and its financial validation rules will be developed against that evidence. The empirical sample and date split remain provisional.
