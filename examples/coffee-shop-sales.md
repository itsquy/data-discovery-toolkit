# Coffee Shop Sales Case Study

Version 1.0 | 16 September 2026

An application of the [Raw Data Profiling Procedure](../methods/raw-data-profiling.md) and [Report Design Method](../methods/report-design.md). This file preserves the original ledger's evidence, assumptions, and three-layer report choices without making them universal toolkit requirements.

## Source and Evidence Status

Source: `Coffee Shop Sales.xlsx`, `Transactions!A1:K149117`. The workbook was inspected programmatically without modification and is not included in the repository. Findings below describe that extract, not independent reconciliation against a POS system.

| Verified observation | Reporting implication |
|---|---|
| 149,116 rows and unique transaction IDs; 80 product IDs; three locations; January-June 2023. | Report the six-month scope and distinguish receipt assumptions from key uniqueness. |
| No missing fields, duplicate rows, or nonpositive quantities/prices found. | Structural checks passed; semantic and external completeness checks remain open. |
| Product IDs map consistently to their recorded descriptions, types, and categories. | IDs support item comparisons, subject to unrecorded variant/pack differences. |
| Each store has recorded sales on all 181 dates. | Calendar-day rates are computable; full operating exposure is unconfirmed. |
| Sales total 698,812.33; quantity totals 214,470. | Establish control totals for sales and quantity. |
| February sales fell 6.8% versus January, while sales per calendar day rose 3.2%. | Show period totals and normalized daily pace together. |
| Each store contributes approximately one-third of sales. | Use a common chain report with location comparisons. |
| Astoria records 79 SKUs; the other stores record 80. | Flag unequal observed assortment in item comparisons. |
| Fifteen product IDs have multiple recorded prices. | Inspect within-SKU price distributions before explaining average-price changes. |
| Recorded selling-hour patterns differ across locations and dates. | Obtain actual opening-hour context before hourly productivity comparisons. |

These observations do not establish promotions, preferences, stockouts, or management effectiveness. P&L, retention, and inventory-efficiency metrics are outside this application's supported scope.

## Application Assumptions

| Assumption or convention | Status | Consequence |
|---|---|---|
| Transaction ID is a receipt/sales invoice ID. | Explicit user assumption. | Count distinct IDs as receipts; use receipt-based ATV and UPT. |
| Each represented receipt has one recorded SKU and potentially multiple units. | Follows from that assumption and observed unique IDs. | No multi-SKU baskets are represented; no cross-category attachment analysis is supported. |
| Quantity x recorded unit price is recorded sales. | Calculation convention. | Do not call it net or tax-exclusive sales without confirmation. |
| Product ID is the SKU comparison key. | Working convention supported by stable mappings. | Investigate omitted pack/variant distinctions where relevant. |
| Prepared Consumption and Packaged Retail organize the assortment. | Proposed analytical mapping. | Confirm business roles in discovery, especially Flavours. |

Receipts are purchases, not unique customers. Do not reconstruct receipts from similar timestamps. If later evidence revises receipt meaning, revise all affected indicators.

## Measurement Definitions

| Indicator | Calculation | What it describes |
|---|---|---|
| Recorded sales | Sum of quantity x unit price. | Recorded sales value. |
| Receipts | Distinct transaction IDs under the assumption. | Purchases represented by the extract. |
| Units | Sum of transaction quantity. | Recorded item quantity; not standardized across unlike products. |
| ATV | Sales divided by receipts. | Average transaction value. |
| UPT | Units divided by receipts. | Units per transaction. |
| ASP | Sales divided by units. | Quantity-weighted average selling price per unit. |

For the full extract, rounded ATV is 4.69, UPT 1.44, and ASP 3.26. The exact identities use unrounded figures: sales = receipts x ATV, and ATV = UPT x ASP.

ASP depends on individual recorded prices and quantity proportions. For a simple illustration, 50 items priced at 4 and 50 at 6 average 5 per unit. Selling 75 of the cheaper item and 25 of the more expensive item yields 4.50 per unit without changing either price. This is a mathematical possibility, not an explanation of customer motives.

## Product Mapping

| Group | Organizing principle | Categories |
|---|---|---|
| Prepared Consumption | Items served for immediate consumption, including recorded modifiers. | Coffee, Tea, Bakery, Drinking Chocolate, Flavours. |
| Packaged Retail | Packaged take-home products and merchandise. | Coffee beans, Loose Tea, Packaged Chocolate, Branded. |

Under this mapping, Prepared Consumption is 90.1% of sales and Packaged Retail 9.9%. Keep Flavours separately identifiable as modifiers. The mapping is not customer segmentation or proof of the actual consumption occasion.

## Whole Business Performance

**Question:** what is the scale, composition, and direction of recorded activity?

| Indicator | Why include it? |
|---|---|
| Recorded sales and growth | Establish overall size and movement. |
| Receipt count and growth | Track represented purchase activity. |
| Sales and receipts per calendar day | Separate period length from daily pace. |
| ATV, UPT, and ASP, with trends | Inspect the arithmetic components of sales movement. |
| Product-group sales shares and percentage-point changes | Detect changes in composition. |
| Store/group contributions to sales change | Identify which components account for the chain movement. |

Use a compact scorecard, six-month trends, group-mix comparisons, and absolute growth-contribution bars. Interpret chain UPT and ASP alongside product composition rather than as standardized demand, workload, or pricing success.

