# Data Agent for Claude Code

A portable Claude Code plugin for evidence-based questions about local Excel data. It includes one data analyst agent, five focused analysis skills, and a conversational entry skill. All instructions are English; answers follow the user's language.

## Package contents

```text
claude-code-data-agent/
  .claude-plugin/
    plugin.json
    marketplace.json
  agents/data-analyst.md
  skills/
    ask-data/SKILL.md
    discover-excel-data/SKILL.md
    analyze-excel-data/SKILL.md
    check-data-quality/SKILL.md
    diagnose-metric-change/SKILL.md
    validate-answer/SKILL.md
  shared/
    workspace.md
    agent-instructions.md
    analysis-rules.md
  README.md
```

No Excel data, source documentation, previous analysis results, credentials, hooks, servers, or schema-specific parser are bundled.

## Load locally for one session

Extract the ZIP first. Start Claude Code from the workspace containing your data, supplying the absolute path to the extracted plugin directory:

```bash
cd /path/to/data-workspace
claude --plugin-dir /path/to/claude-code-data-agent
```

Then ask:

```text
/data-agent:ask-data In ID, which companies rank in the top five in the latest available quarter?
```

Follow-ups such as `AIA Life CAGR over the past two years` can use the market and measure established in the same conversation. The entry skill intentionally runs in that conversation. If a dedicated agent is useful, ask Claude to use `data-agent:data-analyst` and supply the data root and market context.

## Install persistently through the included local marketplace

The same folder includes a small standard marketplace catalog, with plugin source `./`. From a shell:

```bash
claude plugin marketplace add /path/to/claude-code-data-agent
claude plugin install data-agent@data-agent-local
```

Keep the source folder available if you want to reinstall or update from that local marketplace. Start a new Claude Code session after installation. This package does not run those installation commands automatically.

## Data workspace

Keep data in your own workspace, separately from the plugin installation:

```text
<data-root>/
  sharepoint/
    ID/
      data/
    SG/
      data/
  outputs/
```

ID and SG are examples, not a fixed market list. File names, reporting periods, schemas, metrics, and Excel layouts are discovered from the selected sources at runtime. An optional `semantics/` directory is ignored in this version.

The plugin accepts an explicit absolute data-root path in the request. Otherwise it checks the current project directory and its `data-agent/` child for `sharepoint/`. If both or neither exist, it asks which root to use. It never looks for business data relative to the plugin cache.

For this repository layout, starting Claude Code from either the repository root or the existing `data-agent/` workspace resolves the original data without copying it into the plugin.

## Tools and source boundaries

Claude Code needs permission to read the selected data and access to a compatible Excel reader and calculation environment. An available Python environment with an appropriate workbook library is one option; no particular library or schema is mandated. The plugin does not install dependencies.

A single-market question reads only that market's `data/` subtree. Cross-market work must be explicitly requested, keeps each market labeled, and checks definitions, periods, units, and coverage before comparison or aggregation. Missing market data is never filled from another market.

Source workbooks stay read-only. Optional calculations and evidence go to `<data-root>/outputs/<request-id>/<market>/`. Skills provide workflow-level separation; actual access denial requires filesystem or tool permissions in the host.

## Validation

Run the official validator against the plugin folder and the marketplace manifest:

```bash
claude plugin validate /path/to/claude-code-data-agent --strict
claude plugin validate /path/to/claude-code-data-agent/.claude-plugin/plugin.json --strict
claude plugin validate /path/to/claude-code-data-agent/.claude-plugin/marketplace.json --strict
```

Manifest validation checks packaging. It does not prove that every Excel format is readable or that a model will follow every instruction. Verify actual analyses against their source data.

## Official references

- [Create a Claude Code plugin](https://code.claude.com/docs/en/plugins/create)
- [Plugin manifest and path variables](https://code.claude.com/docs/en/plugins-reference)
- [Agents and skills in plugins](https://code.claude.com/docs/en/plugins/components)
- [Distribute a plugin and include a marketplace](https://code.claude.com/docs/en/plugins/publish)
