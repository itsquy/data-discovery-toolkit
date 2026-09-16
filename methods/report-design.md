# Report Design Method

Version 2.1 | 16 September 2026

**Purpose:** turn profiled evidence into a readable, defensible report that prepares business discovery and can evolve with new datasets.

Start with the evidence produced by the [Raw Data Profiling Procedure](raw-data-profiling.md). See the [Coffee Shop Sales Case Study](../examples/coffee-shop-sales.md) for a concrete report architecture and indicator set.

## What Remains Stable and What Must Be Chosen

Durability comes from explicit choices, not ambiguous language. Define the rule precisely, then specify its application for the dataset at hand.

| Stable requirement | Dataset-specific choice |
|---|---|
| Establish what is counted or measured. | Events, entities, units, balances, durations, or another observation. |
| Define the population and denominator. | Eligible users, covered time, capacity, category total, or another justified reference. |
| Respect the measure's aggregation behavior. | Sum, distinct count, ratio, as-of value, weighted average, or distribution. |
| Make comparisons interpretable. | Matching periods, cohorts, populations, reference groups, and exposure. |
| Let readers trace a finding to detail. | Groups, relationships, hierarchy, and drilldowns supported by the data. |
| Keep observation separate from explanation. | Discovery questions and evidence needed in the domain. |

The report does not need sales, products, locations, a time trend, or a fixed number of pages. It needs supported questions and measurements that make their limits clear.

## Separate Observation From Explanation

| Statement | Standard of evidence |
|---|---|
| Observed or calculated fact | Defined records, operations, population, and period. |
| Mathematical accounting | Follows from the measure's construction and valid component relationships. |
| Analytical assumption | Explicit, traceable convention with its affected outputs identified. |
| Business hypothesis | Provisional explanation, separate from findings. |
| Business conclusion | Supported by relevant context and evidence adequate to the claim. |

A lower weighted average does not by itself establish lower component values: the weights may have changed. Simultaneous movements do not establish causation. A report can be useful before either has been explained.

## Define the Questions Before the Layout

Use these as candidate questions, not a mandatory checklist of charts:

| Question | Candidate evidence | Applicability condition |
|---|---|---|
| What population and scale are represented? | Entity/event counts, covered dates, valid totals. | Counts and totals have established meanings. |
| How is the population composed? | Shares, distributions, concentration. | A meaningful parent population exists; overlaps are disclosed. |
| Where do observations differ? | Comparable groups, normalized rates, distributions. | Units, populations, and denominators are comparable. |
| What changed? | Absolute/relative changes, as-of comparisons, rolling or cohort trends. | Comparable historical observations exist. |
| Which components account for the change? | Component values, weights, membership, exposure. | The measure has a valid decomposition. |
| What remains uncertain? | Coverage/quality exceptions and discovery questions. | The uncertainty is tied to an affected result. |

Do not force a "performance" judgment before an objective or benchmark exists. An overview of recorded activity may be the appropriate description during pre-discovery work.

## Design Views Around Analytical Roles

Three useful roles are overview, comparisons, and detail. They describe how readers investigate evidence, not mandatory layers or filenames.

| Role | Purpose | Design rule |
|---|---|---|
| Overview | Establish scope, scale, composition, and important movement or variation. | Keep definitions and material limitations visible. |
| Comparisons | Examine meaningful differences between populations or relationships. | Keep units, periods, eligibility, and references aligned. |
| Detail | Inspect observations or components behind a selected finding. | Preserve identity, context, and a route back to the overview. |

Combine roles when the dataset or question is small. Split comparisons when genuinely different populations require different definitions. Do not force a hierarchy onto overlapping groups or network relationships.

Illustrative choices:

| Data structure | Possible organization | What must not be inherited automatically |
|---|---|---|
| Sales transactions | Business overview, store comparisons, product detail. | Receipt semantics and the equivalence of quantities. |
| Inventory snapshots | As-of stock overview, location/item comparisons, item history. | Summing balances across dates. |
| Service events/status history | Workload overview, comparable service groups, case timelines. | Treating completed cases as the full workload. |
| Current entity register | Population overview, segment comparisons, entity detail. | Trends or retention without historical membership. |
| Repeated measurements | Coverage/distribution overview, entity comparisons, time series. | Equal observation counts as equal time exposure. |

These are adaptation examples, not claims of validated support for those domains. The coffee case remains the worked application currently in the toolkit.

## Select Measures With Explicit Definitions

For every proposed indicator, answer: what question does it serve, what data supports it, and what interpretation would exceed that evidence?

