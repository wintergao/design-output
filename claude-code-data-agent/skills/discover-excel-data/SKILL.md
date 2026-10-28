---
name: discover-excel-data
description: Locate and inspect Excel sources inside a selected market's local data directory. Use before analysis when file coverage, workbook structure, or source meaning is unknown or has changed; exclude semantics.
---

Resolve relative links against this skill file's directory, not the working directory. Before reading source data, resolve the external data workspace using [workspace resolution](../../shared/workspace.md). Never use the plugin installation directory as the data root. Use the current user request and its explicitly established market context.


# Discover and load Excel sources

Follow [shared analysis rules](../../shared/analysis-rules.md). Discover what the current question needs without assuming a fixed format or creating a semantic dictionary.

## Establish scope before opening files

1. Resolve the external data workspace using [workspace resolution](../../shared/workspace.md), then locate its `sharepoint/` directory. Initially list only direct market directory names and whether their `data/` directories exist.
2. Use the market(s) identified by the request or unambiguous active context. If multiple markets are possible and none is selected, ask before reading source contents.
3. Set the allowed source root to `sharepoint/<selected-market>/data/`. For an explicitly requested comparison, establish a separate scope for each selected market.
4. Verify physical path boundaries, including directory aliases and external links, as specified in the shared rules. Another market is never a fallback for missing files.

## Find relevant sources

List candidate workbooks and relevant source documentation inside the selected `data/` subtree. Exclude temporary lock files and transient outputs. Do not enter `semantics` or search all markets with a broad file glob.

Use filenames and directories as hints, not a mandatory naming scheme or proof of coverage. Read relevant notes and workbook metadata to distinguish datasets, revisions, overlapping extracts, and missing periods. Several files may legitimately share a period; determine their role before treating them as duplicates or merging them.

Do not pick a file merely because it has the newest modification time. Resolve materially conflicting versions from source evidence or a targeted clarification. “Latest” means the latest applicable reporting period verified for the selected market.

## Inspect before choosing a loader

Use an available read-only workbook capability appropriate to the actual format. An Excel-aware library may inspect worksheets and merged regions; legacy or binary formats require a compatible reader. Do not assume that renaming a file changes its format or that one library handles every workbook type.

Inspect relevant sheets and bounded ranges first:

- Table boundaries, header rows, merged cells, blank separators, notes, and multiple tables per sheet.
- Record grain, candidate keys, dimension labels, measures, units, and period fields.
- Details, subtotals, totals, and whether the source is a flat table, crosstab, snapshot, or another layout.
- Formulas/cached values, errors, hidden or filtered content, and external references when material.

A preview establishes structure, not a complete analytical result. After identifying the structure, load the full range or all chunks needed for the calculation. Never let a preview limit silently truncate an aggregate.

## Load with explicit extraction choices

Choose worksheets, headers, ranges, types, and transformations from observed evidence. Do not hardcode a sheet name, column position, row count, date pattern, or business dimension from an earlier file. Reuse an extraction rule only after checking that it applies to the new source in the same market.

For multi-row headers, derive hierarchy from actual merged regions and labels. Propagate blanks only when the layout supports it. Unpivot a crosstab only after identifying comparable measures and overlapping totals.

Keep original labels and source locators alongside normalized names. Do not silently merge labels that normalize to the same value. Attach market to each extracted dataset or record; keep different markets in separate containers until an explicit comparison step.

Return a concise inventory to [analyze-excel-data](../analyze-excel-data/SKILL.md): market, files, sheets/ranges, observed grain, available fields, actual period coverage, extraction rules, and material limitations. If a reader or definition is missing, report the affected gap without fabricating data or expanding scope.
