# A Pre-Discovery Reporting Method

Version 1.1 | 16 September 2026

**Workflow position: 02 - Design and evolve the report.** Start with [01 - Raw Data Profiling Procedure](01_raw_data_profiling_procedure.md), which explains how to produce the evidence used here. This document defines how that evidence becomes a report and a business-discovery brief.

## 1. Purpose and Scope

Use this method when raw data arrives before a full understanding of the business. Its output is a structured account of what the data shows, how its measurements behave, and which questions should be taken into business discovery.

The method has three parts:

1. **Profile the evidence:** establish structure, coverage, definitions, and limitations.
2. **Build a descriptive report:** organize size, composition, change, and variation into useful levels of detail.
3. **Prepare discovery:** identify the business facts needed to interpret the observed patterns.

The goal is not to enumerate every possible story. It is to examine materially different aspects of the data without turning a numerical pattern into an unsupported business explanation.

This guide uses the Coffee Shop Sales ledger as its worked application. Its three report layers are Whole Business Performance, Location Performance, and Product Mix and SKU Performance. Other datasets may require different entities, but the reasoning checkpoints remain useful.

P&L, profitability, customer retention, and inventory efficiency are outside the present report because the ledger lacks the required inputs.

## 2. Foundational Rules

### Distinguish Four Types of Statements

| Statement type | Example | What makes it acceptable |
|---|---|---|
| Observation | Category average revenue per unit fell while units increased. | Directly calculated from defined records and periods. |
| Mathematical accounting | The average depends on individual recorded prices and the proportions sold. | Follows from how the measure is constructed. |
| Business hypothesis | A promotion may have influenced purchasing. | Explicitly provisional; competing explanations remain possible. |
| Business conclusion | A promotion caused incremental purchases. | Requires business context and evidence beyond coincident movements. |

Pre-discovery reporting should emphasize the first two. Record hypotheses only when useful, and keep them separate from findings. A discovery question can remain neutral: "What explains the different recorded prices for this SKU?"

### A Measurement Is More Than a Name

For every indicator, specify:

- **Entity:** chain, store, category, product type, or SKU.
- **Population:** which records and products are included.
- **Measure:** sales value, receipts, units, price, or a defined ratio.
- **Denominator:** the total, time exposure, or peer population used.
- **Time window:** calendar month, complete week, or trailing period.
- **Baseline:** the period or population used for comparison.
- **Interpretation boundary:** what the result does not establish.

For example, "SKU share" is incomplete. "SKU units as a percentage of category units at Astoria during the latest 28 days" is reproducible.

### Examine the Arithmetic Before Assigning Reasons

If an aggregate changes, identify what detail was combined to create it. Inspect those components before asking why they changed.

The sequence is:

**Observe -> define the calculation -> inspect components -> test comparability -> formulate discovery questions.**

This permits useful analysis before business discovery while preserving uncertainty about business meaning and causes.

## 3. Profile the Raw Data

### Structural and Semantic Checks

| Check | What to establish | Why it affects reporting |
|---|---|---|
| Source and scope | Export origin, reporting period, filters, currency, and whether the source is operational or sample data. | Prevents treating an extract as the complete business. |
| Row meaning | What one row represents; whether IDs identify receipts, lines, products, or events. | Determines valid counts, averages, and joins. |
| Keys and relationships | ID uniqueness; mappings between products and categories or stores and locations. | Prevents duplication and ambiguous grouping. |
| Coverage | Dates, stores, products, observed selling hours, and missing combinations. | Determines whether comparisons have equivalent exposure. |
| Value quality | Missing fields, invalid dates, duplicate rows, nonpositive quantities, and unusual prices. | Identifies cases needing explanation rather than automatic deletion. |
| Recorded price behavior | Prices by SKU, store, and date; quantities sold at each price. | Distinguishes recorded price variation from aggregate mix movement. |
| Reconciliation | Agreement with any available POS control total or source summary. | Tests completeness beyond internal arithmetic consistency. |
| Absent information | Costs, receipts with linked lines, availability, customers, discounts, tax, or operating hours. | Defines unsupported indicators and discovery priorities. |

A clean table is not necessarily a complete or correctly understood business record. An unexpected value is an investigation item, not automatically an error.

### Assumption Register for This Ledger

