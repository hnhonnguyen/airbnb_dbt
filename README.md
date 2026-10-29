# airbnb_dbt

A dbt project modelling the Airbnb listings dataset: layered staging →
dimensional models → a mart, with schema tests, an SCD2 snapshot, and a seed
for full-moon review analysis.

## Model layers

| Layer | Directory | Materialization | Models |
| --- | --- | --- | --- |
| Staging | `models/src` | ephemeral | `src_listings`, `src_hosts`, `src_reviews` |
| Dimensions & facts | `models/dim`, `models/fact` | table / view | `dim_listings_cleansed`, `dim_hosts_cleansed`, `dim_listings_w_hosts`, `fact_reviews` |
| Mart | `models/mart` | view | `mart_fullmoon_reviews` |

## Supporting pieces

- `seeds/seed_full_moon_dates.csv` — full-moon calendar feeding the mart
- `snapshots/scd_raw_listings.sql` — SCD2 history for raw listings
- `macros/no_nulls_in_columns.sql`, `macros/positive_value.sql` — generic test
  macros
- `tests/` — singular tests for the dimension tables

## Usage

```bash
dbt deps
dbt seed
dbt run
dbt test
dbt snapshot
```

Requires a dbt profile named `airbnb_dbt` pointing at a warehouse seeded with
the source tables (listings, hosts, reviews) defined in `models/sources.yml`.

## Notes

This project follows the staging → dimensional → mart structure common in dbt
courses — a compact example of layered modelling with tests and snapshots.
