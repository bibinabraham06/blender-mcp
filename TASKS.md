# Tasks — blender-mcp

## Now
- Keep dependencies and Blender addon synced with upstream releases.

## Next
- Compare official / bridge / fork on a real task; keep or remove each. Remove unused addons from Blender.
- Untick Allow Telemetry in the Blender addon prefs GUI.
- Rotate the plaintext GitHub token and Tailscale bearer token stored in `~/.gemini/config/mcp_config.json` (exposed in a session transcript 2026-10-17) and move them out of the file.
- Verify integration tests with local Blender instance.

## Later
- Custom tooling for automated CAD/floorplan model generation pipelines.

## Blocked

## Completed
- Pinned `~/.gemini/config/mcp_config.json` `blender` entry to the local audited clone (`uv --directory /Users/bibin/Developer/Projects/blender-mcp run blender-mcp`) with `BLENDER_MCP_SAFE_MODE=1` and `DISABLE_TELEMETRY=true` (2026-10-17).
- Installed and verified `@casys/mcp-build123d`, `bonsai-mcp` (+ Blender `bonsai` BIM extension), `ncmlabs/fusion360_mcp`, `DMontgomery40/bambu-printer-mcp@1.1.11`, `mesh-ai-mcp` (CoACD segmentation + `pyacvd`/Quadriflow retopology + cloud Space bridge), `ComfyUI-Blender` + `comfyui-mcp-server`, `3dgs-render-blender-addon`, and `lidarlink_blender` (2026-10-17).
- Ported #365 safe-mode fix (`script_directories`, `bpy.ops.extensions.*`) with regression tests; set `BLENDER_MCP_SAFE_MODE=1` and `DISABLE_TELEMETRY=true` for the Gemini/Antigravity blender entry (2026-10-17). Codex has no blender MCP configured.
- Audited fork history against the 2026-08-10 upstream compromise window (2026-10-17): no malicious changes found; see DECISIONS.md.
- Initial repo clone and setup.
- Initialized workspace standardization governance files.