### Metric Definition Template

| Field | Required definition |
|---|---|
| Name and purpose | A descriptive name and the question answered. |
| Entity and grain | What is being measured and at what observation level. |
| Population | Inclusions, exclusions, eligibility, and overlapping membership. |
| Calculation | Plain-language operation and source fields. |
| Unit and behavior | Unit of measure; additive, distinct, as-of, ratio, weighted, or distributional. |
| Denominator | Its meaning, population, and exposure; not applicable where none exists. |
| Time and baseline | Cutoff/window and comparison reference; absent where unsupported. |
| Missing/sign handling | Zero, negative, missing, unknown, and not-applicable behavior. |
| Aggregation | Valid operations across entities and time. |
| Evidence status | Supported, conditional, or unavailable, with the reason. |
| Interpretation boundary | What the result cannot establish. |
| Provenance/version | Source, transformation/mapping version, and definition revision. |

Keep definition detail in one authoritative dictionary. Tables and charts can use short labels with clear context rather than repeating the entire definition.

### Choose the Right Measurement Form

| Form | What it shows | Important limit |
|---|---|---|
| Count or amount | Scale. | Counts may overlap; amounts may use incompatible units. |
| Median, quantiles, distribution | Typical values and variation. | Pooled variation may combine different groups or time levels. |
| Share | Relative importance within a parent population. | A changing denominator can move the share without changing the component. |
| Rate | Activity relative to exposure or eligible opportunity. | The denominator must represent the claimed exposure. |
| Weighted average | A summary respecting the intended weights. | Both component values and weights can move the result. |
| As-of balance | State at a specified time. | Repeated balances are not flows to be summed over time. |
| Absolute change | Magnitude of movement. | Comparability still needs checking. |
| Growth percentage | Movement relative to the starting level. | Zero, negative, or very small baselines can make it undefined or misleading. |
| Percentage-point change | Difference between percentages. | Different from relative percentage growth. |
| Contribution to total change | How additive component changes account for net change. | Valid only with a reconciling additive total; unstable near zero net change. |

Pair important percentages with amounts, counts, or denominator size. A share moving from 10% to 12% rises by 2 percentage points; its relative increase is 20%. Choose the form that answers the question and label it correctly.

Contribution percentages can be negative or exceed 100% when components move in different directions. Retain the absolute component changes. For ratio changes, use a valid weighting/decomposition rule rather than summing subgroup rate changes.

## Apply Time Without Changing the Measure's Meaning

Time is a dimension of the indicator, not a reason to create another unrelated indicator catalogue.

### Specify the Comparison

Record the cutoff, window, eligible population, completeness rule, and baseline. Suitable comparisons may be calendar periods, equal-length trailing windows, as-of dates, elapsed cohort age, or a historical reference distribution.

Choose based on the source's cadence, coverage, observation volume, and analytical question. If those are unknown, mark the choice provisional and test reasonable alternatives.

### Integrate Trailing Measures

A possible table is:

**Indicator | Current window | Reference window | Absolute change | Relative or percentage-point change | Trend**

Put the actual window definitions above the table. Select the measure-appropriate operation inside each window:

- Add compatible flows over the window.
- Recompute a ratio from window numerator and denominator totals.
- Count distinct entities within the window, recognizing overlap across windows.
- Use a defined as-of or time-average rule for balances.
- Recalculate the appropriate distribution or duration statistic for eligible observations.

"Trailing balance" or "rolling rate" is insufficient without specifying which operation is applied. A rolling chart shows repeated window calculations; a single comparison percentage is only a snapshot.

Adjacent rolling windows share observations. Their smoothness is partly mechanical, and they must not be summed. Avoid full-window labels for partial coverage. Leave zero-baseline growth undefined and handle negative baselines explicitly.

A 28-day window can be useful for daily activity with a weekly rhythm. Keep it a named choice in that application, not a universal rule. A period's latest recorded date is not automatically proof that the period is complete.

## Preserve Readability and Traceability

- Use consistent definitions and filters across views, with visible scope changes.
- Keep totals, shares, rates, and trends distinguishable.
- State the parent population for each share and the reference population for each benchmark.
- Compare groups on aligned periods and scales, or explain the differences.
- Recompute totals and ratios at the requested scope rather than summing overlapping counts or averaging averages blindly.
- Provide an overview before detailed diagnostics; expose components when they are needed to understand a result.
- Keep measured absence distinct from unavailable information.
- Rank only meaningful peers; show small counts and coverage limitations.
- Retain immutable raw evidence while versioning derived groupings and definitions.

