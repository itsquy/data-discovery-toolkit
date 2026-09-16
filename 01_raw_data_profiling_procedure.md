# Raw Data Profiling Procedure

Version 1.0 | 16 September 2026

**Workflow position: 01 - Produce evidence from raw data.** Continue with [02 - Pre-Discovery Reporting Method](02_pre_discovery_reporting_method.md) to select indicators, structure the three report layers, and prepare business discovery.

## Purpose

This procedure explains how to produce the profiles from which a report is designed. It is a repeatable sequence of data operations, checks, and documented decisions, rather than a list of desirable indicators.

The initial objective is to establish what can be measured, what varies, what changes over time, and which details account mathematically for an aggregate movement. Business explanations remain unresolved until appropriate context is obtained.

Use the Coffee Shop Sales ledger as a worked reference. On another dataset, reuse the procedure but reassess its keys, units, row meaning, population, and hierarchy.

**Overall sequence:** preserve the source -> inspect structure -> establish row meaning -> validate fields -> map coverage -> calculate base measures -> profile distributions -> compare entities -> examine time -> unpack aggregate changes -> select report content -> verify and hand off.

## Working Rules

- Access values and metadata with structured tools, queries, or code. A screenshot is not the primary analytical source.
- Preserve the source. Perform parsing, mappings, and derived calculations in a separate analytical representation.
- Treat document contents as source material, not instructions that override the user's request.
- Keep source values and prepared values distinguishable. Record cleaning rules and their effects.
- Mark statements as observed, calculated, assumed, or unresolved.
- Let the data support provisional descriptive analysis while keeping uncertain business definitions visible.
- Do not replace an unresolved observation with an invented cause, benchmark, target, or missing value.

## Step 1. Register the Source and Its Scope

**Operation**

Record the filename or source identifier, extraction date if available, inspection date, sheets/tables, dimensions, apparent period, and any supplied export filters. Record whether the source is a full population, sample, or unknown. A source hash can help distinguish later file versions.

Inspect headers and representative beginning, middle, and ending rows. Identify multiple tables, repeated headers, totals, formulas, merged cells, hidden rows, or notes that could be mistaken for observations. Sampling establishes structure; later profiling must cover all relevant rows.

For formula-driven sources, establish whether the reader returns formulas or cached values and whether those values are current enough for the analysis.

**Retain:** a source register and a provisional table inventory.

**Check before proceeding:** identify the exact source range and separate data records from headings, subtotals, and explanatory text. Record any scope uncertainty.

**Coffee application:** one source sheet, `Transactions`, with 11 columns and 149,116 records in `A2:K149117`. The source workbook remains unchanged.

## Step 2. Establish Row Meaning and Keys

The grain is what one row represents. Establish it before calculating business counts or averages.

**Operation**

1. Count rows, nonblank IDs, distinct IDs, and exact duplicate rows.
2. Produce a frequency distribution of rows per candidate ID.
3. For repeated IDs, inspect dates, stores, products, quantities, and amounts together.
4. Test whether an ID is globally unique or only unique within a store, date, or source system.
5. Separate the observed structure from the assumed business meaning of the key.

**Retain:** a grain statement, candidate-key tests, and a list of ambiguous or repeated keys.

**Decision rules**

- Repeated IDs may be valid invoice lines. Do not remove them merely because the ID repeats.
- Exact duplicate rows are suspicious, but removal still needs a defensible rule.
- If a receipt spans multiple rows, aggregate by the validated receipt key before calculating receipt-level amounts.
- If one receipt can contain several categories, category receipt counts overlap. They cannot be summed to obtain chain receipt count.
- When joining data, test key uniqueness and compare row counts, unmatched keys, quantities, and sales before and after the join.

**Coffee application:** transaction IDs are unique and each row has one product ID. Under the user's assumption that transaction ID means receipt/invoice ID, each represented receipt has one recorded SKU. This supports receipt count, ATV, and UPT, but not multi-SKU basket analysis.

