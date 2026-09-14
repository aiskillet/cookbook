---
name: data-validation
description: Catch bad data before it corrupts pipelines — schema checks, constraints, and quality tests at ingestion. Use when building data pipelines/ETL or debugging bad downstream data.
---

# Data Validation

Bad data fails silently and corrupts everything downstream — reports, models, decisions. Validate at the boundary, fail loudly, and make data quality a test, not a hope.

## When to Activate
- Building an ingestion pipeline / ETL / data load
- A downstream metric or model looks wrong (garbage upstream)
- Defining a data contract between producer and consumer

## Validate at ingestion (the boundary)
Check data the moment it enters your system, before it's written to the warehouse — the earlier you catch it, the cheaper the fix.

## What to check
- **Schema:** expected columns/fields, correct types, no unexpected nulls.
- **Constraints:** ranges (age ≥ 0), enums (status ∈ set), formats (email/date), uniqueness (no dup primary keys), referential integrity (foreign keys resolve).
- **Volume/freshness:** row counts within expected bounds (a 90% drop = upstream broke); data is recent enough.
- **Distribution drift:** key stats (nulls %, mean, cardinality) haven't shifted anomalously vs. history.

## How to handle failures
- **Fail loud, fail early.** Block or quarantine bad batches; don't silently write them. A visible failure beats a corrupted dashboard nobody questions.
- **Quarantine + alert** rather than drop — you need to see and fix the source.
- **Data contracts:** agree the schema/SLAs with the producer; validate against the contract so breakage is caught at the source.

## Practically
- Use a validation framework (Great Expectations, Pandera, dbt tests, or JSON Schema) — declarative checks beat ad-hoc `if`s.
- Run checks in CI for pipeline code **and** as runtime gates on data.
- Log rejected rows with the reason for debugging.

## Checklist
- [ ] Validated at ingestion, before writing downstream
- [ ] Schema + type + null checks
- [ ] Constraints: ranges, enums, uniqueness, referential integrity
- [ ] Volume/freshness/drift monitored
- [ ] Bad batches quarantined + alerted, not silently dropped