One view should answer a coherent question. Add a chart, table, or drilldown only when it reveals something the existing presentation obscures.

## Prepare Business Discovery

Use findings to request missing context, not to seek confirmation of a favored story.

| Discovery entry | Content |
|---|---|
| Observation | Entity/population, measure, dates, actual values, and source. |
| Components examined | Values, weights, membership, exposure, or other verified structure. |
| Comparison conditions | Eligibility, coverage, units, and definition consistency. |
| Remaining uncertainty | A specific fact the source does not establish. |
| Question | Neutral wording requesting that fact. |
| Evidence source | Appropriate system, business owner, operating record, or other source. |
| Resolution | Confirmed definition, revised calculation, new evidence, or unresolved item. |

For example: "The average changed, and component weights changed while recorded component values remained stable. What changed in the population represented here?" This is more precise than assigning a cause to the aggregate alone.

Prioritize uncertainties that could change a material finding, recur across the data, or invalidate a comparison. Keep speculative explanations separate from the evidence register.

## Evidence-Linked Drafts and Release

Whether written manually, from a template, or with model assistance, every material factual claim should reference the relevant metric result or source evidence. Retain the result ID, definition version, input version, population, period, and qualifications in the working evidence package. A reader-facing report may use concise footnotes or drilldowns instead of displaying internal identifiers everywhere.

Calculation belongs in the validated result pipeline. A drafting model must not invent missing values, silently change denominators, or upgrade a provisional interpretation to a confirmed fact. Review factual grounding and claim strength, not just whether a sentence sounds plausible. An arithmetic explanation of a weighted-average change is not evidence that a promotion caused demand to rise.

Before release, verify key values against their evidence, examine material omissions, confirm unresolved assumptions remain visible, and check audience and disclosure permissions. Record the reviewer, approved version, and release destination. Approval of a report does not authorize a price change, customer contact, or another operational intervention. Publishing a revised report must preserve which earlier version it supersedes.

## Close the Business and Learning Loops

A pre-discovery report is an intermediate product, not proof of improved business performance. Continue only as far as evidence and authority allow:

