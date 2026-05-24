# Data Cleaning and Validation Log

The raw data arrived as a single denormalized Excel export of 102,598 rows and 26 columns. Before any modeling could happen, the data had to be cleaned and validated. Each step below was documented and justified rather than applied blindly, because every cleaning decision changes what the final numbers mean.

## Cleaning steps

| # | Action | Outcome |
|---|---|---|
| 1 | Remove exact duplicates | 541 rows removed |
| 2 | Fix borough typos; drop null borough rows | 2 corrected; 29 dropped |
| 3 | Impute country and country_code | 532 and 131 values imputed |
| 4 | Parse price and service_fee from string to numeric; drop null price rows | 247 rows dropped |
| 5 | Standardise boolean columns (instant_bookable, host_identity_verified) | 2 columns standardised |
| 6 | Fix minimum_nights outliers (negatives nulled; large values flagged) | 13 nulled; 35 flagged |
| 7 | Fix availability_365 range errors (negatives and values over 365 nulled) | 3,214 rows nulled |
| 8 | Parse last_review as a proper date | Date type applied |
| 9 | Drop license column (99.998 percent empty) | 1 column removed |
| 10 | Standardise column names to a consistent convention | 26 columns reviewed |

After these steps the cleaned dataset contained approximately 101,781 rows and 25 columns, with all primary analytical dimensions correctly typed and ready for modeling.

## Validation decisions and assumptions

These are the judgement calls made during cleaning and modeling. They are stated explicitly so that anyone reading the analysis knows exactly what the numbers assume.

**Geographic scope.** All listings are treated as being in New York City, USA. This is justified by all non-null country values being United States and all coordinates clustering in the NYC metro area.

**Typo correction.** The values brookln and manhatan were treated as data-entry errors for Brooklyn and Manhattan.

**Boolean interpretation.** An unconfirmed host identity is interpreted as not-verified (false). This reflects the literal semantic, not a judgement about host trustworthiness.

**Negative values.** Negative minimum_nights and negative availability_365 are physically impossible and were nulled.

**Out-of-range values.** availability_365 values above 365 have no valid interpretation and were nulled. minimum_nights values above 365 may represent legitimate long-term arrangements and were retained.

**Host attribute resolution.** Where the same host id appeared with different host name, verification, or listings-count values (a known data quality issue), the first occurrence was kept when building dim_host.

**Cancellation policy nulls.** Null cancellation policy values were treated as a third category, Unknown.

**Date dimension scope.** The date dimension spans 2010 to 2026 to comfortably cover all observed last-review dates with margin on either side.

**Service fee mechanics.** Exploratory analysis showed that service fee is a deterministic linear function of price (about 20 percent). This confirms Airbnb's published fee structure rather than reflecting a market-driven correlation, so no correlation-based conclusions are drawn from this column.

**Review rate scale.** The review rate number column is interpreted as a 1-to-5 average rating per listing, supported by the observed value range and consistent with Airbnb's public rating scheme.

## Known limitations

- **Single-source snapshot.** All findings come from one point-in-time export, so time-based analysis is confounded by listing churn.
- **No data dictionary.** Some columns are interpreted from context; if those interpretations are wrong, derived metrics are correspondingly affected.
- **Uneven geographic granularity.** Borough-level findings are robust; neighbourhood-level findings would need minimum-sample filtering to be reliable.
- **Free-text fields excluded.** House rules and listing names were not analyzed.
