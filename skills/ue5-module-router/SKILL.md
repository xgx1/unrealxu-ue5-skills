---
name: unreal-module-router
description: Route UE5.6/5.7 questions to most precise skill via module names, aliases, intent keywords, layer context (RenderCore, AIModule, AssetRegistry).
---

# Quick Start
- Extract explicit module names from user prompt first.
- If module is found, route by exact module mapping before keyword heuristics.
- If module is not found, use aliases and layer context.

# Workflow
- Parse prompt for module candidates (for example `RenderCore`, `AIModule`, `AssetRegistry`).
- Lookup module in `ue5-module-routing-table-final.csv`.
- If multiple hits, prioritize:
  1. exact module name match
  2. Build.cs path similarity
  3. alias overlap with prompt keywords
- Return routing payload:
  - `primary_skill`
  - `secondary_skill`
  - `recommended_mcp_tools[]`
  - `route_confidence`
  - `route_reason`
- Use `secondary_skill` when request spans multiple concerns in one module context.

# Constraints
- Prefer deterministic routing; avoid broad guesses if exact module match exists.
- Keep one primary target skill unless user explicitly asks cross-module analysis.
- If module maps to `unreal-module-build`, answer module-boundary/design first.
- Prefer dedicated MCP tools before `execute_script`.

# Failure Handling
- If no module is recognized, fallback to closest capability skill and state reason.
- If confidence is low, provide top 2 candidates and request module confirmation.
- Use `execute_script` only when dedicated MCP tools are insufficient and explain why.
- If mapping is outdated, regenerate from `unreal-module-build/scripts/generate_module_index_v2.py`.

# Escalation
- Escalate for large cross-cutting refactors across many modules.
- Escalate when requested module belongs to plugin source outside indexed scope.