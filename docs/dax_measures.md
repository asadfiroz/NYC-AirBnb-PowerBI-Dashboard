# DAX Measures

Nine measures were authored in a dedicated `_Measures` table. Centralizing them keeps the analytical logic in one place and keeps the fact table free of calculated-column clutter. Each measure was formatted appropriately (currency, percentage, or whole number) so visuals render correctly without further intervention.

| Measure | Definition | Purpose |
|---|---|---|
| Total Listings | `COUNTROWS(fact_listings)` | Top-line market-size KPI |
| Avg Price | `AVERAGE(fact_listings[price])` | Headline pricing metric |
| Median Price | `MEDIAN(fact_listings[price])` | Outlier-resistant pricing alternative |
| Avg Availability | `AVERAGE(fact_listings[availability_365])` | Host commitment and demand proxy |
| Total Reviews | `SUM(fact_listings[number of reviews])` | Engagement and activity indicator |
| Distinct Hosts | `DISTINCTCOUNT(fact_listings[host id])` | Supply-side diversity |
| Host Verification Rate | Verified divided by total, using `CALCULATE` and `DIVIDE` | Trust and platform integrity |
| Instant Book Rate | Instant-book divided by total | Frictionless-booking adoption |
| Avg Review Rate | `AVERAGE(fact_listings[review rate number])` | Quality and satisfaction signal |

## Notes on measure choices

**Why both Avg Price and Median Price.** The average is skewed upward by a long tail of luxury listings, which makes prices look almost identical across boroughs. The median is included as an outlier-resistant alternative that recovers the more typical, Manhattan-led pricing pattern. Having both lets a viewer see the dispersion rather than a single misleading number.

**Occupancy Proxy.** An additional measure, an `AVERAGEX` over `(365 minus availability_365) divided by 365`, was built into the model as an approximate annual booked share. It is available in the model for further analysis even though it is not surfaced on the single-page dashboard.