| Item | Current status | Reporting consequence |
|---|---|---|
| Transaction ID identifies a receipt or sales invoice. | User-specified assumption. | Receipt count, ATV, and UPT may be used on this basis. |
| Every transaction ID appears once, with one product ID. | Observed in the file. | Each represented receipt contains one recorded SKU, potentially multiple units. |
| Quantity multiplied by unit price represents recorded sales. | Calculation convention. | Do not label it net sales or tax-exclusive revenue without confirmation. |
| Product ID is the SKU comparison key. | Working convention; names/categories map consistently in the file. | Compare IDs, while confirming unrecorded pack-size or variant differences where prices vary. |
| Dates with sales represent observed activity. | Observed. | They do not establish scheduled opening hours or full-day operation. |
| Prepared Consumption and Packaged Retail are analytical groups. | Proposed mapping. | Confirm category roles during discovery, particularly Flavours. |

Receipt count is purchase count, not unique customer count. This extract has no multi-SKU receipt baskets from which to derive cross-category attachment. Do not invent basket links from similar timestamps.

## 4. Understand the Measurement Components

Use plain arithmetic identities to organize investigation. These identities account for totals; they do not prove causes.

| Measurement relationship | What to examine when the total changes |
|---|---|
| Sales = receipts x average transaction value | Purchase count and value per purchase. |
| Average transaction value = units per transaction x average selling price per unit | Quantity per purchase and value per unit. |
| Monthly sales = number of calendar days x sales per calendar day | Period length and daily pace. |
| Category sales = sum of the sales of its SKUs | Which products contribute to the movement. |
| Average selling price = total sales divided by total units | Individual recorded prices and the proportions of units sold at those prices. |

ATV means average transaction value. UPT means units per transaction. ASP means average selling price per unit. Calculate ratios from the totals for the selected population, rather than averaging store or daily averages without appropriate weights.

### Why an Average Can Move Without Individual Price Changes

| Product | Unchanged price | Period A units | Period B units |
|---|---:|---:|---:|
| Basic coffee | 4.00 | 50 | 75 |
| Premium coffee | 6.00 | 50 | 25 |
| Total units | | 100 | 100 |
| Total sales | | 500.00 | 450.00 |
| Sales divided by units | | 5.00 | 4.50 |

The observed average fell because the proportions changed. The example establishes that a lower average does not uniquely identify lower individual prices. It does not explain why the proportions changed.

For a real investigation, compare each SKU's prices and quantity shares across periods. A promotion, preference change, or availability issue belongs to later business explanation.

## 5. The Three-Layer Report

Use one set of metric definitions across the three layers. A lower layer supplies detail behind the higher layer rather than creating an independent version of the same total.

### Layer 1: Whole Business Performance

**Question:** What is the size, composition, and direction of recorded business activity?

| Indicator | Definition | Reason for inclusion |
|---|---|---|
| Recorded sales | Sum of quantity x recorded unit price. | Establishes scale and total movement. |
| Receipt count | Distinct transaction IDs under the receipt assumption. | Measures completed purchases represented in the extract. |
| Sales and receipts per calendar day | Period sales or receipts divided by calendar days. | Separates period length from daily pace. |
| ATV | Sales divided by receipts. | Shows value per purchase. |
| UPT | Units divided by receipts. | Shows quantity per purchase; interpret changes alongside product mix. |
| ASP | Sales divided by units. | Shows value per unit; interpret with price and mix detail. |
| Product-group sales share | Group sales divided by whole-business sales. | Shows composition and shifts between the two product groups. |
| Contribution to sales change | Each store/group's sales change, in currency and as a share of total change. | Locates the components accounting for overall movement. |

Suggested presentation: a compact scorecard, six-month sales and daily-pace trends, a group-mix comparison, and a chart showing each component's contribution to the sales change.

Whole-business UPT and ASP combine unlike products. Use them to describe the recorded arithmetic, not as proof of service workload, pricing success, or customer behavior.

### Layer 2: Location Performance

**Question:** Where does the whole-business pattern hold, and where does it differ?

| Indicator | Definition | Reason for inclusion |
|---|---|---|
| Store sales and chain share | Store sales and store sales divided by chain sales. | Shows scale and changing contribution. |
| Store daily sales and receipt growth | Change in sales/receipts per calendar day. | Compares movement while accounting for different period lengths. |
| ATV, UPT, and ASP by store | Same definitions as Layer 1, restricted to the store. | Locates differences in the components of sales. |
| Growth gap versus peers | Store growth rate minus the growth rate of the other stores combined. | Shows whether its trajectory differs from the rest of the chain. |
| Local group/category sales mix | Group/category sales divided by store sales; compare with other stores. | Identifies composition differences hidden by similar store totals. |
| Weekday and daypart profile | Sales, receipts, and relevant units by weekday/time interval; shares and averages. | Identifies the timing and concentration of observed activity. |
| Daily variation | Median and middle 50% range within comparable weekdays and recent periods. | Shows how representative an average is. |

