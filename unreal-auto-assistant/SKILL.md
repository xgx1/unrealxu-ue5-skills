---
name: unreal-auto-assistant
description: "UE5.6/5.7 assistant entry: Unreal question without named skill → auto-route to most precise UE5 skill + dedicated MCP tools."
---

# Quick Start
- Treat this as the default entry for UE5.6/UE5.7 requests.
- Parse user intent first without requiring module names.
- Route to `blueprint-module-router` when module-level precision is needed.

# Workflow
- Detect request type: Blueprint, C++, UI, save/load, networking, world interaction, debugging, performance, packaging.
- If module names appear, delegate routing to `blueprint-module-router`.
- If module names do not appear, route by intent:
  - Blueprint -> `blueprint-blueprint-workflow`
  - C++ gameplay -> `unreal-cpp-foundations`
  - UI/UMG/Slate -> `unreal-umg-lifecycle`
  - save/load/replication -> `unreal-serialization-savegames`
  - pickup/spawner/world interaction -> `unreal-physics-collision`
  - perf/packaging -> `unreal-testing-debugging`
  - debugging/validation -> `unreal-testing-debugging`
  - architecture/refactor -> `unreal-module-build`
- Return one primary skill and optional secondary skill for cross-domain requests.
- Return routing payload fields:
  - `primary_skill`
  - `secondary_skill`
  - `recommended_mcp_tools[]`
  - `route_confidence`
  - `route_reason`

# Natural Language To Skill And Tools
- Blueprint requests:
  - target skill: `blueprint-blueprint-workflow`
  - recommended tools: `blueprint_feature_build`, `blueprint_modify`, `blueprint_query`
- C++ gameplay/system requests:
  - target skill: `unreal-cpp-foundations`
  - recommended tools: `blueprint_query`, `asset_search`, `get_output_log`
- UI/UMG/Slate requests:
  - target skill: `unreal-umg-lifecycle`
  - recommended tools: `blueprint_query`, `blueprint_modify`, `capture_viewport`
- Save/load/replication requests:
  - target skill: `unreal-serialization-savegames`
  - recommended tools: `character_data`, `blueprint_query`, `get_output_log`
- World interaction/pickup/spawner requests:
  - target skill: `unreal-physics-collision`
  - recommended tools: `spawn_actor`, `get_level_actors`, `set_property`, `move_actor`
- Performance/packaging requests:
  - target skill: `unreal-testing-debugging`
  - recommended tools: `run_console_command`, `get_output_log`, `capture_viewport`, `open_level`
- Debug/validation requests:
  - target skill: `unreal-testing-debugging`
  - recommended tools: `get_output_log`, `asset_search`, `blueprint_query`, `task_list`
- Architecture/module-boundary requests:
  - target skill: `unreal-module-build`
  - recommended tools: `asset_search`, `asset_dependencies`, `asset_referencers`

# Constraints
- Do not require users to know skill names.
- Prefer deterministic routing with explicit reason.
- Keep fallback behavior explicit when confidence is low.
- Prefer dedicated MCP tools before `execute_script`.

# Failure Handling
- If intent is ambiguous, return top 2 route candidates and ask one short clarification.
- If request spans many systems, split into staged route steps.
- Clarification template:
  - `Quick check: do you want A(<candidate_1>) or B(<candidate_2>)?`
  - ask once, then continue.

# Escalation
- Escalate when query depends on plugin/engine source outside indexed scope.
- Escalate when org-level coding standards are required but not available in repo.