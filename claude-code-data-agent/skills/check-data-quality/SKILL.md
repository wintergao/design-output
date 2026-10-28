---
name: check-data-quality
description: Assess whether selected Excel data supports the current question by checking relevant completeness, duplication, structure, units, and comparability risks. Use for specific input concerns or a requested data quality review.
---

Resolve relative links against this skill file's directory, not the working directory. Before reading source data, resolve the external data workspace using [workspace resolution](../../shared/workspace.md). Never use the plugin installation directory as the data root. Use the current user request and its explicitly established market context.


# Check Data Quality

Read the [shared analysis rules](../../shared/analysis-rules.md). Keep checks proportional to the intended calculation.
Use [discover-excel-data](../discover-excel-data/SKILL.md) to locate inputs and [analyze-excel-data](../analyze-excel-data/SKILL.md) to read and calculate.

## Scope and Market Identity

- Record a market for every dataset, quality finding, and calculation input. Read only the selected market's `data/` directory.
- Keep source workbooks read-only and ignore `semantics/`; inspect available workbook content without assuming a fixed schema or filename pattern.
- Never borrow another market's definitions, units, mappings, or previous results. Missing local evidence remains missing.
- Access multiple markets only when explicitly requested. Check each market independently before testing compatibility between them.
- Reuse completed checks only when their market, source version, range, and intended use still match; do not expand an ordinary question into a full audit.

## Inspect the Relevant Inputs

- Identify sheets, headers, labels, merged cells, notes, hidden regions, formulas, and totals from the actual workbook.
- Establish the observation grain and requested data range before interpreting repeated rows, blank labels, or apparent outliers.
- Check required fields for missing values, type errors, numeric text, percentage storage, and ambiguous markers. Missing is not zero.
- Distinguish formula text, cached results, and actually recalculated values; do not claim recalculation merely because a cache was read.
- Verify units, currencies, and scale factors from evidence in the selected market. Leave unknown units unresolved.
- Separate file naming and modification time from business dates, reporting windows, cumulative values, and snapshots.
- Determine required period and population coverage from the question; do not assume a specific time granularity or fill gaps with nearby files.
- Preserve source classification and limitations when available; documentation claims do not establish external authenticity.

## Check the Intended Operation

- For aggregation, distinguish duplicate records from valid repeated observations and exclude overlapping detail, subtotal, and total levels.
- Reconcile additive measures only across compatible units and non-overlapping populations; retain unexplained differences.
- Verify numerators, denominators, and weights for rates and averages. Do not assume stocks or distinct counts can be added across time.
- For joins, inspect unmatched keys, row multiplication, lost coverage, and changes in totals before and after the operation.
- For comparisons, verify definitions, category mappings, coverage, units, and time windows separately for each selected dataset.
- In an explicit cross-market comparison, document supported mappings and unresolved differences; identical labels alone do not prove equivalence.
- Treat extreme or negative values as signals to investigate unless a supported rule makes them invalid; do not silently remove them.

## Return Findings

State whether the requested calculation is supported, supported with limitations, or blocked by a specific input gap.
Attach market, file path, sheet, cells or ranges, executed checks, and measured differences to material findings.
Distinguish passed, failed, and unperformed checks. A local problem need not block unrelated supported results.
Keep transformations reproducible in the analysis; never repair source files as part of this workflow.
Return findings to the calling workflow without recursive audits; use [validate-answer](../validate-answer/SKILL.md) for output claims.
