# Data Model

The cleaned flat table was decomposed into a classic Kimball star schema: one central fact table surrounded by five dimension tables. This design separates the additive, measurable facts (price, fees, availability, review counts) from the descriptive context that surrounds them (host attributes, geography, room type, cancellation policy, time).

![Star schema](../assets/star_schema.png)

## Why a star schema

A single denormalized table is easy to build but hard to analyze cleanly. It repeats descriptive values on every row, makes filtering ambiguous, and slows down aggregations. Splitting the data into a fact table plus dimensions gives each kind of information its own home, keeps the fact table narrow and fast, and makes the relationships in the model explicit and predictable.

## Tables

**fact_listings** is the central fact table. It retains the listing identifier, the foreign keys to all dimensions, and the measurable attributes: price, service fee, minimum nights, number of reviews, reviews per month, review rate number, availability (days per year), instant bookable flag, construction year, latitude, and longitude.

**dim_host** holds host details: host id (key), host name, identity verification status, and calculated listings count. It was built by referencing the fact table, removing duplicates on host id, and resolving conflicts with a first-row-wins rule where the same host id appeared with different values.

**dim_neighbourhood** provides geography: neighbourhood (key), borough, and a surrogate neighbourhood id. It supports a borough-to-neighbourhood drill-down hierarchy.

**dim_room_type** is a small lookup table covering the four observed room types: Entire home/apt, Hotel room, Private room, and Shared room.

**dim_cancellation_policy** covers the three observed Airbnb policy categories plus an Unknown bucket for nulls.

**dim_date** spans 1 January 2010 to 31 December 2026, generated programmatically in Power Query M code and marked as a date table in Power BI to enable time-intelligence functions. It contains date (key), year, quarter, month number, month name, and year-month.

## Relationships

| From (fact) | To (dimension) | Cardinality | Filter direction |
|---|---|---|---|
| fact_listings[host id] | dim_host[host id] | Many-to-one | Single |
| fact_listings[neighbourhood] | dim_neighbourhood[neighbourhood] | Many-to-one | Single |
| fact_listings[room type] | dim_room_type[room type] | Many-to-one | Single |
| fact_listings[cancellation_policy] | dim_cancellation_policy[cancellation_policy] | Many-to-one | Single |
| fact_listings[last review] | dim_date[Date] | Many-to-one | Single |

All five relationships are single-direction. Bidirectional (both-way) filtering was deliberately avoided. While it is occasionally convenient, it introduces ambiguity in the filter context that can produce silently incorrect aggregations.  
