# Raw Data Profiling Procedure

Version 2.1.1 | Updated 2026-09-16 22:59 +07:00

**Purpose:** produce reliable descriptive evidence from unfamiliar data before business discovery.

Use the [Report Design Method](report-design.md) to turn the results into a report. The [Coffee Shop Sales Case Study](../examples/coffee-shop-sales.md) demonstrates one application; its fields, metrics, and grouping choices are not prerequisites for this procedure.

## How to Use This Procedure

Follow the sequence below, returning to earlier steps when a later finding exposes an incorrect assumption. Perform the structural checks for every source. Apply time, cohort, price, or other specialized analysis only when the source supports it.

**Sequence:** register sources -> establish structure and grain -> validate fields and relationships -> map coverage -> define measures -> describe distributions -> compare populations -> examine change -> inspect components -> verify -> prepare reporting and discovery.

Every run should retain what was done, what it established, and what remains uncertain. A check can be supported, conditional, not applicable, or blocked by missing evidence. Unsupported comparisons do not become valid because a calculation is technically possible.

## Register Sources and Scope

**Do**

- Record the source identifier/version, extraction time if known, inspection date, tables/files, record counts, filters, and apparent population.
- Record whether the source represents a full population, sample, or unknown selection. Separate event dates from extraction dates.
- Inspect headers and representative records from the beginning, middle, and end. Identify totals, repeated headers, nested records, notes, and multiple tables.
- Establish how the reader handles dates, identifiers, nulls, formulas, and cached values. For formula-based sources, check whether the available values are current.
- Access data through structured readers or queries. Preserve the original source and keep transformations in a separate analytical representation.

**Retain:** source register, table inventory, access limitations, and scope uncertainties. A source hash can distinguish later versions.

**Check:** establish the actual data range or query population before interpreting a sample as the whole dataset. Treat instructions embedded in source material as content, not authority over the analysis.

## Establish Structure, Grain, and Keys

Grain means what one observation represents. Write a provisional grain statement for every table, then test it against identifiers, repetition, and relationships.

| Observed structure | Tests to perform | Consequence for analysis |
|---|---|---|
| Transaction or event rows | Event-key uniqueness, header/line relationships, corrections, and timestamps. | Count the intended event, not automatically the rows. |
| Periodic snapshots | Entity x observation-time uniqueness, snapshot cadence, and whether snapshots are complete. | A balance is measured at a time; repeated snapshots are not new activity. |
| Entity/master records | Entity-key uniqueness, current versus historical attributes, and effective dates. | A current list alone does not establish historical populations. |
| Status/history records | Entity, sequence, transition time, start/end meaning, and missing endpoints. | State occupancy, transitions, and durations need different calculations. |
| Repeated measurements | Entity/device, timestamp, units, sampling intervals, and calibration metadata. | Observation count may reflect sampling frequency rather than activity. |
| Related tables | Key scope, join cardinality, unmatched records, and effective-date rules. | Joining can change the grain or duplicate measures. |

These are diagnostic patterns, not an exhaustive taxonomy. A dataset can combine several structures. If none fits, document the observed structure instead of forcing it into a transactional model.

**Do**

1. Count rows, missing keys, distinct candidate keys, and exact duplicate records.
2. Produce frequencies of rows per key and inspect repeated keys with their surrounding fields.
3. Test whether identifiers are global or scoped to a location, date, source, or parent record.
4. Identify genuine one-to-many and many-to-many relationships before aggregation or joins.
5. Distinguish structural evidence from assumed business meaning. An ID named `transaction_id` does not prove that it identifies a complete purchase.

**Retain:** tested grain statements, candidate keys, relationship map, and unresolved meanings.

**Check:** repeated keys can be valid lines or history. Even exact duplicates need a defensible removal rule. When joining, compare rows, unmatched keys, distinct entities, and relevant measure totals before and after. Stop the affected calculation if join multiplication is unexplained.

## Profile Fields and Mappings

For every field, record its stored type, proposed analytical role, unit, missingness, distinct count, and representative values. Profile all relevant records; use samples to investigate findings, not to certify full-source quality.

| Field role | Profile | Investigate |
|---|---|---|
| Identifier | Uniqueness, repeated-key frequency, missing keys. | Collisions, key reuse, leading zeros, unexpected multiplicity. |
| Numeric measure | Minimum, quartiles, median, maximum, zeros, negatives, and sum where meaningful. | Mixed units, signs, special values, extreme observations. |
| Date/time | Parse failures, range, frequency, gaps, interval lengths. | Timezone, business date, date ambiguity, partial periods. |
| Category/text | Frequencies, rare values, whitespace/case variants. | Split labels, conflated categories, changing definitions. |
| Relationship field | Distinct mapped values per key, unmatched keys, mapping changes over time. | Missing entities, valid reclassification, inconsistent mapping. |
| Nested or compound value | Child counts, optional elements, and proposed expansion rule. | Loss or duplication when flattening. |