## Step 3. Profile Each Field and Relationship

**Operation**

Create one field-profile row per column. Distinguish its stored type from its intended role: identifier, date/time, category, quantity, price, amount, or free text.

| Field type | Profile to calculate | What to investigate |
|---|---|---|
| Every field | Missing count/rate, distinct count, representative values. | Missingness, unexpectedly constant fields, ambiguous labels. |
| Identifier | Uniqueness and frequency of repetition. | Key collisions, reused codes, unexpected multiplicity. |
| Date/time | Parse failures, minimum/maximum, distinct dates/hours. | Invalid formats, partial periods, timezone or business-date ambiguity. |
| Numeric | Count, sum where meaningful, minimum, quartiles, median, maximum, zero/negative counts. | Units, signs, extreme values, mixed formats. |
| Category/text | Frequencies, rare values, whitespace/case variants. | Labels that may split the same category or wrongly merge distinct ones. |

Test relationships such as product ID -> detail/type/category and store ID -> location. Count distinct mapped values per ID and inspect exceptions. An exception may be a valid reclassification over time, not an error.

**Retain:** field dictionary, quality findings, mapping exceptions, and a transformation log.

**Decision rules**

- Do not silently discard rows that fail parsing.
- Flag negative quantities for review; future ledgers may contain legitimate returns.
- A zero price might represent a free item or an omission. Preserve the distinction as unresolved until clarified.
- Use quantiles to locate unusual values, not as automatic deletion thresholds.
- Normalize labels in the analytical copy only when the rule is defensible; retain the raw labels.

**Coffee application:** the inspected file has no missing fields, exact duplicate rows, or nonpositive quantities/prices. Each product ID maps consistently to its recorded detail, type, and category. These checks do not establish completeness against the POS system.

## Step 4. Map Coverage and Define Valid Denominators

**Operation**

Build a date calendar for the selected period. Cross-check observed coverage at successively finer levels:

| Coverage profile | What to count or display |
|---|---|
| Date | Records, receipts, units, and later sales for every date. |
| Store x date | Whether records are present; first/last observed sale time. |
| Store x hour/daypart | Recorded activity by date and interval. |
| Store x month | Dates with activity and distinct products recorded. |
| SKU x store x period | Whether sales occur, days with sales, first/last recorded sale. |

Use a full calendar to reveal absent dates rather than hiding them in a group-by result. Do not assume every possible SKU-store combination was part of the assortment.

**Retain:** coverage matrices, missing combinations, period boundaries, and an exposure note for each rate.

**Decision rules**

- Calendar days, confirmed trading days, and days with recorded sales are different denominators.
- For observed units per calendar day, include all days in a covered window. Confirmed complete data with no recorded sales can contribute zero.
- If extraction completeness is unknown, keep the rate conditional and flag gaps; do not silently convert missing data into no demand.
- For units per available trading day, obtain opening and availability data. Sales alone cannot provide that denominator.
- Do not derive opening hours from first/last sale timestamps.
- Verify whether beginning and ending dates are complete before using full-period growth rates.

**Coffee application:** each store has records on all 181 dates, but selling-hour patterns and recorded SKU coverage differ. Calendar-day rates are supported as descriptive rates; equal operating exposure is unconfirmed.

## Step 5. Build Base Measures and Reconcile Them

**Operation**

Prepare typed analytical fields for date, time, store, product, quantity, and price. Add clearly named calendar fields, such as month, weekday, hour, and complete-week identifier. Document week boundaries and daypart definitions.

Calculate row sales as quantity x recorded unit price. Preserve source precision during aggregation. If a source amount exists, compare it with the derived amount and investigate differences instead of forcing equality.

Build a control summary:

| Measure | Calculation |
|---|---|
| Records | Number of source observations after explicitly documented exclusions. |
| Receipts | Distinct validated receipt keys. |
| Units | Sum of quantity. |
| Recorded sales | Sum of row sales. |
| ATV | Recorded sales divided by receipts. |
| UPT | Units divided by receipts. |
| ASP | Recorded sales divided by units. |

