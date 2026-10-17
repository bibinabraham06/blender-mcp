# Tasks — blender-mcp

## Now
- Keep dependencies and Blender addon synced with upstream releases.

## Next
- Compare official / bridge / fork on a real task; keep or remove each. Remove unused addons from Blender.
- Set `DISABLE_TELEMETRY=true` (and untick Allow Telemetry in the addon) for every client that launches the PyPI server, incl. `~/.gemini/config/mcp_config.json`; consider pinning it or pointing it at a reviewed clone instead of `uvx blender-mcp`.
- Enable `BLENDER_MCP_SAFE_MODE=1` where code execution is used; port the upstream #365 safe-mode fix (`script_directories`, `bpy.ops.extensions.*`) into `safe_mode.py`.
- Verify integration tests with local Blender instance.

## Later
- Custom tooling for automated CAD/floorplan model generation pipelines.

## Blocked

## Completed
- Audited fork history against the 2026-08-10 upstream compromise window (2026-10-17): no malicious changes found; see DECISIONS.md.
- Initial repo clone and setup.
- Initialized workspace standardization governance files.