Suggested presentation: stores side by side, aligned trend charts, and a weekday/daypart heatmap. Use the same dates and scales for comparisons.

Compute peer ratios from pooled numerators and denominators. Do not average store growth rates or ATVs blindly. Compare complete weekdays and consistent dayparts; a raw total can be larger simply because more occurrences are included.

Do not infer opening hours from the first and last sale. Equal calendar-day coverage does not establish equal selling hours, store capacity, or customer opportunity.

### Layer 3: Product Mix and SKU Performance Across Locations

**Question:** Which product components account for changes, and how do the same products differ across stores?

Use two views within this layer:

1. **Mix Overview:** group, category, and product-type composition and change.
2. **SKU Comparison:** selected product IDs compared across locations and periods.

The hierarchy is **group -> category -> product type -> SKU**. Keep all locations available in the SKU comparison, even when the investigation began in one store.

| View / Indicator | Definition | Reason for inclusion |
|---|---|---|
| Mix: sales share | Component sales divided by explicitly named parent sales. | Shows where sales value is concentrated. |
| Mix: unit share | SKU units divided by units in a comparable category or product type. | Reveals quantity composition and movements that can affect ASP. |
| Mix: concentration | Sales share of a fixed number of leading SKUs within a defined family. | Shows whether performance is broad or concentrated in a few items. |
| SKU: sales and quantity | Recorded sales and units, with changes and trends. | Separates value movement from quantity movement. |
| SKU: receipt count and UPT | Receipts containing the SKU; units divided by those receipts. | Separates purchase frequency from units per purchase. |
| SKU: price distribution | Recorded prices, units at each price, and associated dates/stores. | Identifies actual recorded price differences. |
| SKU: ASP | SKU sales divided by SKU units. | Summarizes realized recorded prices for a matching item. |
| SKU: units per calendar day | Period units divided by all calendar days in the selected period. | Provides a consistent observed sales rate, including zero-sale days. |
| SKU: contribution to parent sales change | SKU sales change and its share of category/type sales change. | Identifies items accounting for parent-level movement. |
| SKU: sales coverage | Days with sales, first/last recorded sale, and locations with sales. | Flags gaps requiring availability or assortment clarification. |

Use units within sensible product families. A syrup serving, beverage, bag of beans, and mug are different units. Do not interpret their pooled quantity as a standardized measure of demand or workload.

Sales coverage is a diagnostic, not an availability measure. No sale could mean no demand, no stock, no listing, a closed store, or missing data. Unknown coverage should remain unknown rather than silently becoming zero performance.

### Product-Group Mapping

| Group | Principle | Categories |
|---|---|---|
| Prepared Consumption | Products served for immediate consumption, including recorded modifiers. | Coffee, Tea, Bakery, Drinking Chocolate, Flavours. |
| Packaged Retail | Packaged take-home products and merchandise. | Coffee beans, Loose Tea, Packaged Chocolate, Branded. |

Keep Flavours identifiable as modifiers within the first group. This mapping organizes products by their intended role; it is not a statistical customer segmentation or evidence of the actual occasion of use.

### Avoid a Crowded Third Layer

- Show group/category composition before opening a SKU table.
- Filter by group, category, or type rather than ranking all 80 SKUs together by default.
- Keep price distributions and sales-coverage details as diagnostics behind the main SKU table.
- Show sales and units together; pair percentages with counts or amounts.
- Display shares at only one clearly named hierarchy level per column.
- Do not add category subtotals to the same grand total as their component SKU rows.

## 6. Percentages, Trends, and Trailing Windows

### Different Measurements Answer Different Questions

| Form | Definition | Meaning |
|---|---|---|
| Absolute change | Current value minus previous value. | Magnitude of movement. |
| Growth percentage | Current value divided by previous value, minus one. | Movement relative to the starting size. |
| Mix percentage | Component value divided by its parent total. | Relative importance within a population. |
| Mix change in percentage points | Current share minus previous share. | Change in relative importance. |
| Share of sales change | Component sales change divided by total sales change. | How much of the net movement that component accounts for. |
| Trend | A consistently calculated sequence across time. | Persistence, reversals, and changes in pace. |

