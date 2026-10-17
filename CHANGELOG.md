# Changelog — blender-mcp

Most recent first.

## 2026-10-17
- Safe mode: block `script_directories` (any position) and `bpy.ops.extensions.*`; added 4 regression tests (73 pass).
- Installed official Blender Lab MCP addon (`mcp` 1.0.0, port 9877) and registered `blender-official` MCP (local scope).
- Installed seehiong `blender-mcp-bridge` addon (127.0.0.1:8888) and registered `blender-bridge` MCP (http://127.0.0.1:8008/mcp, local scope).
- Audited fork history post-2026-08-03 and the cached PyPI `mcp-for-blender` 2.1.9 code; results in DECISIONS.md.
- Installed `blender-kiln` plugin, `gltf-transform` and `gltfpack` (global npm).

## 2026-09-04
- Added workspace governance files (`AGENTS.md`, `CLAUDE.md`, `TASKS.md`, `DECISIONS.md`, `CHANGELOG.md`).
