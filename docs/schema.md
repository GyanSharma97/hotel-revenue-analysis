# Data Dictionary

## Fact: `hotels` (CTE = 2018 ∪ 2019 ∪ 2020)

| # | Column | Type | Description |
|---|---|---|---|
| 1 | hotel | text | `City Hotel` or `Resort Hotel` |
| 2 | is_canceled | int (0/1) | 1 if the booking was cancelled |
| 3 | lead_time | int | Days between booking and arrival |
| 4 | arrival_date_year | int | Arrival year |
| 5 | arrival_date_month | text | Arrival month name |
| 6 | arrival_date_week_number | int | ISO week of arrival |
| 7 | arrival_date_day_of_month | int | Day of arrival |
| 8 | stays_in_weekend_nights | int | Saturday/Sunday nights booked |
| 9 | stays_in_week_nights | int | Monday–Friday nights booked |
| 10 | adults | int | Number of adults |
| 11 | children | float | Number of children (contains nulls) |
| 12 | babies | int | Number of babies |
| 13 | meal | text | Meal plan → **FK to `meal_cost.meal`** |
| 14 | country | text | ISO-3 country of origin |
| 15 | market_segment | text | Booking segment → **FK to `market_segment.market_segment`** |
| 16 | distribution_channel | text | e.g. TA/TO, Direct, Corporate |
| 17 | is_repeated_guest | int (0/1) | Returning guest flag |
| 18 | previous_cancellations | int | Guest's prior cancellations |
| 19 | previous_bookings_not_canceled | int | Guest's prior completed bookings |
| 20 | reserved_room_type | text | Room type code reserved |
| 21 | assigned_room_type | text | Room type code assigned |
| 22 | booking_changes | int | Number of amendments |
| 23 | deposit_type | text | No Deposit / Non Refund / Refundable |
| 24 | agent | text | Travel agent ID (`NULL` as text when absent) |
| 25 | company | text | Company ID (`NULL` as text when absent) |
| 26 | days_in_waiting_list | int | Days on waiting list |
| 27 | customer_type | text | Transient, Contract, Group, Transient-Party |
| 28 | adr | float | Average Daily Rate |
| 29 | required_car_parking_spaces | int | Parking spaces requested |
| 30 | total_of_special_requests | int | Number of special requests |
| 31 | reservation_status | text | Check-Out, Canceled, No-Show |
| 32 | reservation_status_date | date | Date of last status change (used as the dashboard date axis) |

## Dimension: `market_segment`

| Column | Type | Description |
|---|---|---|
| market_segment | text (PK) | Segment name |
| Discount | float | Discount rate applied to the segment (0–1) |

## Dimension: `meal_cost`

| Column | Type | Description |
|---|---|---|
| meal | text (PK) | Meal plan code (BB, HB, FB, SC, Undefined) |
| Cost | float | Meal cost |

## Relationships

| From | To | Cardinality | Join |
|---|---|---|---|
| hotels.market_segment | market_segment.market_segment | many-to-one | LEFT JOIN |
| hotels.meal | meal_cost.meal | many-to-one | LEFT JOIN |

## Power BI model

In Power BI the joined result is loaded as a single table, `Query1`. These fields are added in Power BI:

| Field | Likely definition |
|---|---|
| Revenue | `(stays_in_week_nights + stays_in_weekend_nights) * adr` (may also apply `(1 - Discount)`) |
| Total Nights | `stays_in_week_nights + stays_in_weekend_nights` |
| Parking Percentage | `SUM(required_car_parking_spaces) / Total Nights` |

*The DAX in the .pbix is stored compressed, so these definitions are inferred from field names and report usage.*