A share moving from 10% to 12% increases by 2 percentage points. That is different from its relative increase of 20%. Use percentage points for mix comparisons.

Contribution percentages can exceed 100% or be negative when components move in opposite directions. When total change is near zero, prioritize the currency changes because the percentage becomes unstable.

### Integrate Trailing Windows Into Existing Indicators

Trailing is a time specification, not a new business measure. A useful recent-performance view is:

**Indicator | Latest 28 days | Previous 28 days | Absolute change | Change % or pp | Rolling trend**

Examples include sales over the last 28 days, receipt count over the last 28 days, and category sales share over the last 28 days. Preserve the original calculation and denominator.

For a reporting date of 30 June 2023:

- Latest 28 days: 3 June through 30 June, inclusive.
- Previous 28 days: 6 May through 2 June, inclusive.
- Growth compares those non-overlapping windows.
- A rolling trend recalculates the latest-28-day measure at successive reporting dates.

Recalculate ATV as window sales divided by window receipts. Recalculate shares from window totals. Do not take a simple average of daily ratios.

### Select Windows Deliberately

| Window | Suggested use | Limitation |
|---|---|---|
| Calendar month | Historical business review and reconciliation. | Months differ in length and weekday composition. |
| Complete week | Operational comparisons and recurring patterns. | Low-volume SKUs may fluctuate substantially. |
| Trailing 7 days | Faster signal for high-volume items. | Can be sensitive to events and sparse sales. |
| Trailing 28 days | Default recent-performance comparison across the three layers. | Smooths changes and can conceal short-lived movements. |

A 28-day window includes four of each weekday, but does not control for holidays, operating hours, or product availability. Adjacent rolling windows share many observations; their smoothness is partly mechanical and is not independent confirmation of a pattern. Do not add overlapping rolling totals together.

Use only completed windows, or clearly mark partial windows. The default reporting date is the last complete date in the extract, not today's date. This January-June file does not support year-over-year growth or annual seasonality conclusions.

If the previous value is zero, percentage growth is undefined. Report the actual values and label the comparison "no prior-period sales" where appropriate. If coverage is missing, label it unknown instead.

## 7. Evidence From the Original Ledger

Source: `Coffee Shop Sales.xlsx`, `Transactions!A1:K149117`. The source workbook is not included in this repository.

The source was inspected programmatically without modification. The checks below describe the inspected extract, not independent validation against the POS system.

| Verified observation | Implication for the report |
|---|---|
| 149,116 rows, 149,116 unique transaction IDs, 80 product IDs, three locations, January-June 2023. | Apply the receipt assumption explicitly and report the six-month scope. |
| No missing fields, duplicate rows, or nonpositive quantities/prices were found. | Structural checks passed; semantic and external completeness checks remain necessary. |
| Each location has recorded sales on all 181 calendar dates. | Calendar-day rates can be calculated without missing calendar dates in this extract. |
| Recorded sales total 698,812.33 and quantity totals 214,470. | Receipt-based ATV is approximately 4.69, UPT 1.44, and ASP 3.26. |
| February sales fell 6.8% versus January, while sales per calendar day rose 3.2%. | Show totals and normalized daily pace together. |
| Prepared Consumption accounts for 90.1% of sales and Packaged Retail 9.9%, using the mapping above. | Keep group shares consistent with the actual category membership. |
| Astoria records 79 SKUs; the other two locations record 80. | Flag unequal observed assortment before SKU benchmarking. |
| Fifteen product IDs have multiple recorded prices. | Include price-distribution diagnostics instead of assuming a fixed SKU price. |
| Recorded selling-hour patterns differ across locations and dates. | Obtain opening-hour context before hourly productivity comparisons. |

These findings justify the initial report design. They do not establish promotions, customer preferences, stockouts, or management effectiveness.

## 8. Turn the Report Into a Discovery Brief

Keep one investigation register alongside the report. Each entry should move from a precise observation to a specific missing business fact.

| Field | What to record |
|---|---|
| Observation | Metric, entity, dates, values, and source. |
| Arithmetic components checked | Prices, quantities, shares, counts, or exposure already examined. |
| Comparison conditions | Coverage, units, population, and period alignment. |
| Remaining uncertainty | The fact the ledger cannot resolve. |
| Discovery question | A neutral question that requests that fact. |
| Evidence source / owner | POS owner, store manager, product master, operating calendar, or another appropriate source. |
| Resolution | Definition confirmed, source corrected, comparison revised, or still unresolved. |

Examples:

