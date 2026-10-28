---
name: analyze-excel-data
description: Execute traceable queries, aggregations, rankings, ratios, and period comparisons from inspected local Excel sources. Preserve market scope and derive loading and calculation choices from each workbook's actual structure.
---

Resolve relative links against this skill file's directory, not the working directory. Before reading source data, resolve the external data workspace using [workspace resolution](../../shared/workspace.md). Never use the plugin installation directory as the data root. Use the current user request and its explicitly established market context.


# Analyze Excel data

Follow [shared analysis rules](../../shared/analysis-rules.md). Use [data discovery](../discover-excel-data/SKILL.md) when sources or extraction rules are not established.

## Translate the question into a calculation

Identify the selected market, measure, population, filters, period, grouping, and comparison. Reuse explicit user definitions and verified source meaning. Clarify ambiguity that materially changes the answer; do not invent units or metric definitions from labels alone.

Confirm record grain and the appropriate additive level before calculating. Identify which details, aggregates, or independent datasets support the question. Source rows, entities, transactions, and numeric cells are different counting units.

## Execute using available tools

Use an available read-only Excel reader and calculation environment. This skill requires no fixed script, library, layout, filename convention, or standard schema. If no suitable capability exists, state which calculation is blocked.

Load the full relevant population using the inspected sheet, range, header, and type rules. For large inputs, process relevant chunks while preserving complete coverage and grouping/deduplication state. Keep sources unchanged and make transformations reproducible.

Maintain market and source provenance through selection, reshaping, joins, and aggregation. Check join keys, unmatched rows, and row multiplication where relevant. Retain market in identity and grouping whenever multiple markets could be present.

Apply the requested operation:

- **Totals:** aggregate mutually exclusive, compatible observations without adding overlapping subtotals again.
- **Rankings:** use consistent population, period, and unit; explain ties or cutoffs. Filtering to selected entities changes the ranking population.
- **Shares/rates:** calculate compatible numerators and denominators and state the denominator scope. An unavailable or zero denominator does not yield a valid percentage.
- **Changes:** read both periods and calculate current minus baseline; compute relative change only when the baseline supports its interpretation. Verify the intended prior period instead of automatically selecting the previous available file.
- **Distinct counts/averages:** use a verified entity key or appropriate weights; row counts or averages of subgroup averages may not answer the question.

Keep missing or invalid values explicit unless a supported handling rule is established. Failed reads and unmatched filters are not measured zero. Save useful intermediate evidence outside `sharepoint/`, identified by request and market.

## Explicit multi-market analysis

Load and calculate each selected market independently first. Do not transfer extraction rules, field meanings, classifications, currencies, or filters from one market to another without checking its sources.

Establish verified mappings for requested metrics/dimensions and a compatible time basis. Label results by market and retain separate units, coverage, and sources. If “latest” differs by market, state the dates and resolve whether the question requires each market's latest period or a common period.

Do not match entities across markets solely because names or IDs coincide. Requested alignment needs an explicit mapping and must preserve contributing markets. Comparison does not authorize pooling; combined totals require a requested aggregation and verified unit, definition, and overlap compatibility.

When a market lacks data or a comparable definition, return supported results for the others and identify the missing market-specific result. Do not substitute another market's value, silently extrapolate, or present mismatched periods as like-for-like.

## Validate and answer

Use [check-data-quality](../check-data-quality/SKILL.md) for material input issues and [diagnose-metric-change](../diagnose-metric-change/SKILL.md) when asked what drove a change. Retain inputs, parameters, and successful execution results; do not present unexecuted calculations as findings.

Apply [validate-answer](../validate-answer/SKILL.md). Lead with the answer, identify market and actual period, and provide enough source locations and calculation detail to recover important numbers. Separate observed limitations and unsupported causal explanations from established results.
