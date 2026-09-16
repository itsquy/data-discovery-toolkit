# Data Discovery Toolkit

A practical, evolving method for profiling unfamiliar data and building useful reports before business discovery.

This project is a long-term professional toolkit: a place to develop analytical judgment, document repeatable procedures, and turn lessons from new datasets into better reporting methods.

## Start Here

Begin with [Raw Data Profiling Procedure](methods/raw-data-profiling.md) to establish grain, coverage, supported measures, and descriptive evidence. Continue with [Report Design Method](methods/report-design.md) to choose views, indicators, comparisons, and discovery questions.

See [Coffee Shop Sales Case Study](examples/coffee-shop-sales.md) for the original worked application, including its three-layer report and receipt assumptions.

The method guides define reusable rules. Named cases contain dataset-specific evidence and design choices. Refer to each file by its descriptive title; reading order belongs here rather than in file numbers.

| Resource | Owns |
|---|---|
| Raw Data Profiling Procedure | Operations, applicability conditions, checks, and evidence to retain. |
| Report Design Method | Report questions, measurement definitions, comparison rules, and evolution criteria. |
| Coffee Shop Sales Case Study | Ledger findings, assumptions, retail indicators, grouping, and window choices. |

## Core Approach

**Observe -> define the measurement -> inspect its components -> test comparability -> prepare discovery questions.**

Keep observed facts, mathematical relationships, assumptions, and business explanations distinct. A pattern can justify a question before it justifies a conclusion.

For example, lower sales per unit can reflect changes in individual prices, changes in the proportions of products sold, or both. The toolkit helps identify what must be examined before choosing a business explanation.

## Current State

This repository currently contains a documented procedure and reporting framework. It is not yet an executable profiling application or an automated dashboard.

The guides include field and coverage checks, measure aggregation rules, report-design choices, percentage and trailing-window rules, verification checklists, and templates for assumptions and discovery questions. They explicitly distinguish transactions, snapshots, entity records, events, and repeated measurements when those structures require different treatment.

The original sales workbook is not included. Worked observations provide context for the method and do not imply that its business interpretations have been independently confirmed.

## How This Will Evolve

The framework will evolve as more data types and business contexts are examined. Each new dataset should challenge the existing assumptions rather than simply inherit the coffee-ledger model.

For every new application:

1. Re-establish what a row, identifier, quantity, amount, and time period mean.
2. Apply the profiling procedure and record where it succeeds or breaks down.
3. Adapt the reporting hierarchy and denominators to the actual process.
4. Use business discovery to resolve uncertainties and correct definitions.
5. Document the lesson, revise the method, and preserve the reason for the change.

Potential future applications include inventory snapshots and movements, service or operational events, customer and subscription histories, and multi-table datasets. These are future directions, not validated modules today. The guides describe how to approach such structures; only the coffee ledger currently has a worked case in this repository.

## Development Direction

### Role in the Wider Practice