| Observation | Discovery preparation |
|---|---|
| The same SKU has several recorded prices. | Map prices by date and location, then ask what determines the recorded price and whether pack sizes or adjustments are omitted. |
| A category average falls while its SKU quantity shares shift. | Quantify the share changes, then ask what changed in assortment, availability, or selling context. Do not lead with a promotion claim. |
| One store has no recorded sales of an item seen elsewhere. | Check the full period, then ask whether the item was listed and available at that store. |
| Activity appears at different early/late hours across stores. | Identify affected dates, then request the operating calendar and changes in hours. |

Prioritize investigations by the size of the affected activity, recurrence of the pattern, and how much an unresolved definition could change the report. Avoid filling the brief with every small anomaly.

## 9. Build and Evolve the Method

### Initial Build Sequence

1. Preserve the raw source and record its identity and coverage.
2. Create the assumption register and confirm keys, row meaning, and aggregation rules.
3. Define a compact metric dictionary before drawing charts.
4. Build the whole-business view, then location comparisons, then product detail.
5. Add consistent calendar and trailing comparisons to existing indicators.
6. Reconcile totals across layers and independently spot-check ratios.
7. Convert important unresolved patterns into a discovery brief.
8. Revisit the report after discovery; update definitions before interpreting new results.

### Metric Dictionary Template

For each metric record: **name; question answered; formula in plain language; entity; numerator; denominator; inclusions/exclusions; time window; comparison baseline; source fields; assumptions; zero/missing behavior; aggregation rule; interpretation boundary; version**.

Not every metric belongs on the first screen. Distinguish headline indicators, explanatory breakdowns, and diagnostic details. Promote a diagnostic only when it repeatedly matters to a report user.

### Rules for Changing the Framework

| Trigger | Appropriate change |
|---|---|
| A definition is clarified. | Update the dictionary and affected calculations; restate historical comparisons where feasible. |
| A recurring discovery question cannot be answered. | Add the smallest useful breakdown or request the missing data. |
| A metric duplicates another without adding insight. | Remove it from the main view or retain it only as a diagnostic. |
| A comparison is misleading because populations differ. | Revise eligibility, normalization, or labeling before adding more indicators. |
| A new dataset has different entities or a different row meaning. | Rebuild the hierarchy and metric definitions; preserve the reasoning checkpoints. |
| A proposed explanation survives discovery. | Label the evidence supporting it; do not treat confirmation of context as automatic proof of causation. |
| Operational or experimental data becomes available. | Extend into decision evaluation as a separate, explicitly supported stage. |

Change the method when evidence exposes a weakness, not merely because another metric is available. Keep a version log containing the change, reason, affected indicators, and effect on historical comparability.

### Transfer to Other Datasets

The reusable structure is **overall population -> meaningful comparison groups -> underlying entities or events**. The coffee ledger uses business, stores, and products. Another dataset might use service organization, branches, and service types. Time remains a comparison dimension across all layers.

Never assume that a transaction, customer, unit, or price has the same meaning in the next dataset. The reusable capability is choosing and defending a measurement, recognizing what it hides, and revising it when new evidence arrives.

## 10. Completion Checklist

- [ ] Row meaning and ID assumptions are explicit.
- [ ] Source scope, coverage, and unavailable business context are recorded.
- [ ] Each indicator has a defined population, denominator, period, and comparison.
- [ ] Sales, receipt, and quantity totals reconcile across layers.
- [ ] Ratios are calculated from matching totals rather than unweighted averages of averages.
- [ ] Absolute values accompany important percentages.
- [ ] Growth percentages and percentage-point changes are labeled distinctly.
- [ ] Calendar effects, partial periods, and unequal exposure are addressed or flagged.
- [ ] Zero sales, missing data, and unknown availability remain distinguishable.
- [ ] Trailing windows are explicit and rolling trends are not mistaken for independent evidence.
- [ ] Product comparisons use matching identities and sensible units.
- [ ] Observations, mathematical accounting, hypotheses, and conclusions remain separate.
- [ ] The report produces a focused set of business-discovery questions.
- [ ] Assumptions and method changes are versioned.

## Version Log

| Version | Date | Change | Reason |
|---|---|---|---|
| 1.0 | 2026-09-16 | Initial method and coffee-ledger application. | Consolidates the three-layer structure, receipt assumption, indicator definitions, trailing comparisons, and pre-discovery reasoning. |
| 1.1 | 2026-09-16 | Numbered as workflow document 02 and linked to the profiling procedure. | Makes the sequence from raw-data examination to report design explicit. |
