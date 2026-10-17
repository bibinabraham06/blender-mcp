# Changelog — blender-mcp

Most recent first.

## 2026-10-17
- Completed cross-project 3D/CAD audit: pinned Antigravity `~/.gemini/config/mcp_config.json` `blender` entry to local clone with `BLENDER_MCP_SAFE_MODE=1` and `DISABLE_TELEMETRY=true`; wired project-scoped MCP servers (`build123d`, `bonsai-mcp`, `mesh-ai-mcp`, `3mf`, `blender-official`) across all 3D/CAD/woodworking repos; added Quadriflow retopology to `iphone-lidar/scripts/cleanup_mesh.py`; removed retired `baby-stroller-handle` and `joie-stroller-clamp`.
- Installed `@casys/mcp-build123d` (`~/.local/share/mcp-build123d`, Python 3.12 venv + Deno stdio) and registered `build123d` MCP (local scope).
- Installed Blender 5.2 `bonsai` BIM extension (`bl_ext.blender_org.bonsai`) and `JotaDeRodriguez/Bonsai_mcp` (`~/.local/share/bonsai-mcp`, addon `bonsai_mcp_addon.py` patched to port 9878 / `bonsaimcp.*` IDs); registered `bonsai-mcp` (local scope).
- Installed `ncmlabs/fusion360_mcp` (`~/.local/share/fusion360-mcp` + `FusionMCP` AddIn on port 5001) and registered `fusion360-ncmlabs` in `Projects/fusion360` (local scope).
- Installed `DMontgomery40/bambu-printer-mcp@1.1.11` (`~/.local/share/bambu-printer-mcp`, includes native macOS OrcaSlicer CLI integration) and registered `bambu-printer-mcp` in `Projects/bambulab-x2d` (local scope).
- Created and verified `mesh-ai-mcp` (`~/.local/share/mesh-ai-mcp`: CoACD part segmentation, `pyacvd` Voronoi & Blender Quadriflow auto-retopology, plus `PartField`/`SAMPart3D`/`MeshAnythingV2` Gradio cloud bridge); registered `mesh-ai-mcp` (local scope).
- Installed `alexisrolland/ComfyUI-Blender` addon (`comfyui_blender` 4.5.1) into Blender 5.2 and `joenorton/comfyui-mcp-server` (`~/.local/share/comfyui-mcp-server`); registered `comfyui-mcp` (local scope).
- Installed `Kiri-Innovation/3dgs-render-blender-addon` (5.1.0) and `lidarlink_blender.py` into Blender 5.2 addons.
- Safe mode: block `script_directories` (any position) and `bpy.ops.extensions.*`; added 4 regression tests (73 pass).
- Installed official Blender Lab MCP addon (`mcp` 1.0.0, port 9877) and registered `blender-official` MCP (local scope).
- Installed seehiong `blender-mcp-bridge` addon (127.0.0.1:8888) and registered `blender-bridge` MCP (http://127.0.0.1:8008/mcp, local scope).
- Audited fork history post-2026-08-03 and the cached PyPI `mcp-for-blender` 2.1.9 code; results in DECISIONS.md.
- Installed `blender-kiln` plugin, `gltf-transform` and `gltfpack` (global npm).

## 2026-09-04
- Added workspace governance files (`AGENTS.md`, `CLAUDE.md`, `TASKS.md`, `DECISIONS.md`, `CHANGELOG.md`).