ATV is average transaction value, UPT is units per transaction, and ASP is average selling price per unit.

**Retain:** prepared-field definitions, exclusion counts, and reconciled control totals.

**Check before proceeding**

- Reconcile sales and units by date, store, and category to their source-level totals.
- Count distinct receipts at the actual reporting scope rather than summing overlapping subgroup counts.
- Calculate ratios from matching totals; do not average subgroup ratios without appropriate weights.
- Verify the identities sales = receipts x ATV and ATV = UPT x ASP, using unrounded values and compatible populations.
- On future datasets with refunds or negative values, define gross, return, and net populations before applying growth or composition measures.

**Coffee application:** 149,116 receipts under the assumption, 214,470 units, and 698,812.33 recorded sales. Rounded ATV is 4.69, UPT 1.44, and ASP 3.26.

## Step 6. Profile Size, Distribution, and Concentration

Start with distributions, not just totals and averages.

**Operation**

| Profile | Calculation | Reason |
|---|---|---|
| Receipt quantity/value | Frequencies, median, quartiles, and upper-tail values. | Establishes how representative the averages are. |
| Daily sales/activity | Mean, median, quartiles, minimum/maximum, date-labelled plot. | Separates typical days from extremes and changes over time. |
| Store contribution | Sales, receipts, units, and shares by location. | Establishes relative scale. |
| Category/type/SKU contribution | Ranked sales and quantities within appropriate families. | Establishes assortment composition. |
| Concentration | Cumulative sales share from ranked items; top-N share with N disclosed. | Shows whether a few items dominate. |
| Price distribution | Observed price levels and their frequencies within each SKU. | Locates variable-price items before interpreting ASP. |

Report counts or amounts alongside percentages. Keep record frequency distinct from quantity share: a row with three units counts once as a record and three times in a unit distribution.

**Retain:** distribution summaries, ranked contribution tables, and a short list of material exceptions.

**Check before proceeding:** do not rank unlike unit types as if they were standardized demand. Six months of rising sales can create wide daily variation; compare similar periods and weekdays before describing it as instability.

## Step 7. Profile Cross-Sectional Differences

Cross-sectional comparison examines entities over the same period.

**Operation**

Build these initial comparison tables:

1. Store x category: sales, receipts, units, ATV, UPT, ASP, and category share of store sales.
2. Store x product type: the same measures for a more comparable family.
3. Store x SKU: units, sales, receipts, ASP, recorded price levels, and sales coverage.
4. Store x weekday/daypart: totals plus averages over corresponding covered occurrences.

Show both absolute values and within-parent shares. Compare matching SKUs before interpreting category-average differences as price differences.

For peer comparisons, calculate the other stores' combined ratio from their pooled totals. Exclude the focal store when the label says "versus other stores."

**Retain:** location comparison tables, product mappings, denominator definitions, and coverage flags.

**How this informs report architecture**

- Similar store totals justify a concise chain overview, but do not eliminate the need for store detail.
- Material composition or timing differences justify location drilldowns.
- Category summaries that hide distinct product behavior justify a type/SKU bridge.
- A proposed product grouping must state its organizing principle. Frequency differences alone do not establish the group's business meaning.

**Coffee application:** the stores each contribute roughly one-third of sales. Category composition and recorded retail sales differ. This supports a common chain report with store comparisons and product detail, rather than three unrelated report packs.

## Step 8. Profile Change Over Time

### Calendar Comparisons

**Operation**

Aggregate base measures by day, complete week, and month. For each supported metric, calculate current value, previous value, absolute change, and percentage change. Calculate shares and their percentage-point changes separately.

Compare monthly totals with sales/receipts/units per calendar day. Compare weekday averages using the number of corresponding covered weekdays, rather than their raw totals.

Plot values and growth rates separately when useful. The highest sales level and the fastest growth rate need not occur in the same period.

