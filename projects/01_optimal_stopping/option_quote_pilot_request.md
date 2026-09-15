# Historical option-quote pilot request

Source decision updated on **14 September 2026**. This request replaces the earlier request for an option chain from Capital IQ Pro. It is a small feasibility audit, not the final empirical sample.

## Dataset and scope

The preferred candidate is [Cboe DataShop Option EOD Summary](https://datashop.cboe.com/option-eod-summary). [OptionMetrics through WRDS](https://wrds-www.wharton.upenn.edu/pages/about/data-vendors/optionmetrics/) is an alternative if an institutional entitlement is available. Neither paid access nor a recent extract has been acquired.

| Item | Request |
| --- | --- |
| Underlying | SPY, State Street SPDR S&P 500 ETF Trust, NYSE Arca; ISIN US78462F1030 |
| Dates | Latest five completed trading sessions actually available at acquisition; record their exact dates and the delivery lag |
| Observation | Prefer the matched option and underlying 15:45 US Eastern snapshots; retain both 15:45 and EOD columns if delivered |
| Contracts | Calls and puts with 30 to 180 calendar days to expiry, all strikes; if the provider delivers every expiry, preserve that complete file and apply this restriction locally |
| Format | Original CSV or native export, with all provider headers, documentation and missing-value conventions intact |
| Add-ons | Model-implied volatility and Greeks are optional. They are not necessary to acquire the observed bid and ask inputs |
| Filters | No discretionary liquidity filtering before the raw file is saved |

The actual date and calendar must determine snapshot time. Cboe's specification states that columns labelled `1545` represent 12:45 US Eastern on early-close days. These days must be identified from the relevant exchange calendar. Do not assume every row was observed at 15:45, or combine afternoon option quotes with later Capital IQ closing prices.

## Required concepts

- Contract identity: underlying symbol, root or trading class, call/put, strike and expiry. Retain an explicit provider identifier if supplied.
- Prices: option bid and ask with sizes, contemporaneous underlying bid and ask, quote date, snapshot convention and timezone.
- Activity: trade volume and open interest, including whether each measure refers to the start or end of the session.
- Reference data: currency, multiplier, deliverable, exercise style, settlement method and relevant expiry times. Obtain missing terms from a separate authoritative contract source.
- Provenance: dataset and version, acquisition date, query settings, file hashes, field definitions and applicable reuse terms.

The Cboe sample's empty, deprecated `delivery_code` is not evidence of a standard contract. Root, strike and expiry alone do not establish its deliverable or exercise convention. Avoid index substitutions: SPX is not the SPY ETF option contract.

## Acceptance and storage

Save unmodified deliveries under **data/private/cboe/**, or the corresponding provider directory within **data/private/**. Record their hashes before cleaning. The public repository will contain original code, methods and only outputs permitted by the applicable terms.

The pilot passes only after checking identity, dates, units, missing-value meanings, duplicate records, bid–ask consistency, quote timing and the missing contract-reference inputs. A current snapshot establishes only cross-sectional coverage. A historical vendor demonstration file establishes only its format. Neither establishes the requested recent five-session panel or supports historical out-of-sample conclusions on its own.

Before any paid order, obtain the exact price and included fields for this limited extract. A displayed zero subtotal before symbols and dates are selected is not a quotation. The sample audit does not authorise a purchase.