## Location Performance

**Question:** where do the chain's patterns hold or differ?

| Indicator | Why include it? |
|---|---|
| Store sales and share of chain sales | Establish scale and changing contribution. |
| Daily sales and receipt growth | Compare movement while accounting for month length. |
| ATV, UPT, and ASP by store | Locate differences in the components of sales. |
| Growth gap versus other stores combined | Compare the store's trajectory with its peers. |
| Local group/category mix and share changes | Reveal composition differences hidden by similar totals. |
| Weekday/daypart sales, receipts, relevant units, and shares | Describe when recorded activity occurs. |
| Median and middle 50% of daily sales within comparable periods/weekdays | Show whether averages represent consistent days or large variation. |

Use stores side by side and aligned trend charts. Calculate peer ratios from pooled totals and define whether the focal store is excluded. Use weekday averages per covered occurrence. Observed selling hours do not establish scheduled hours or equal opportunity.

## Product Mix and SKU Performance Across Locations

**Question:** which product components account for movement, and how do matching items differ between stores?

Keep two views: **Mix Overview** and **SKU Comparison**. Use group -> category -> product type -> SKU as the product hierarchy; location is a comparison dimension across those levels, not a mandatory parent of every item.

| View / indicator | Calculation or content | Reason |
|---|---|---|
| Mix: sales share | Component sales divided by the named parent sales total. | Shows value composition. |
| Mix: unit share | SKU units divided by comparable type/category units. | Shows quantity composition and shifts affecting ASP. |
| Mix: concentration | Top-N share within a named family; disclose N. | Shows whether a few items dominate. |
| SKU: sales and units | Values, changes, and trends for each. | Distinguishes value and quantity movement. |
| SKU: receipts and UPT | Item receipts and units divided by those receipts. | Separates purchase frequency from quantity per purchase. |
| SKU: recorded price distribution | Prices, units at each price, and associated dates/locations. | Locates actual recorded price variation. |
| SKU: ASP | Item sales divided by item units. | Summarizes prices recorded for matching items. |
| SKU: units per calendar day | Quantity divided by all days in the covered period. | Describes observed sales pace, including covered zero-sale days. |
| SKU: contribution to parent change | Item sales change and share of the parent's sales change. | Identifies the items behind category/type movement. |
| SKU: sales coverage | Days with sales, first/last sale, locations with sales. | Flags gaps needing listing or availability context. |

A syrup serving, coffee, bag of beans, and mug are not equivalent units of demand. Keep quantity comparisons within sensible families. Missing records do not establish stockouts or zero demand.

Avoid a crowded SKU table: start with mix, filter relevant families, place price/coverage diagnostics in drilldowns, and pair percentages with quantities or amounts. Do not sum subtotals and their underlying rows together.

## Time and Trailing Choices

These are proposed choices for this daily sales application:

- Calendar months for historical review, with calendar-day rates alongside totals.
- Complete weeks for operational comparisons.
- Latest 28 days versus the previous 28 days for recent movement across all three report views.
- A 7-day rolling average where high volume supports a faster signal; sparse retail SKUs may require longer windows.

Use **indicator | latest 28 days | previous 28 days | absolute change | growth % or share change in pp | rolling trend**. Calculate ratios from window totals. A daily sales rate and a window sales total remain different measures.

For a cutoff of 30 June 2023, latest 28 days means 3-30 June and previous 28 days means 6 May-2 June. Confirm boundary completeness before treating the windows as full exposure. Equal weekday counts do not control for holidays, hours, or availability.

The six-month extract does not support year-over-year growth or annual seasonality conclusions. Do not sum overlapping rolling totals or describe their smoothness as independent evidence of stable demand.

## Discovery Questions Prepared by the Profile

| Observation or uncertainty | Evidence to inspect/request |
|---|---|
| The same SKU has several recorded prices. | Map by date and store; ask what determines recorded price and whether variants or adjustments are omitted. |
| Category ASP moves while unit shares change. | Quantify matching-item prices and shares; ask about the population and selling context. |
| An item has no recorded sales at one store. | Check the full period; ask whether it was listed and available. |
| Activity begins/ends at different hours across dates/stores. | Request opening calendars and operational changes. |
| Each transaction ID appears only once. | Confirm that the extract contains complete receipts rather than a filtered or transformed view. |

Keep the questions neutral. A recorded price change does not prove a promotion, and higher quantity alongside a lower price does not prove price-driven demand.

## Transferable Lessons and Boundaries

| Lesson to retain in the method | Choice that stays in this case |
|---|---|
| Confirm grain before naming metrics. | Transaction ID is assumed to identify a receipt. |
| Separate aggregate averages into values and weights. | SKU prices and quantities explain the sales ASP calculation. |
| Examine period exposure. | Calendar-day rates and 28-day comparisons suit this initial application. |
| Let comparisons follow actual relationships. | Business, location, and product/SKU views form the selected architecture. |
| Separate observed coverage from business availability. | Missing item sales need listing/stock context. |
| Define group membership and validate its meaning. | Prepared Consumption and Packaged Retail are the proposed groups. |

## Revision Notes

- Version 1.0: consolidated the original coffee-ledger findings, assumptions, indicator structure, and time-window choices into a named case study. The source evidence is unchanged; reusable rules now live in the method guides.