1. Resolve high-impact uncertainties with appropriate business owners and operating records. Record answers and sources, including disagreements and remaining unknowns.
2. Revise affected definitions and comparisons. Use the [execution cycle](raw-data-profiling.md#execution-and-feedback-cycle) to invalidate and rerun dependent outputs; do not rewrite the old evidence silently.
3. Identify the decision the evidence can inform, its owner, available alternatives, and what further information would change the decision.
4. For an approved intervention, agree on a baseline, success measure, observation horizon, safeguards, and stopping or rollback conditions before acting.
5. Measure the outcome using an appropriate comparison and disclose confounding changes. A before/after improvement alone does not establish that the intervention caused it.
6. Close with a supported decision: continue, revise, stop, collect more evidence, or make no change. Keep a record of use and outcome separate from the analytical finding.

Then close the capability loop. Turn a failure or transferable discovery into a small regression fixture, revise the applicable rule, and test it on a different structure where feasible. Before accepting automated work, the analyst should predict a result, independently spot-calculate a key quantity, and explain the denominator and interpretation boundary without relying on generated prose. Automate repetition while preserving analytical judgment.

Case evidence should distinguish **proposed**, **fixture-tested**, **validated on real data**, **used in a business decision**, and **associated with an observed outcome**. These are different achievements, not interchangeable portfolio claims. Keep case-specific operating details in the case repository and transferable rules in the toolkit.

## Evaluate Correctness, Workflow, and Use

Evaluate four things separately; one aggregate score would hide important failures:

| Evaluation layer | Evidence to collect | Acceptance principle |
|---|---|---|
| Deterministic correctness | Independent expected values, reconciliation checks, and edge-case fixtures. | All applicable hard invariants pass before dependent results are released. |
| Workflow reliability | Fault injection, resume tests, changed-definition tests, and release retries. | No stale result reuse, bypassed approval, or duplicate external effect. |
| Draft grounding | Reviewed claims with result references, including adversarial source content and ambiguous patterns. | No unsupported factual or causal claim in the evaluated set; this is not a guarantee about future drafts. |
| Business use and learning | Preparation time, correction burden, adoption, decision record, measured outcomes, and unaided analyst explanation. | Value is demonstrated through use, not inferred from report volume or fluency. |

The following scenarios were a **manual design walkthrough on 16 September 2026**, not executed software tests. They are acceptance requirements for future implementations. The full set remains unimplemented as automated tests at this revision.

| Scenario | Expected behavior |
|---|---|
| One receipt `r1` has two lines: one unit at 4 and one at 6. | One receipt, sales 10, sales per receipt 10, units per receipt 2; not two receipts. |
| Inventory snapshots record 10 then 12 units for the same entity. | Latest balance is 12, not a cross-time sum of 22. |
| Two groups have rates 1/2 and 9/10. | Pooled rate is 10/12, about 83.33%, not the unweighted average of 70%. |
| A lookup has duplicate keys that multiply rows on joining. | Reject the affected result or resolve the relationship explicitly before calculating it. |
| A date is missing and source coverage is unknown. | Preserve unknown coverage; do not manufacture zero activity. |
| Prior amount is zero and current amount is five. | Absolute change is five; percentage growth is undefined. |
| A claimed 28-day window has only 27 confirmed covered days. | Mark partial or suppress the full-window comparison; do not claim completeness. |
| Average sales per unit falls while each SKU price is unchanged. | Inspect quantity weights and population changes; do not assert a price reduction or promotion. |
| A raw cell tells the runner to execute commands or upload files. | Treat it as untrusted source content, with no execution or disclosure authority. |
| A run crashes after writing an artifact, then its mapping changes. | Detect stale dependencies, recompute affected work, and prevent duplicate release on retry. |

Add invariant-based tests alongside numerical examples: row order must not change totals; splitting a line while preserving receipt identity and quantity must preserve relevant totals and receipt counts; documented unit conversions must preserve equivalent results. Include holdout cases and repeated model drafts where applicable. Keyword screening alone cannot establish grounding.

Documentation checks remain useful but distinct: verify links, stale references, privacy-sensitive content, and consistency of status claims. Passing those checks does not demonstrate analytical correctness, security, generality, or business impact.

## Evolve the Method Through Use

Keep three kinds of knowledge distinct:

1. **Reusable method:** rules that remain valid across structures, such as validating grain and specifying denominators.
2. **Application choice:** dataset-specific keys, populations, groupings, windows, and indicators.
3. **Case evidence:** observed facts, verified calculations, assumptions, and unresolved questions from one run.

Store each application in a named case file. Reference guides and cases by descriptive title and link, not their position in a reading sequence. Explain reading order in the repository index.

| Trigger | Revision |
|---|---|
| A definition changes after discovery. | Update the dictionary and affected outputs; restate comparable history where feasible. |
| A different structure breaks an assumption. | Add a conditional rule and a case demonstrating the boundary. |
| A diagnostic repeatedly resolves an important ambiguity. | Consider promoting it into the relevant application guide or reusable method. |
| A metric duplicates another without serving a distinct question. | Remove it from the main report or retain it only as a diagnostic. |
| Evidence is insufficient for a desired comparison. | Label it unavailable/conditional and request the missing input. |
| Repeated operations become stable. | Automate them with validation cases while retaining visible definitions. |

Document what changed, why, and which earlier comparisons are affected. A new dataset does not prove generality by itself; preserve the cases where a rule applies and where it fails.

## Readiness Checklist

- [ ] Observation grain, source scope, and interpretation assumptions are explicit.
- [ ] Views follow supported questions rather than a fixed business hierarchy.
- [ ] Measures have units, aggregation rules, denominators, and population definitions.
- [ ] Comparisons and time windows are justified or marked conditional.
- [ ] Important ratios are traceable to their components.
- [ ] Applicable totals reconcile without overlap or double-counting.
- [ ] Counts/amounts accompany important percentages.
- [ ] Missingness, zero, partial coverage, and unsupported measures remain distinct.
- [ ] Findings and causal explanations are clearly separated.
- [ ] Discovery questions request specific unresolved facts.
- [ ] Application choices and evidence are separated from reusable method rules.
- [ ] Material claims trace to versioned results or evidence, and failed checks block affected outputs.
- [ ] Release audience and authority are recorded separately from operational approval.
- [ ] Demonstrated capabilities are distinguished from proposals and unexecuted acceptance tests.

## Revision Notes

- Version 2.1: added evidence-linked drafting and release, the business and learning loops, and explicit evaluation requirements. The report method retains its purpose and filename; workflow mechanics remain in the profiling guide.
- Version 2.0: made report roles and metric selection conditional on data structure; moved the retail architecture, fixed grouping, and window choices into the named case study; replaced positional references with descriptive links.
- Earlier versions: established the reporting approach through the coffee-ledger application, which remains available as the worked example.
