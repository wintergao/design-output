---
name: ask-data
description: Answer a business or analytical question from local market-scoped Excel files using the Data Agent workflow. Use for factual queries, rankings, calculations, period comparisons, or follow-up questions about those sources.
argument-hint: "[market and question, optionally with an absolute data-root path]"
---

# Ask Data Agent

Read and follow [Data Agent operating instructions](${CLAUDE_PLUGIN_ROOT}/shared/agent-instructions.md). Resolve relative links in supporting files against the directory of the file containing the link, not the working directory.

User request:

$ARGUMENTS

Use the argument text as the task. If it is empty, use the current user request; ask what to analyze only if there is no actionable question.

Resolve the external data root and market before reading source contents, load the relevant skills, run the calculation, and return a verified answer with sources.

Run in the current conversation so follow-up questions retain their established market and metric context. Do not automatically launch a separate subagent. If the user requests delegation, provide the resolved data root, market scope, measure, period, and question to `data-agent:data-analyst`; that agent must not guess missing conversation context.