The goal is business-systems capability, not an ever-larger catalogue of charts. The [career direction in Business System Practice](https://github.com/itsquy/business-system-practice/blob/3dd6d593f2a9ea5e1d1fd8cae3f7f3ef27f90978/knowledge/10-distilled/business-systems-career.md) connects business problems, processes, data, rules, software, AI, human governance, and measurable outcomes.

Keep repository responsibilities distinct:

| Repository | Responsibility | Evidence of progress |
|---|---|---|
| [Business System Practice](https://github.com/itsquy/business-system-practice) | Career direction, active priorities, and distilled learning. | A coherent practice and explicit choices about what to develop next. |
| Data Discovery Toolkit | Reusable analytical methods and, later, tested software. | Reproducible results and demonstrated transfer across data structures. |
| [Fresh-Meat Distributor Case](https://github.com/itsquy/case001-sales-intelligence-system-local-fresh-meat-distributor) and future case repositories | Business context, implementation, use, and outcomes. | A real decision, approved intervention where appropriate, and measured results. |

The three repositories inspected on 16 September 2026 are compatible with one goal, but do not yet prove an end-to-end practice: the distributor repository was empty, and the career handover named campaign funnels as the initial commercial focus. Choosing between those tracks remains open; repository names alone do not establish a change in priority. Select one active case using access to data, an available business owner, and a measurable decision. Link shared methods rather than duplicating them across cases.

### Evaluation Baseline

The 16 September 2026 evaluation of [revision 75d8b00](https://github.com/itsquy/data-discovery-toolkit/tree/75d8b0053ddfd369a71ee4ecbd72eb2511794546) passed five documentation-hygiene checks: all five local files matched their remote Git blobs; all twelve relative Markdown links resolved; the four Markdown files contained no positional document references; no checked dataset extensions were tracked; and no drive-qualified Windows paths were found. These are narrow checks, not a complete privacy or analytical audit.

Ten analytical and workflow scenarios were also reviewed against the written method. That was a design walkthrough, not execution of an engine. There was no executable implementation, dependency configuration, behavioral test suite, or CI to evaluate. The method is a useful specification; automation reliability and business impact remain unproven. The [evaluation requirements](methods/report-design.md#evaluate-correctness-workflow-and-use) define the next acceptance tests.

### Build in Small Increments

- [x] Document a repeatable raw-data profiling procedure.
- [x] Define the initial reporting framework and its reasoning.
- [ ] Apply the method to additional data types and document transferable lessons.
- [ ] Extract reusable run records, metric dictionaries, and discovery templates.
- [ ] Build tested profiling scripts for stable, repeatable checks.
- [ ] Add examples and validation cases covering different data grains and edge cases.
- [ ] Consider a CLI or interactive interface once repeated usage establishes its requirements.

Automation should follow validated procedures. Success means more reliable comparisons, clearer assumptions, and better discovery questions, not simply more generated charts or metrics.

Start with one source adapter and one business question. Establish deterministic results and edge-case tests before adding report generation or optional AI. Add a second, structurally different case before claiming generality. A CLI is a possible first interface; a dashboard or agent platform is not a prerequisite.

Use three linked loops: **evidence** (source to validated report), **business** (discovery to decision and observed outcome), and **capability** (lesson to regression test, revised method, and independent explanation). See the [execution cycle and proposed stack](methods/raw-data-profiling.md#execution-and-feedback-cycle) and the [discovery-to-outcome rules](methods/report-design.md#close-the-business-and-learning-loops). These are implementation proposals, not features already delivered.

## Working With Data

Preserve original inputs and keep raw datasets outside version control by default. Commit methods, code, and appropriately prepared examples. Recheck provenance and sharing rights before adding any future dataset or case study.

Use the run-record template in the [Raw Data Profiling Procedure](methods/raw-data-profiling.md) and the metric definition and revision rules in the [Report Design Method](methods/report-design.md) to make each application reproducible.

When adding a new dataset, create a descriptively named case under `examples/`. Promote a lesson into `methods/` only when its applicability and limits can be stated clearly. Add specialized rules when evidence requires them; do not make the core instructions vague to accommodate unknown future cases.

Update a guide in place while its responsibility remains the same. Rename or split it only when that responsibility changes substantially, updating incoming links and recording the reason. Step numbers can identify workflow order; they must not substitute for durable document names.

When noting a version, include its update timestamp with timezone and a brief change summary and reason: `Version | Updated | Changed | Why`. Describe the meaningful difference from the preceding revision, like a concise Git commit summary, rather than repeating the document's contents. Keep the latest version and timestamp at the top of each versioned guide and put its newest revision note first. Use Git history for exact line changes. For older revisions, retain the known date and leave an unknown time unspecified; do not invent timestamps.

## Ownership and Status

Maintained by [itsquy](https://github.com/itsquy) as an evolving professional practice and potential practical tool.

Initial documentation: September 2026. Scope and implementation will grow through use across additional datasets.