A statistical outlier is not automatically an error. A negative amount may represent a reversal; a zero may be valid. Document parsing failures, exclusions, and normalization rules instead of silently discarding them.

**Retain:** field dictionary, quality findings, mapping exceptions, transformation log, and before/after counts.

## Map Coverage and Exposure

Coverage describes which observations are represented. Exposure describes the time or opportunity behind a rate. Presence of records does not prove full coverage or equal exposure.

**Do**

- Determine the intended population and period from available evidence; mark unconfirmed boundaries.
- Compare observed entities, dates, and combinations against known expected records or schedules where available.
- For time-based data, build the appropriate calendar or schedule to reveal gaps. For snapshots, compare against the expected snapshot cadence rather than assuming daily data.
- For non-temporal data, inspect membership coverage and missing relationships without inventing a date series.
- Distinguish calendar time, observed time, confirmed operating time, eligible membership, and availability.
- Verify period endpoints and late-arriving data before labeling a period complete.

**Retain:** coverage matrices, missing combinations, excluded populations, and denominator qualifications.

**Check:** do not build every possible entity combination and label the absent ones zero unless those combinations are valid and covered. Distinguish observed zero, unknown, not applicable, and unobserved. Do not infer a service's operating hours from its first and last event.

## Define Supported Measures

Before calculating totals or ratios, assign an aggregation rule to each measure.

| Measure behavior | Operation to establish | Important restriction |
|---|---|---|
| Additive amount or count | Sum disjoint observations in compatible units. | Avoid overlap, duplicated headers, and incompatible currencies/units. |
| Balance or stock | Select an as-of value or a defined time average. | Do not sum repeated balances across time. |
| Distinct entity count | Count validated unique keys at the requested scope. | Group counts may overlap and may not add to the total. |
| Ratio or weighted average | Recompute from matching numerators and denominators. | A simple average of subgroup ratios is usually a different measure. |
| Duration | Define start/end events, unit of analysis, and cutoff treatment. | Completed-only records exclude open cases and can bias the result. |
| Continuous measurement | Choose observation weighting or time weighting and describe coverage. | Dense sampling must not silently receive extra importance. |
| Percentage/share | Define eligibility and the parent population. | Overlapping categories do not form an additive composition. |

Derived measures need both mathematical and semantic support. Quantity x unit price is a sales convention for compatible transaction data, not a universal formula for numeric columns.

**Do**

1. Define units, inclusions, sign conventions, and any gross/net distinction.
2. Calculate supported base measures and retain numerator/denominator components.
3. Reconcile to source control totals when available. Internal consistency is not external completeness.
4. Verify arithmetic identities using unrounded values and matching populations.

**Retain:** measure definitions, base summaries, control checks, and limitations. For example, counts may be supported while a utilization rate remains unavailable because capacity is missing.

## Describe Distributions and Concentration

**Do**

- Examine amounts and frequencies, then median, quartiles, tails, and unusual values where meaningful.
- Compare mean and median to assess how representative the average is.
- Show both counts/amounts and percentages; disclose the weighting behind a distribution.
- Examine ranked contributions and concentration only for measures with a meaningful additive total.
- Examine coverage and data-quality patterns as well as business measures.
- Inspect a few ordinary records as well as extremes.

Distinguish the unit being counted. One event with five items counts once in an event distribution and five times in an item distribution. For sampled data, describe the sample and any justified weights rather than treating sample proportions as proven population proportions.

**Retain:** distribution profiles, concentration tables where applicable, and material exceptions with source references.

**Check:** a broad distribution can reflect changing levels over time or different populations. Compare like periods or groups before describing the variation as instability.

## Compare Populations and Entities

Select dimensions from the available relationships and the question being investigated. Locations, products, cohorts, service types, or devices are possible choices; none is mandatory.

**Do**

1. Choose a small set of comparisons that can reveal scale, composition, exposure, or distribution differences.
2. Align periods, units, eligibility, and definitions.
3. Show absolute measures beside normalized measures where a defensible denominator exists.
4. Compare both full populations and matched entities where composition changes matter; label them separately.
5. Pool numerators and denominators for an aggregate peer ratio. Disclose whether the focal entity is included in the peer reference.
6. Flag small counts and unequal coverage before ranking apparent differences.

**Retain:** comparison tables, reference populations, denominator rules, and coverage flags.