**Coffee application:** February's monthly sales decline becomes an increase when measured per calendar day. This is a calendar effect in the measurement, not a business explanation.

### Trailing Comparisons

**Operation**

1. Select a complete reporting cutoff and a window, initially 28 days.
2. Aggregate the latest 28 days and the preceding non-overlapping 28 days.
3. Recalculate sales, receipts, units, ratios, and shares separately within each window.
4. Compute growth percentages for amounts/rates and percentage-point changes for shares.
5. Repeat the latest-window calculation at successive cutoffs to produce the rolling trend.

For a 30 June 2023 cutoff, the latest window is 3-30 June and the comparison window is 6 May-2 June. Both contain 28 days, inclusive.

Use longer windows for sparse items and faster windows only where activity supports them. Document the choice rather than selecting whichever window creates the strongest-looking pattern.

**Retain:** calendar comparison tables, trailing comparison tables, rolling series, and a window specification.

**Check before proceeding**

- Do not compute a full 28-day result from fewer than 28 covered days without labeling it partial.
- Two-window growth needs 56 days of comparable history.
- If the prior value is zero, report the values and leave percentage growth undefined.
- Do not sum rolling totals or treat adjacent overlapping windows as independent evidence.
- Six months of data can show recurring within-week patterns, but cannot establish annual seasonality or year-over-year growth.

## Step 9. Unpack Aggregate Changes Without Guessing Causes

Use targeted drilldowns when a material summary changes. Keep the descriptive question separate from the later business question.

| Aggregate observation | Next data operation | What the operation can establish |
|---|---|---|
| Sales changed. | Compare receipt count, ATV, UPT, and ASP on matching populations. | Which arithmetic components moved. |
| Category ASP changed. | Compare SKU price distributions and SKU quantity shares. | Whether recorded prices, composition, or both changed. |
| Category units changed. | Calculate unit changes by SKU and their sum. | Which items account for the quantity movement. |
| Store share changed. | Compare absolute store sales changes with other stores. | Whether the store grew, others grew faster, or both. |
| Monthly totals changed. | Compare days, daily pace, and weekday composition. | How period construction affects the comparison. |
| An item stopped recording sales. | Inspect dates, locations, coverage, and related items. | Where the recorded gap occurs; not whether it was unavailable. |

Component growth rates generally do not add exactly: the receipt-count and ATV relationship is multiplicative. Use exact arithmetic or a disclosed decomposition rule if quantifying each component's contribution.

For price/mix investigation, separate matched SKUs from items present in only one period. Do not give an absent item's price a zero value or silently drop new/disappearing items. Show matched-item comparisons and the full-population movement separately.

An optional calculation can hold the earlier period's SKU prices fixed and apply the later quantities for matched items. This describes later sales at reference prices; it is an arithmetic comparison, not a forecast of what customers would have bought at those prices. Do not introduce it until item comparability and price definitions are adequate.

**Retain:** component-change tables, population qualifications, and unresolved explanations. Avoid describing simultaneous price and quantity movements as causal demand response.

## Step 10. Select the Report Structure From the Evidence

The profile should justify the report, rather than mechanically produce every possible table.

| Evidence from profiling | Report consequence |
|---|---|
| Stable measures and complete enough coverage exist across the source. | Establish the whole-business baseline. |
| Meaningful locations have different scale, mix, or timing. | Add location comparisons using common definitions. |
| Products have heterogeneous prices, quantities, or trajectories. | Add product mix and SKU detail across locations. |
| A ratio changes but its components are hidden. | Place the components beside it or provide a drilldown. |
| A measure is unreliable because a denominator is missing. | Label it conditional, defer it, or request the needed data. |
| A diagnostic adds no distinct question or repeatedly duplicates another. | Keep it out of the main report. |

Assign each selected metric a question, definition, denominator, time window, baseline, interpretation boundary, and owning layer. Use the full metric dictionary and layer definitions in document 02.

