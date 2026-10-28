# Resolve the data workspace

Plugin resources and user data live in different locations. Resolve both before reading business data.

## Plugin resources

The registered entry skill and agent use the plugin root supplied by Claude Code to locate bundled instructions. This root can point to an installation cache and can change after an update. Resolve relative links in supporting files against the directory of the file containing the link, not the working directory. Files opened directly with Read are not templates; do not use an unexpanded placeholder as a path.

Do not assume plugin path variables are exported into ordinary Bash tool calls. Use the resolved absolute paths from the loaded instructions, quote paths in shell commands, and do not write results or install dependencies into the plugin directory.

## External data root

A data root is the user-owned directory that contains `sharepoint/`:

```text
<data-root>/
  sharepoint/
    <market>/
      data/          Excel sources and relevant source documentation
      semantics/     Optional; excluded from this version
  outputs/           Optional, market-labeled analysis artifacts
```

Resolve it in this order:

1. Use an absolute data-root path explicitly provided by the user or the calling agent. If given the `sharepoint/` directory itself, normalize its parent as the data root. Confirm the expected directory exists without reading source contents.
2. For a follow-up, reuse a previously resolved root only when the project context is unchanged and the user has not selected a different root. Verify it still exists.
3. Otherwise inspect only these two conventional locations: `<project-directory>/sharepoint/` and `<project-directory>/data-agent/sharepoint/`. The corresponding data roots are the project directory and its `data-agent/` child. Use the session's project/working directory; in a delegated task, use the project directory supplied by the caller. Do not substitute the plugin directory.
4. If exactly one candidate exists, use it. If both or neither exist, ask which data root to use. A delegated agent without a clarification tool returns the unresolved choice to its caller. Do not recursively search the filesystem or choose whichever root contains familiar data.

Use an actual project path from the session or caller. If that context is missing, ask for it or return the missing context to the caller.

Record the resolved absolute data root in task context. Resolve market scope before opening any workbook or source documentation:

- Use the market explicitly requested by the user or established by an unambiguous follow-up. Market names are actual directory identifiers, not a fixed list.
- Otherwise list only direct market-directory names and whether their `data/` directories exist. If exactly one market has a `data/` directory, select and state it. If several are available, ask which market to analyze; if none are available, report the missing data location. A delegated agent returns unresolved choices to its caller.
- Read multiple markets only when the user explicitly requests them, maintaining a separate source scope for each. Never inspect other markets' files to decide which market the user meant.
- Do not expand file access merely because an explicitly selected root or market is unavailable; preserve host permission checks and report the gap.

Apply [shared analysis rules](analysis-rules.md) to canonical path containment, aliases, external workbook references, and market separation. A root selection never authorizes crossing into an unselected market or reading `semantics`.

## Outputs and dependencies

Keep source files read-only. When useful, write results under `<data-root>/outputs/<request-id>/<market>/` or an explicitly selected writable output location. Explicit comparison artifacts use `outputs/<request-id>/comparison/` and retain all market labels and source references.

Choose an available compatible Excel reader and calculation tool at runtime. This plugin provides instructions, not an Excel engine or dataset-specific parser. If a dependency is missing, use an existing compatible capability or report the missing requirement. Do not silently install software, alter shared runtimes, or connect external data services.
