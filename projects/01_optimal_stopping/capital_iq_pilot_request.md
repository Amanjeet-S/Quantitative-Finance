# Capital IQ supporting-data request

Updated **14 September 2026**. The earlier request for exchange-traded option quotes from Capital IQ Pro is superseded by the [dedicated option-quote pilot](option_quote_pilot_request.md). Capital IQ's capital-structure options and warrants fields do not supply that panel.

## Purpose and timing

Use Capital IQ for the underlying security's price history, dividends and corporate actions. The required dates depend on the acquired option pilot. Freeze the option observation dates before requesting a large supporting export. A small export may be used earlier to inspect column definitions and availability.

| Dataset | Fields and conventions to preserve |
| --- | --- |
| SPY reference identity | NYSE Arca listing, ticker, provider identifier, ISIN US78462F1030, currency and trading calendar |
| Daily underlying history | Date, raw unadjusted close, available OHLC, volume, adjusted close or total-return series, adjustment factors and definitions |
| Dividends and distributions | Cash amount per share, currency, distribution type, announcement date, ex-date, record date and payment date |
| Corporate actions | Splits and other adjustments, effective dates, adjustment ratios and identifier changes where available |
| Provenance | Original headers, export date, dataset, source and field definitions, timezone and missing-value conventions |

Preserve revised or missing fields. Do not manufacture announcement dates from ex-dates, or reconstruct what was known historically using only a current dividend summary. ETF distributions must be identified from the underlying records and their definitions.

Raw underlying prices support contract-level spot handling; adjusted prices and total returns support different statistical questions. Keep them separate. A daily Capital IQ close is not contemporaneous with a 15:45 option quote: use the dedicated option provider's matched underlying quote for that valuation snapshot.

Save untouched exports under **data/private/capital_iq/**. Record hashes and audit actual fields before analysis. Capital IQ share-dilution calculations are not inputs to pricing an exchange-traded SPY option.