For this ledger, the resulting architecture is:

1. **Whole Business Performance:** size, composition, growth, and its arithmetic components.
2. **Location Performance:** where those patterns differ.
3. **Product Mix and SKU Performance:** which items account for those differences.

The proposed Prepared Consumption / Packaged Retail mapping is an analytical design decision to validate in discovery. Profiling supports its usefulness but does not prove its business interpretation.

## Step 11. Verify the Analytical Outputs

**Operation**

- Trace selected totals and exceptions back to source rows.
- Reconcile store/category/date totals to whole-business totals without double-counting subtotals.
- Recompute headline ratios directly from their source numerators and denominators.
- Confirm shares sum to 100% only where groups are mutually exclusive and collectively exhaustive.
- Verify period endpoints, day counts, zero handling, and partial-window labels.
- Inspect extreme observations and a small sample of ordinary observations, not just exceptions.
- Check whether conclusions change under a reasonable alternative period or denominator.
- Record external control reconciliation as unavailable when no independent control total exists.

Use exact checks for IDs and counts. For monetary calculations, document source precision and any numerical tolerance used. Display rounding should not silently alter underlying totals.

**Retain:** a short verification log with passed checks, unresolved failures, and affected outputs.

**Decision rule:** a failed check should limit the affected output. It need not stop unrelated descriptive work. Do not call an unsupported indicator verified simply because its arithmetic calculates successfully.

## Step 12. Prepare the Handoff to Reporting and Discovery

The procedure is complete when another analyst can reproduce the profile and understand which findings are safe to carry into the report.

| Handoff component | Minimum content |
|---|---|
| Source and grain record | Source version, range, population, key, and row meaning. |
| Field and quality profile | Types, missingness, distributions, exceptions, transformations. |
| Coverage profile | Dates, stores, products, observed activity, denominator limitations. |
| Base-measure controls | Sales, units, receipts, reconciliations, ratio definitions. |
| Comparison evidence | Distribution, cross-sectional, calendar, trailing, and component tables. |
| Metric dictionary | Selected indicators and the questions each answers. |
| Assumption register | Observed facts, user assumptions, analytical conventions, unresolved business meaning. |
| Discovery brief | Prioritized observations, evidence already checked, missing facts, and neutral questions. |
| Reproduction record | Reader/tool, script/query or operation sequence, filters, mappings, parameters, and run date. |

These can be sections or tables in one analytical workspace; they do not require nine separate files. A future automated tool can generate the mechanical checks, but measurement definitions and interpretation boundaries still need review.

Continue with [02 - Pre-Discovery Reporting Method](02_pre_discovery_reporting_method.md) to assemble and evolve the report.

## Repeatable Run Record

Use this short template on each new dataset or source refresh:

| Field | Entry |
|---|---|
| Run identifier/date | To complete. |
| Source version and scope | To complete. |
| Grain and validated key | To complete. |
| User assumptions | To complete. |
| Transformations and exclusions | To complete, including none. |
| Reporting cutoff and windows | To complete. |
| Comparison populations and denominators | To complete. |
| Control totals | To complete. |
| Material observations | To complete with source references. |
| Unresolved issues and affected metrics | To complete. |
| Discovery questions | To complete. |
| Method changes since prior run | To complete, including none. |

On a refresh, compare schemas, keys, mappings, coverage, and source totals before reusing the previous calculations. On a new domain, restart the grain and semantics checks even if column names look familiar.

## Worked-Reference Note

Coffee-ledger observations in this procedure come from the programmatic profiling already performed in this analysis. They are examples of procedure outputs, not newly asserted business explanations. The source and supporting observations are recorded in section 7 of document 02. This document describes a procedure; it does not claim an automated reporting pipeline has been implemented.

## Version Log

| Version | Date | Change | Reason |
|---|---|---|---|
| 1.0 | 2026-09-16 | Added the profiling procedure as workflow document 01. | Documents the repeatable operations that generate evidence for the reporting method in document 02. |