**Check:** matching only entities present in both periods can hide entry, exit, or survival effects. Keep those populations visible. Do not infer a causal explanation from a difference or correlation.

## Examine Change When Time Is Supported

Skip temporal analysis when there is no valid time dimension or comparable history. A file's extraction timestamp does not create a history of its entities.

### Calendar, As-Of, and Cohort Comparisons

Choose the comparison according to the measure:

- For flows/events, aggregate into covered periods and compare totals with meaningful exposure-based rates.
- For stocks, compare defined as-of observations; document treatment of missing or stale snapshots.
- For cohorts, define entry and elapsed observation time. Do not compare a mature cohort with a recently entered cohort as if both had equal follow-up.
- For durations, retain open/censored cases separately and state how the reporting cutoff affects interpretation.

Calculate current/prior values and absolute changes. Use percentage growth when its baseline and sign make the interpretation meaningful; a negative or near-zero baseline may require another presentation. Use percentage points for changes in shares or rates expressed as percentages.

### Trailing and Rolling Comparisons

Treat window length as an explicit design choice based on cadence, volume, exposure, and the phenomenon being examined.

1. Select a complete cutoff, window length, and measure-specific aggregation rule.
2. Compute the latest window and an explicitly defined comparison window, often the preceding non-overlapping window.
3. Recompute ratios and shares from matching window components; do not average daily ratios blindly.
4. Repeat the window calculation at successive cutoffs to create a rolling series.
5. Inspect how a reasonable alternative window changes the result, without selecting a window solely for a stronger-looking pattern.

For example, a 28-day window can balance weekday counts in daily activity data. It is not the default for snapshots, monthly reporting, sparse events, or every dataset with dates.

**Retain:** period definitions, cutoff/completeness evidence, comparison tables, rolling series, and sensitivity notes.

**Check:** do not sum overlapping windows or interpret their smoothness as independent confirmation. Two adjacent full windows need two windows of covered history. Leave zero-baseline growth undefined. Limit seasonal claims to what the available cycles support.

## Inspect the Components of Aggregate Movement

Use the measure's definition to choose the next operation, rather than immediately choosing a business story.

| Observation | Mathematical or structural check |
|---|---|
| A weighted average changed. | Inspect component values and their weights. |
| A total changed. | Inspect disjoint component changes and changes in coverage or population. |
| A proportion changed. | Inspect both numerator and denominator, and membership eligibility. |
| A time-normalized rate changed. | Inspect activity and exposure separately. |
| An as-of balance changed. | If compatible movement data exists, reconcile opening balance, additions, removals, and adjustments. |
| Average duration changed. | Inspect the distribution, case mix, and included/completed/open cases. |
| Aggregate and subgroup trends disagree. | Inspect composition, weighting, and definition changes before favoring either summary. |

Separate entrants, exits, and unmatched entities from matched-item comparisons. Do not assign a missing component a zero value merely to complete an equation.

Arithmetic decomposition is not causal attribution. If several decomposition conventions are possible, document the chosen reference values and interaction treatment. Component growth percentages do not generally add to overall growth.

**Retain:** component tables, comparison qualifications, and the exact unknown facts needed for interpretation.

## Verify and Hand Off

**Do**

- Trace important totals and exceptions to source records.
- Reconcile only measures that should reconcile under their aggregation rules.
- Confirm composition sums to 100% only for a mutually exclusive, exhaustive partition.
- Check ratios, cutoffs, signs, units, partial periods, and zero/missing handling.
- Use exact checks for identifiers/counts and documented tolerances for numeric calculations.
- Record unresolved failures and which outputs they affect; do not block unrelated supported analysis.

**Retain a reproducible evidence package:** source/grain record, field and quality profile, coverage/exposure profile, supported measure definitions, relevant distributions/comparisons, assumptions, verification results, and a focused discovery brief. These can be sections in one workspace rather than separate files.

Choose report content using the [Report Design Method](report-design.md). A profile is ready for that handoff when another analyst can reproduce its important measurements and distinguish supported results from unresolved interpretation.

## Run Record Template

| Field | What to record |
|---|---|
| Run and source version | Run date, source identity, extraction scope, table/range/query. |
| Structure and grain | Observation type, validated keys, relationships, and temporal meaning. |
| Assumptions | Supplied assumptions, analytical conventions, and unresolved meanings. |
| Transformations | Parsing, normalization, joins, exclusions, and before/after controls. |
| Measures | Units, aggregation rules, numerator/denominator definitions. |
| Coverage and comparisons | Populations, exposure, periods, cutoffs, and reference groups. |
| Findings | Precise observations, operations performed, and source references. |
| Verification | Passed checks, failures, and affected outputs. |
| Discovery | Missing facts, neutral questions, and proposed evidence sources. |
| Reproduction | Tool/query/script, parameters, mappings, and method version. |

