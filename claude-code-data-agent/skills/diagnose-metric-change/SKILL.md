---
name: diagnose-metric-change
description: Reproduce a comparable Excel metric change and quantify supported contributions across observed dimensions. Use for movement or driver questions, not a simple value lookup or unsupported causal attribution.
---

Resolve relative links against this skill file's directory, not the working directory. Before reading source data, resolve the external data workspace using [workspace resolution](../../shared/workspace.md). Never use the plugin installation directory as the data root. Use the current user request and its explicitly established market context.


# Diagnose Metric Change

Read the [shared analysis rules](../../shared/analysis-rules.md). Explain measured changes at the strength supported by the evidence.
Use [discover-excel-data](../discover-excel-data/SKILL.md) to locate inputs and [analyze-excel-data](../analyze-excel-data/SKILL.md) for actual calculations.

## Establish the Comparison

- Identify the selected market, measure, comparison windows, population, filters, grain, and units without assuming a workbook layout or time granularity.
- Read only that market's `data/` directory, keep originals read-only, and ignore `semantics/`.
- Attach market identity to every input and result. Never reuse another market's definitions, units, mappings, or prior-turn results.
- Use multiple markets only for an explicit cross-market request; reproduce each market's values independently before comparing them.
- Check actual business dates and coverage against filenames and sheet labels. The previous available file is not necessarily the requested baseline.
- Resolve material ambiguity in the metric or baseline before making precise claims. Do not invent definitions or substitute missing observations.

## Reproduce the Movement

- Read both values and execute the calculation, retaining market, file, sheet, cell or range, filters, and calculation evidence.
- Confirm compatible definitions, units, populations, and windows; invoke focused [check-data-quality](../check-data-quality/SKILL.md) checks when these affect trust.
- Report absolute change as current minus baseline. For a positive baseline, relative change is that difference divided by the baseline.
- A zero baseline has no defined relative change; for negative baselines, explain a supported convention or report absolute change only.
- Distinguish percentage-point differences from relative percentage changes in rates.
- Preserve unknown or unavailable values. Never use another market's values to fill a missing baseline or missing category.
- For explicit cross-market work, check coverage and mappings per market; disclose incompatible measures or units instead of forcing a comparison.

## Quantify Supported Contributions

- Choose dimensions supported by the observed data and relevant to the question; do not assume particular business categories.
- For additive measures, align non-overlapping categories and reconcile their changes to the total; retain unexplained residuals.
- Separate entry, exit, missingness, and classification changes. Absence is zero only when evidence establishes complete coverage and that interpretation.
- Avoid double counting detail and totals or combining overlapping decomposition views as independent contributions.
- For rates and averages, examine numerators, denominators, weights, and mix; do not average subgroup percentages without a valid basis.
- Use multiplicative decompositions only when their metric relationship is established, and state how interaction effects are treated.
- Contribution shares require a reconciled decomposition and nonzero total change. Explain negative shares or shares above 100% caused by offsets.
- Preserve the observed time grain; sparse or aggregated observations do not establish event timing, statistical significance, or causality.

## Explain and Verify

Separate calculated facts, supported contributions, hypotheses, and unresolved causes. Business explanations require evidence beyond coincident movement.
Lead with the measured change and its main supported contributors, then include material limitations and traceable sources for each market.
If evidence only locates a change, state that its business cause remains unverified; do not invent events, targets, or external context.
Use [validate-answer](../validate-answer/SKILL.md) to check consequential numbers, reconciliation, market separation, and claim strength, reusing completed checks.
