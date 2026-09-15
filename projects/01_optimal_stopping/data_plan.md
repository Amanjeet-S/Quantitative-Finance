# Observed market data: feasibility and acquisition plan

Source review date: **14 September 2026**.

**Current source decision:** use a dedicated historical option-quote provider for the empirical pilot. The account holder has clarified that Capital IQ Pro's options and warrants information concerns capital structure and dilution, rather than the exchange-traded quote panel required here. Capital IQ is retained for underlying prices, dividends and corporate actions. Real-time trading data are not required for this research design; consistently timed historical quotes are.

## Available access and verified sources

| Source | What is established | What remains unresolved |
| --- | --- | --- |
| [S&P Capital IQ](https://www.capitaliq.com/) / [Capital IQ Pro](https://www.spglobal.com/market-intelligence/en/solutions/products/sp-capital-iq-pro) | Signed-in access and the US SPY underlying security page were verified. The account holder clarified the options-data limitation. | Underlying history, dated dividend and corporate-action exports still need auditing. Do not request an exchange-traded option chain from this source. |
| [US Treasury interest-rate statistics](https://home.treasury.gov/policy-issues/financing-the-government/interest-rate-statistics) and [documented XML feed](https://home.treasury.gov/treasury-daily-interest-rate-xml-feed) | Public official par-yield observations can be retrieved with dates and original field names. | A defensible discount-curve construction and the appropriate financing convention are separate research decisions. |
| [Cboe Option EOD Summary](https://datashop.cboe.com/option-eod-summary) | The documentation specifies historical US option NBBO snapshots and underlying bid–ask fields for stocks and ETFs. The published demonstration ZIP and specification have been downloaded and structurally inspected. | Preferred candidate for the recent SPY pilot. No recent quote panel, paid entitlement or firm price has been acquired. The 2023 demonstration is not current empirical evidence. |
| [OptionMetrics through WRDS](https://wrds-www.wharton.upenn.edu/pages/about/data-vendors/optionmetrics/) | The provider description lists historical option bid and ask prices, volume, open interest, underlying information, dividends and corporate actions. | Alternative if institutional access is available. Access and the latest actual quote date must be checked; a catalogue update or an option expiry date is not a quote observation date. |
| [Cboe historical options volume](https://www.cboe.com/us/options/market_statistics/historical_data/) | The public download is an options-volume series. | It does not supply the quote panel required for pricing or calibration. |

The Cboe quote documentation distinguishes a 15:45 US Eastern snapshot from end-of-day quotes. That difference matters when aligning the underlying. It also records a quote-size methodology change effective 22 June 2026 for [interval quotes](https://datashop.cboe.com/option-quote-intervals). Any use of that product must retain the applicable definition rather than assuming an unchanged series.

## Option-quote and underlying export audits

Begin with one underlying and a few actual trading dates to inspect the schema. Expand the sample only after confirming these fields. Field names below describe required concepts; a source may require separate quote and contract-reference files.

The next acquisition is the [historical SPY option-quote pilot](option_quote_pilot_request.md). The [Cboe demonstration audit](cboe_sample_audit.md) records actual column names, structural checks and unresolved documentation issues. A separate [Capital IQ request](capital_iq_pilot_request.md) covers supporting underlying data once the quote dates are established.

### Account inspection on 14 September 2026

The [SPY Security Detail page](https://capitaliq.spglobal.com/web/client?auth=inherit#company/capitalIssuesDetail?ID=5721437&Key=1771706&KeyType=1) identifies ARCA:SPY and ISIN US78462F1030. Its pricing section is dated 11 September 2026. The page offers underlying price and volume information, a dividend summary and an **Export to Excel as Data** action. These observations establish access to the underlying security page, not an option quote panel or a validated time series.

The security side-menu search for options returned no match. The **Screener > Pricing Data** field selector listed Company Issues, Indexes, CDS Single Names, CDS Groups and Rates/Yields. No option category was visible in that selector. The site map and Markets navigation did not reveal an option-chain page. These are bounded navigation checks, not proof that the subscription excludes options elsewhere in the product.

The linked provider Help Center returned an error before documentation could be inspected. An attempted Excel export of the underlying reference page did not produce a confirmed download. No market observations from this inspection have been ingested into the empirical dataset.

**Subsequent correction:** the account holder clarified that Capital IQ Pro is not the source of the required exchange-traded option quotes. The earlier navigation investigation is closed. The source plan now separates dedicated option data from Capital IQ's supporting underlying data.

### Cboe demonstration audit on 14 September 2026

The two CSV variants in the published sample each contain 32,672 records across 34 columns, including 7,630 SPY records, all dated 25 August 2023. They are alternative demonstrations, not distinct market samples to combine. Basic column-width and duplicate-key checks passed. Price validity, contract terms and market completeness have not been certified. See the [audit](cboe_sample_audit.md) for hashes and scope.

For the pilot, prefer the matched option and underlying `1545` quotes. Resolve early-close days, when this label represents 12:45 US Eastern, and retain start-of-day open-interest semantics. The specification contains inconsistent ask-price wording for `bid_eod`; that ambiguity must be resolved before relying on its exact semantics. The production loader must recognise provider-defined zero placeholders and must not treat the deprecated `delivery_code` as a complete contract description.

| Group | Required information | Why it matters |
| --- | --- | --- |
| Contract identity | Provider contract identifier, underlying identifier, call/put, strike, expiry, currency and multiplier | Correctly identify the security and unit of account |
| Exercise and settlement | American/European/Bermudan style, settlement type, expiry and settlement times, adjusted-contract flag | Ensure the numerical problem matches the traded contract |
| Option quote | Timestamp and timezone, bid, ask, bid/ask size where available, quote-status flags | Measure spreads and detect stale, crossed or invalid quotes |
| Underlying quote | Contemporaneous unadjusted price or bid and ask, timestamp | Match the option's spot input to the quote time |
| Underlying history | Raw prices, adjusted prices and adjustment factors with clear definitions | Keep return estimation separate from contract-level spot and strike handling |
| Corporate actions | Splits, special distributions, changes to deliverables and identifiers | Prevent artificial jumps and incorrect strike comparisons |
| Dividends | Amount, currency, announcement date, ex-date and payment date | Use only information known at valuation and model cash flows correctly |
| Liquidity | Volume and open interest with observation dates | Assess coverage and support declared filters |
| Export provenance | Provider, dataset, extraction time, query settings, units and documentation | Reproduce the selection and interpret the fields |

Provider model outputs such as implied volatility or Greeks must be labelled as estimates. They are not independent observed prices and must not be used to validate the same model that generated them.

## Quote selection and information timing

The initial pilot should determine available frequency and history. The final date range, contract universe, moneyness and maturity restrictions will be written into a configuration before the principal empirical evaluation.

Reject malformed identities and impossible dates. Record exclusions for non-positive prices where inappropriate, crossed markets, stale quotes, missing inputs and non-standard deliverables. Keep zero bids identifiable rather than mechanically turning them into positive prices. Apply liquidity thresholds as declared design choices and report sensitivity to them.

Never match an afternoon option quote to a later underlying close without recording the mismatch. For a historical exercise decision, use dividend information available by that date. Current revisions and current corporate-action databases may need extra treatment to support point-in-time claims.

## Rates already accessible

The Treasury acquisition code saves the raw XML, parsed observations and a metadata file. The latest observation available in the initial retrieval is **11 September 2026**, with 175 observations in the 2026 feed through the requested cutoff. See [the snapshot metadata](../../data/public/treasury/2026-09-14/metadata.json).

Values remain annual percentage par yields exactly as the source labels them. Missing entries remain missing. The module does not bootstrap discount factors or claim to supply an OIS curve. The current retrieval is a saved data vintage, not an archive of every historical publication vintage.

## Repository storage

Public Treasury snapshots can be inspected alongside their source URLs and hashes. Capital IQ exports belong in **data/private/capital_iq/** and Cboe material in **data/private/cboe/**. Both are covered by the existing ignored **data/private/** directory. Public documentation, field mappings, acquisition instructions and permitted derived results should allow review without uploading restricted source data.

No quote dataset is fabricated to fill the current gap. Until a real option sample has passed the audit, numerical model checks remain separate from empirical results.