On a refresh, inspect schema, key, mapping, and coverage changes before reusing calculations. On a new dataset, repeat grain and semantic checks even if column names are familiar.

## Execution and Feedback Cycle

The procedure above describes analytical operations. The stages below describe how to execute, review, and revisit them. They are a proposed workflow, not an implemented automation system. Begin with stages 00-06; later stages remain human-governed and may conclude without an intervention.

| Stage | Work and retained output | Gate or authority |
|---|---|---|
| 00 Receive | Register access scope, immutable source, hash, extraction boundaries, and run identity. | Source access and intended use are permitted; business meaning may still be unknown. |
| 01 Inspect | Produce structural, field, relationship, and coverage profiles using deterministic readers. | Record failures and affected scope; do not silently repair meaning. |
| 02 Propose definitions | Draft grain, keys, units, mappings, measures, and unresolved questions. | Human judgment; optional AI may suggest, not approve. |
| 03 Accept a contract | Version the selected definitions, dependencies, assumptions, and acceptance record. | Authorized analyst accepts confirmed or explicitly provisional semantics. |
| 04 Calculate and test | Produce metric results, components, reconciliation checks, and lineage. | Failed checks block dependent results; unaffected results may continue. |
| 05 Draft | Produce report views and discovery questions from validated results. | Follow the [Report Design Method](report-design.md); keep conditional results visibly conditional. |
| 06 Review and release | Check factual claims, limitations, audience, and permitted distribution. | An authorized reviewer approves release, not business action. |
| 07 Discover | Record business-owner answers, evidence sources, and remaining unknowns. | Distinguish supplied explanations from independently verified facts. |
| 08 Revise | Amend definitions, invalidate dependent outputs, and rerun affected stages. | Reaccept changed semantics; retain superseded versions. |
| 09 Define a decision | Record the decision owner, baseline, success measure, guardrails, and observation horizon. | Business owner chooses whether to intervene. |
| 10 Pilot | Execute a bounded approved change with monitoring and rollback conditions. | Separate operational permission; a report is not authorization. |
| 11 Assess | Compare outcomes with the agreed baseline and appropriate controls; record confounders. | Continue, revise, stop, or conclude that evidence is insufficient. |
| 12 Learn | Retain a case lesson, regression fixture, method revision, and analyst explanation. | State what was demonstrated and where transfer is still untested. |

