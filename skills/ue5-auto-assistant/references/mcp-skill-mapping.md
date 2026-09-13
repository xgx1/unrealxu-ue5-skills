# MCP Skill Mapping (UE5.6/UE5.7)

This file defines skill-to-tool mapping for deterministic routing.

## blueprint-auto-assistant
- whitelist:
  - `status`, `get_ue_context`, `task_list`, `task_status`
- graylist:
  - `get_output_log`, `capture_viewport`
- avoid:
  - `execute_script` as first action

## blueprint-module-router
- whitelist:
  - `asset_search`, `asset_dependencies`, `asset_referencers`, `blueprint_query`, `get_output_log`
- graylist:
  - `task_submit`, `task_status`, `task_result`
- avoid:
  - direct world mutation before route confidence is established

## blueprint-blueprint-workflow
- whitelist:
  - `blueprint_feature_build`, `blueprint_modify`, `blueprint_query`, `enhanced_input`
- graylist:
  - `asset_search`, `get_output_log`
- avoid:
  - `execute_script` unless Blueprint tools cannot express the operation

## unreal-cpp-foundations
- whitelist:
  - `blueprint_query`, `asset_search`, `get_output_log`, `get_ue_context`
- graylist:
  - `blueprint_modify`
- avoid:
  - editor-world mutation tools for pure C++ design requests

## unreal-umg-lifecycle
- whitelist:
  - `blueprint_query`, `blueprint_modify`, `asset_search`, `capture_viewport`, `get_output_log`
- graylist:
  - `material`, `asset`
- avoid:
  - `execute_script` for routine UMG/Slate wiring

## unreal-serialization-savegames
- whitelist:
  - `character_data`, `blueprint_query`, `blueprint_modify`, `asset`, `asset_search`, `get_output_log`
- graylist:
  - `task_submit`, `task_status`, `task_result`
- avoid:
  - client-side-only mutation assumptions without authority checks

## unreal-physics-collision
- whitelist:
  - `spawn_actor`, `get_level_actors`, `set_property`, `move_actor`, `delete_actors`, `open_level`, `capture_viewport`
- graylist:
  - `material`, `character`
- avoid:
  - `run_console_command` for level switching when `open_level` exists

## unreal-testing-debugging
- whitelist:
  - `get_output_log`, `asset_search`, `blueprint_query`, `get_level_actors`, `capture_viewport`, `task_list`, `task_status`, `task_result`
- graylist:
  - `run_console_command`
- avoid:
  - broad state-changing operations during diagnosis

## unreal-testing-debugging
- whitelist:
  - `run_console_command`, `get_output_log`, `capture_viewport`, `open_level`, `asset_dependencies`, `asset_referencers`, `asset_search`
- graylist:
  - `task_submit`, `task_status`, `task_result`
- avoid:
  - `execute_script` before standard profiling/validation tools are exhausted

## unreal-module-build
- whitelist:
  - `asset_search`, `asset_dependencies`, `asset_referencers`, `blueprint_query`, `get_ue_context`
- graylist:
  - `get_output_log`
- avoid:
  - direct mutation tools unless explicitly requested
