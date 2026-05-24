# Data Dictionary

A column-by-column reference for the cleaned dataset, grouped by the kind of information each field carries.

## Geography

| Column | Description |
|---|---|
| borough | One of the five NYC boroughs (Manhattan, Brooklyn, Queens, Bronx, Staten Island) |
| neighbourhood | The neighbourhood within a borough |
| lat | Latitude coordinate of the listing |
| long | Longitude coordinate of the listing |
| country | Country (all values are United States) |
| country code | Country code |

## Pricing

| Column | Description |
|---|---|
| price | Nightly price in US dollars |
| service fee | Airbnb service fee; found to be roughly 20 percent of price (a billing rule, not a market signal) |

## Host information

| Column | Description |
|---|---|
| host id | Unique identifier for the host |
| host name | Display name of the host |
| host_identity_verified | Whether the host has completed identity verification (true or false) |
| calculated host listings count | Number of listings operated by the host |

## Booking conditions

| Column | Description |
|---|---|
| room type | One of: Entire home/apt, Hotel room, Private room, Shared room |
| cancellation_policy | The cancellation policy category (plus an Unknown bucket for nulls) |
| instant_bookable | Whether the listing can be booked instantly without host approval |
| minimum_nights | Minimum number of nights required per booking |
| availability_365 | Number of days per year the listing is available for booking |
| construction year | Reported construction year of the property |

## Review activity

| Column | Description |
|---|---|
| number of reviews | Total review count for the listing |
| reviews per month | Average reviews received per month |
| last review | Date of the most recent review |
| review rate number | Average guest rating, interpreted on a 1-to-5 scale |

## A note on interpretation

The dataset shipped without an official data dictionary. Several columns (review rate number, calculated host listings count, construction year) are interpreted from context and from Airbnb's publicly documented platform behaviour. These interpretations are stated in the [data cleaning log](data_cleaning.md) so the assumptions behind every derived metric are transparent.