Loop back only to stages whose inputs or assumptions changed. A completed discovery with no justified intervention is a valid outcome. The detailed rules for stages 05-12 belong in the [reporting, discovery, and evaluation guidance](report-design.md#evidence-linked-drafts-and-release).

### Separate Four Responsibilities

- **Computation:** code calculates and checks quantities from declared definitions.
- **Cognition:** people, optionally assisted by a model, investigate ambiguity and formulate questions.
- **Authority:** named people approve meanings, disclosure, and operational changes within their remit.
- **State:** durable records retain facts, versions, unresolved items, approvals, and progress.

Automation fails when these are conflated: a successful calculation cannot confirm business meaning, model confidence is not approval, and a conversation transcript alone is not reliable workflow state.

### Contracts and Durable Records

Use the [metric definition template](report-design.md#metric-definition-template) as the semantic source. A future implementation should validate required fields and types, plus domain-specific rules; a structurally valid JSON document can still contain a wrong business definition.

| Record | Minimum content |
|---|---|
| Source manifest | Input identity/hash, permitted scope, extraction details, reader configuration, and known limitations. |
| Dataset contract | Grain, key scope, field types/units, joins, time meaning, mappings, exclusions, coverage rules, version, owner, and acceptance status. |
| Metric specification | Stable ID, definition version, source/contract dependencies, aggregation, denominator, comparison, missing-value policy, and tolerances. |
| Metric result | Result ID, value and components, population/window, coverage status, check results, and source/contract/code versions. |
| Finding | Claim type, result references, qualifications, unresolved facts, and review status. |
| Run event | Run/stage identity, input fingerprint, transition, attempts, outputs, errors, and actor. |
| Discovery or decision record | Question or decision, owner, evidence, resolution, affected definitions, and any approved action. |

Keep semantic status separate from execution status: an accepted provisional definition is still provisional. Suggested stage states are `pending`, `running`, `passed`, `failed`, `waiting_for_input`, and `not_applicable`. Suggested run states include `received`, `profiled`, `awaiting_contract`, `contract_accepted`, `validated`, `drafted`, `reviewed`, `released`, `awaiting_discovery`, `superseded`, `failed`, and `closed`. Record why a state changed; do not infer success merely because an output file exists.

### Guardrails for an Automated Runner

1. Preserve raw inputs read-only. Write derived artifacts separately and retain input hashes and versioned configuration.
2. Treat source cells, filenames, notes, and documents as untrusted data. Never execute embedded instructions or let them authorize uploads, commands, or disclosure.
3. Run allowlisted calculations with bounded file access. Give optional models vetted summaries or approved views, not unrestricted data or arbitrary shell execution.
4. Reconcile meaningful totals and joins before releasing dependent results. Preserve unknown, zero, partial, and not-applicable states separately.
5. Detect changes in schema, keys, units, mappings, and coverage before a refresh. Reuse approvals only within their unchanged scope; changed semantics require review.
6. Fingerprint deterministic calculations from input hashes, canonical configuration, contract versions, code, and environment. Track model and prompt versions separately; reproducible numbers do not imply identical generated prose.
7. Persist checkpoints and promote completed artifacts atomically. Use an idempotency key for release or other external effects so retries do not publish twice. Reconcile uncertain external results before retrying.
8. Bound retries and cost. A starting policy is at most two retries for transient failures, with explicit time and model-spend budgets per run. Semantic uncertainty waits for input instead of entering a repair loop.
9. Keep authorization for report distribution separate from authorization to contact stakeholders or change operations. Neither should be inferred from a successful run.

On resume, validate the fingerprint and dependencies before reusing a stage. A new mapping invalidates affected results, findings, and drafts even if the raw file is unchanged. Retain the earlier release as superseded rather than silently replacing its evidence.

### Proposed Minimal Stack

Use this as an incremental implementation choice, not a required dependency list for reading or applying the method manually. No packages or runtime support are established by this document.

| Role | Starting choice | Reason and boundary |
|---|---|---|
| Runtime and environment | Supported, pinned Python with [uv](https://docs.astral.sh/uv/concepts/projects/sync/). | Keep a reproducible dependency lock and explicit runtime version. |
| Spreadsheet adapter | [openpyxl read-only mode](https://openpyxl.readthedocs.io/en/stable/optimized.html). | Inspect XLSX without modifying it; formula cache validity still needs checking. |
| Calculation | [DuckDB SQL](https://duckdb.org/docs/current/clients/python/overview); Parquet when useful. | Auditable transformations; SQL alone does not enforce valid grain or semantics. |
| Typed contracts | [Pydantic strict validation](https://docs.pydantic.dev/latest/concepts/strict_mode/) plus business checks. | Reject malformed definitions while keeping semantic approval human-owned. |
| Workflow state | Python's [SQLite interface](https://docs.python.org/3/library/sqlite3.html) and versioned run artifacts. | A local transaction log is sufficient before shared execution is needed. |
| Interface and report | [Typer](https://typer.tiangolo.com/) CLI and [Jinja](https://jinja.palletsprojects.com/en/stable/) Markdown/HTML templates. | Keep calculations separate from presentation. |
| Tests and CI | [pytest fixtures and parametrization](https://docs.pytest.org/en/stable/how-to/parametrize.html); [GitHub Actions with minimal token permissions](https://docs.github.com/en/actions/tutorials/authenticate-with-github_token). | Use synthetic fixtures; do not upload private inputs to CI. |
| Optional language model | One replaceable adapter for evidence-linked drafts. | The calculation and validation path must run without it. |

Implement one transaction adapter and a small set of tests first. Add snapshots, histories, or other structures only with explicit contracts and boundary tests. Defer orchestration platforms, vector databases, multiple agents, and a web interface until repeated use establishes a need. Evaluate the outputs using the [acceptance scenarios](report-design.md#evaluate-correctness-workflow-and-use).

## Revision Notes

- Version 2.1.1 | Updated 2026-09-16 22:59 +07:00 | Changed: timestamped version notes with explicit change summaries and reasons. Why: make revision history understandable without opening a full Git diff; analytical rules are unchanged.
- Version 2.1 | Updated 2026-09-16 (time not recorded) | Changed: added the proposed execution cycle, contracts, resumable state, guardrails, and implementation stack. Why: translate the manual procedure into an automation specification without implying that the engine is implemented.
- Version 2.0 | Updated 2026-09-16 (time not recorded) | Changed: separated the retail case, added structure-specific operations, and replaced positional references with descriptive links. Why: support different data structures without inheriting coffee-ledger assumptions or unstable document numbering.
- Earlier versions: developed the procedure through the coffee-ledger application. Its specific assumptions and indicators are retained in the case study.
