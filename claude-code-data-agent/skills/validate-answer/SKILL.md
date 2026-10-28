---
name: validate-answer
description: Verify consequential Excel answer values, calculations, market scope, citations, and claim support before delivery or within a requested review. Keep ordinary checks focused rather than auditing every source.
---

Resolve relative links against this skill file's directory, not the working directory. Before reading source data, resolve the external data workspace using [workspace resolution](../../shared/workspace.md). Never use the plugin installation directory as the data root. Use the current user request and its explicitly established market context.


# Validate the Answer

Read the [shared analysis rules](../../shared/analysis-rules.md). Validate the current answer against evidence actually obtained.
Use [discover-excel-data](../discover-excel-data/SKILL.md) for missing source locations and [analyze-excel-data](../analyze-excel-data/SKILL.md) for necessary rereads or recalculation.

## Select the Review Scope

- Identify the user's question, selected market, answer draft, material claims, calculation records, and existing checks.
- Focus on values and assumptions that could change the conclusion; expand to a comprehensive review only when requested.
- Reuse evidence only when its market, source version, range, and calculation scope match the current question.
- Use focused [check-data-quality](../check-data-quality/SKILL.md) checks for demonstrated input risks; avoid recursive validation.
- Keep source workbooks read-only and ignore `semantics/` throughout review.

## Verify Market and Source Boundaries

- Every dataset and computed result carries explicit market identity, and each material source citation points into that market's selected `data/` directory.
- No definition, unit, mapping, input value, or prior answer has been borrowed from ID for SG, SG for ID, or another market; establish each interpretation from its own sources.
- Multiple markets appear only when the user explicitly requested cross-market work; each market's values remain individually traceable.
- Cross-market comparisons verify definitions, category mappings, units, coverage, and windows separately for each market; unsupported equivalence is disclosed.
- Missing evidence is not replaced with another market's data, a neighboring period, or zero.
- Source references identify the actual file, sheet, and cells or ranges read; filenames and documentation claims do not replace numerical evidence.
- The answer respects observed business dates and grain, distinguishes source values from derived values, and preserves material source limitations.

## Verify Calculations

- Check relevant row ranges and aggregation levels; exclude duplicate records and overlapping detail, subtotal, and total values.
- Check rate and average numerators, denominators, weights, and eligible populations; verify that any summation is valid for the measure.
- Verify comparison windows, absolute and relative differences, percentage points, zero denominators, and treatment of negative baselines.
- Confirm that rankings and superlatives use the stated population and comparable measures, including ties and incomplete coverage.
- Reconcile contribution totals and retain residuals or incompatible classifications instead of manufacturing a balancing category.
- Use original-cell readback, an independent calculation, or a supported subtotal to check key results when useful; rerunning identical code is not independent validation.
- Check rounding, signs, scale factors, units, and currencies against evidence; unknown units remain unknown.

## Verify Claims and Deliver

- Distinguish observed patterns, contribution relationships, assumptions, and causal claims. Limit each claim to its supported population and time grain.
- Evaluations such as unusual, on target, or leading need an identified benchmark; plausible wording alone is not evidence.
- Correct the answer and analysis calculations when needed, then recheck affected claims without altering source workbooks.
- When evidence is insufficient, narrow or qualify the conclusion and preserve independently supported results.
- Report only checks actually performed. A scoped pass does not certify all workbook contents or external source authenticity.
- Ordinary answers include necessary sources and limitations; requested reviews additionally state findings, corrections, and unresolved gaps by market.
